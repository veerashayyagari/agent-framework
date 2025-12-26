# Troubleshooting and Debugging Guide

This guide helps developers diagnose and resolve common issues when working with the .NET Agent Framework. It covers debugging techniques, common error patterns, logging configuration, telemetry setup, and provider-specific troubleshooting.

---

## Table of Contents

1. [Debugging Basics](#debugging-basics)
2. [Common Errors and Solutions](#common-errors-and-solutions)
3. [Logging Configuration](#logging-configuration)
4. [Telemetry and Tracing](#telemetry-and-tracing)
5. [Inspecting Agent State](#inspecting-agent-state)
6. [Provider-Specific Debugging](#provider-specific-debugging)
7. [Performance Diagnostics](#performance-diagnostics)

---

## Debugging Basics

### Enabling Verbose Logging

The framework uses Microsoft.Extensions.Logging throughout. To enable verbose logging:

```csharp
using Microsoft.Extensions.Logging;

// Create a logger factory with Debug/Trace level
var loggerFactory = LoggerFactory.Create(builder =>
{
    builder
        .SetMinimumLevel(LogLevel.Trace)  // Most verbose
        .AddConsole()
        .AddDebug();
});

// Pass to agent creation
var agent = chatClient.CreateAIAgent(
    "MyAgent",
    "You are a helpful assistant",
    loggerFactory: loggerFactory
);
```

**Log Level Guidelines**:

| Level | What It Shows |
|-------|---------------|
| `Trace` | Full message content, options, responses (sensitive data) |
| `Debug` | Method invocations and completions |
| `Information` | High-level agent operations |
| `Warning` | Recoverable issues, deprecations |
| `Error` | Failures with exception details |

**Warning**: `LogLevel.Trace` logs sensitive data including message content, API keys in headers, and response payloads. Never enable in production.

### Attaching a Debugger to Agent Execution

Set breakpoints in the middleware pipeline to inspect execution:

```csharp
var agent = baseAgent.AsBuilder()
    .Use((messages, thread, options, next, cancellationToken) =>
    {
        // Set breakpoint here to inspect:
        // - messages: Input messages to the agent
        // - thread: Current conversation state
        // - options: Run options including tools

        return next(messages, thread, options, cancellationToken);
    })
    .Build();
```

For deeper debugging, set breakpoints in:
- `ChatClientAgent.RunAsync` - Entry point for all agent invocations
- `ChatClientAgent.PrepareThreadAndMessagesAsync` - Thread and message preparation
- `LoggingAgent.RunAsync` - If using logging middleware

### Inspecting the Middleware Pipeline

The framework uses a decorator pattern. To understand the pipeline:

```csharp
// Inspect the pipeline by examining services
var agent = baseAgent.AsBuilder()
    .Use(inner => new LoggingAgent(inner, logger))
    .Use(inner => new OpenTelemetryAgent(inner))
    .Build();

// Get metadata about the wrapped agent
var metadata = agent.GetService<AIAgentMetadata>();
Console.WriteLine($"Provider: {metadata?.ProviderName}");

// Get the underlying chat client
var chatClient = agent.GetService<IChatClient>();
Console.WriteLine($"ChatClient type: {chatClient?.GetType().Name}");
```

---

## Common Errors and Solutions

### 1. Authentication Failures

**Error Message**:
```
System.ClientModel.ClientResultException: HTTP 401 (Unauthorized)
```
or
```
Azure.RequestFailedException: The API key is invalid or expired
```

**Likely Causes**:
- Missing or invalid API key
- Expired credentials or tokens
- Wrong endpoint URL
- Incorrect Azure AD scope

**Solutions**:

```csharp
// OpenAI - Check API key is set
var apiKey = Environment.GetEnvironmentVariable("OPENAI_API_KEY")
    ?? throw new InvalidOperationException("OPENAI_API_KEY environment variable is required");

// Azure OpenAI - Verify endpoint and key
var endpoint = new Uri(Environment.GetEnvironmentVariable("AZURE_OPENAI_ENDPOINT")!);
var credential = new AzureKeyCredential(Environment.GetEnvironmentVariable("AZURE_OPENAI_KEY")!);
var client = new AzureOpenAIClient(endpoint, credential);

// For Azure AD authentication, ensure correct scope
var tokenCredential = new DefaultAzureCredential();
// Scope for Azure OpenAI: "https://cognitiveservices.azure.com/.default"
```

**Anthropic-specific**:
```csharp
// Ensure API key is properly set
var client = new AnthropicClient()
{
    APIKey = Environment.GetEnvironmentVariable("ANTHROPIC_API_KEY")
        ?? throw new InvalidOperationException("ANTHROPIC_API_KEY is required")
};
```

### 2. Rate Limiting Errors

**Error Message**:
```
PurviewRateLimitException: Rate limit exceeded
```
or
```
HTTP 429 (Too Many Requests)
```

**Likely Causes**:
- Exceeding tokens per minute (TPM) quota
- Exceeding requests per minute (RPM) quota
- Concurrent request limits

**Solutions**:

```csharp
// Implement exponential backoff retry
var retryPolicy = new RetryPolicy(
    maxRetries: 3,
    delay: TimeSpan.FromSeconds(1),
    maxDelay: TimeSpan.FromSeconds(30),
    backoffMultiplier: 2
);

// Or use Polly for resilience
services.AddHttpClient("OpenAI")
    .AddPolicyHandler(Policy
        .Handle<HttpRequestException>()
        .OrResult<HttpResponseMessage>(r => r.StatusCode == HttpStatusCode.TooManyRequests)
        .WaitAndRetryAsync(3, retryAttempt =>
            TimeSpan.FromSeconds(Math.Pow(2, retryAttempt))));
```

### 3. Thread Incompatibility Errors

**Error Message**:
```
InvalidOperationException: The provided thread is not compatible with the agent.
Only threads created by the agent can be used.
```

**Likely Causes**:
- Using a thread created by a different agent
- Mixing server-managed and client-managed threads
- Deserializing a thread with the wrong agent

**Solutions**:

```csharp
// Always create threads from the specific agent
var agent = chatClient.CreateAIAgent("MyAgent", "instructions");
var thread = agent.GetNewThread();  // Correct

// For server-managed threads (Azure AI, Copilot Studio)
var thread = agent.GetNewThread(conversationId: "existing-conversation-id");

// For client-managed threads with custom storage
var thread = agent.GetNewThread(new InMemoryChatMessageStore());

// Serialize and deserialize with the same agent
var serialized = thread.Serialize();
var restored = agent.DeserializeThread(serialized);  // Use same agent!
```

### 4. Service-Managed Thread Errors

**Error Message**:
```
InvalidOperationException: Service did not return a valid conversation id
when using a service managed thread.
```

**Likely Causes**:
- Using a conversation ID with a provider that doesn't support server-side storage
- Provider doesn't return conversation IDs

**Solutions**:

```csharp
// For OpenAI Chat Completions (no server storage), use client-managed threads
var thread = agent.GetNewThread(new InMemoryChatMessageStore());

// For Azure AI Foundry or OpenAI Responses API (server storage), use conversation IDs
var thread = agent.GetNewThread(conversationId: "conv-123");
```

### 5. Tool/Function Invocation Failures

**Error Message**:
```
NotSupportedException: Function Invocation Middleware is only supported
without options or with ChatClientAgentRunOptions.
```

**Likely Causes**:
- Wrong options type when using function middleware
- Tool method signature issues
- Missing service provider for DI-based tools

**Solutions**:

```csharp
// Use correct options type for function invocation middleware
var options = new ChatClientAgentRunOptions
{
    ChatOptions = new ChatOptions
    {
        Tools = [AIFunctionFactory.Create(MyToolMethod)]
    }
};

// Ensure service provider is passed for DI-dependent tools
var agent = chatClient.CreateAIAgent(
    "MyAgent",
    "instructions",
    tools: tools,
    services: serviceProvider  // Required for DI in tools
);

// Tool method signature must be correct
[Description("Gets the current weather")]
public static string GetWeather(
    [Description("City name")] string city,
    [Description("Temperature unit")] string unit = "celsius")
{
    return $"Weather in {city}: 22{unit[0].ToString().ToUpper()}";
}
```

### 6. Continuation Token Errors

**Error Message**:
```
InvalidOperationException: A thread must be provided when continuing
a background response with a continuation token.
```
or
```
InvalidOperationException: Input messages are not allowed when continuing
a background response using a continuation token.
```
or
```
InvalidOperationException: Continuation tokens are not allowed to be used
for initial runs.
```

**Likely Causes**:
- Missing thread when using background responses
- Providing new messages when resuming with a continuation token
- Using continuation token on first run

**Solutions**:

```csharp
// Initial request with background responses
var thread = agent.GetNewThread();  // Thread required!
var options = new AgentRunOptions { AllowBackgroundResponses = true };
var response = await agent.RunAsync(messages, thread, options);

// Resume with continuation token
if (response.ContinuationToken != null)
{
    var resumeOptions = new AgentRunOptions
    {
        ContinuationToken = response.ContinuationToken
    };

    // Note: No new messages when resuming!
    var nextResponse = await agent.RunAsync(
        Array.Empty<ChatMessage>(),  // Empty messages
        thread,  // Same thread
        resumeOptions);
}
```

### 7. Streaming Resumption Errors

**Error Message**:
```
NotSupportedException: Streaming resumption is only supported when chat history
is stored and managed by the underlying AI service.
```
or
```
NotSupportedException: Using context provider with streaming resumption is not supported.
```

**Likely Causes**:
- Using streaming resumption with client-managed threads
- Using streaming resumption with AIContextProvider

**Solutions**:

```csharp
// Streaming resumption requires server-managed threads
var thread = agent.GetNewThread(conversationId: "conv-123");

// Don't use AIContextProvider when resuming streams
var options = new ChatClientAgentOptions
{
    // Remove AIContextProviderFactory when using streaming resumption
    // AIContextProviderFactory = ...  // Comment out
};
```

### 8. Agent Not Registered (DurableTask)

**Error Message**:
```
AgentNotRegisteredException: No agent named 'MyAgent' was registered.
Ensure the agent is registered using ConfigureDurableAgents before using it in an orchestration.
```

**Likely Causes**:
- Agent not registered in DI container
- Agent name mismatch
- Registration order issues

**Solutions**:

```csharp
// Register agents before using in orchestrations
services.ConfigureDurableAgents(config =>
{
    config.RegisterAgent("MyAgent", (sp, name) =>
    {
        var chatClient = sp.GetRequiredService<IChatClient>();
        return chatClient.CreateAIAgent(name, "instructions");
    });
});

// Ensure name matches exactly when invoking
var agentProxy = context.CreateAgentProxy("MyAgent");  // Case-sensitive!
```

### 9. Workflow/Declarative Exceptions

**Error Message**:
```
DeclarativeWorkflowException: Error executing workflow step 'StepName'
DeclarativeActionException: Action 'ActionName' failed to execute
DeclarativeModelException: Invalid model configuration
```

**Likely Causes**:
- Invalid YAML/JSON workflow definition
- Missing action handler
- Invalid model reference

**Solutions**:

```csharp
// Validate workflow schema before loading
try
{
    var workflow = WorkflowLoader.Load("workflow.yaml");
}
catch (DeclarativeWorkflowException ex)
{
    Console.WriteLine($"Workflow error: {ex.Message}");
    // Check workflow.yaml for schema issues
}

// Ensure all actions are registered
services.AddWorkflowAction<MyCustomAction>("my-action");
```

### 10. Conversation ID Conflict

**Error Message**:
```
InvalidOperationException: The ConversationId provided via ChatOptions is different
to the id of the provided AgentThread. Only one id can be used for a run.
```

**Likely Causes**:
- Thread has one conversation ID, options specify another

**Solutions**:

```csharp
// Use one source for conversation ID
var thread = agent.GetNewThread("conv-123");

// Don't override in options
var options = new ChatClientAgentRunOptions
{
    ChatOptions = new ChatOptions
    {
        // ConversationId = "different-id"  // Don't do this!
    }
};
```

---

## Logging Configuration

### How LoggingAgent Works

`LoggingAgent` is a delegating agent that wraps another agent and logs all operations:

```csharp
// LoggingAgent wraps the inner agent
public sealed partial class LoggingAgent : DelegatingAIAgent
{
    private readonly ILogger _logger;

    public LoggingAgent(AIAgent innerAgent, ILogger logger)
        : base(innerAgent)
    {
        _logger = logger;
    }
}
```

**Logged Events**:
- `Debug`: Method invocations (`RunAsync invoked`, `RunStreamingAsync invoked`)
- `Trace`: Full message content, options, metadata, and responses (sensitive!)
- `Debug`: Completions (`RunAsync completed`)
- `Error`: Failures with exception details
- `Debug`: Cancellations

### Log Levels and What They Show

```csharp
// Debug level - shows invocation flow without sensitive data
[LoggerMessage(LogLevel.Debug, "{MethodName} invoked.")]
private partial void LogInvoked(string methodName);

// Trace level - shows full sensitive data (NEVER in production!)
[LoggerMessage(LogLevel.Trace, "{MethodName} invoked: {Messages}. Options: {Options}. Metadata: {Metadata}.")]
private partial void LogInvokedSensitive(string methodName, string messages, string options, string metadata);

// Error level - shows failures
[LoggerMessage(LogLevel.Error, "{MethodName} failed.")]
private partial void LogInvocationFailed(string methodName, Exception error);
```

### Configuring ILoggerFactory

**Console Application**:
```csharp
using Microsoft.Extensions.Logging;

var loggerFactory = LoggerFactory.Create(builder =>
{
    builder
        .SetMinimumLevel(LogLevel.Debug)
        .AddConsole(options =>
        {
            options.IncludeScopes = true;
            options.TimestampFormat = "HH:mm:ss ";
        });
});

var agent = baseAgent.AsBuilder()
    .UseLogging(loggerFactory)
    .Build();
```

**ASP.NET Core / Hosted Application**:
```csharp
// In Program.cs or Startup.cs
builder.Services.AddLogging(logging =>
{
    logging.AddConsole();
    logging.AddDebug();
    logging.SetMinimumLevel(LogLevel.Debug);

    // Filter specific categories
    logging.AddFilter("Microsoft.Agents.AI", LogLevel.Trace);
    logging.AddFilter("Microsoft.Extensions.AI", LogLevel.Debug);
});

// Agent automatically uses ILoggerFactory from DI
builder.Services.AddAIAgent("MyAgent", (sp, name) =>
{
    var loggerFactory = sp.GetRequiredService<ILoggerFactory>();
    return chatClient.CreateAIAgent(name, "instructions", loggerFactory: loggerFactory);
});
```

### Sample Logging Setup

```csharp
using Microsoft.Extensions.Logging;
using Microsoft.Agents.AI;

// Complete logging setup example
var loggerFactory = LoggerFactory.Create(builder =>
{
    builder
        // Set base level
        .SetMinimumLevel(LogLevel.Information)

        // Verbose for agent operations (for debugging)
        .AddFilter("Microsoft.Agents.AI", LogLevel.Debug)
        .AddFilter("Microsoft.Agents.AI.LoggingAgent", LogLevel.Trace)  // Full content

        // Console output
        .AddConsole(options =>
        {
            options.TimestampFormat = "[yyyy-MM-dd HH:mm:ss.fff] ";
        })

        // Optional: File logging via Serilog
        // .AddSerilog(new LoggerConfiguration()
        //     .WriteTo.File("agent-logs.txt", rollingInterval: RollingInterval.Day)
        //     .CreateLogger())
});

var logger = loggerFactory.CreateLogger<LoggingAgent>();

var agent = baseAgent.AsBuilder()
    .Use(inner => new LoggingAgent(inner, logger))
    .Build();
```

---

## Telemetry and Tracing

### How OpenTelemetryAgent Works

`OpenTelemetryAgent` implements the [OpenTelemetry Semantic Conventions for Generative AI systems v1.37](https://opentelemetry.io/docs/specs/semconv/gen-ai/):

```csharp
// OpenTelemetryAgent wraps the inner agent and delegates to OpenTelemetryChatClient
public sealed class OpenTelemetryAgent : DelegatingAIAgent, IDisposable
{
    private readonly OpenTelemetryChatClient _otelClient;

    public OpenTelemetryAgent(AIAgent innerAgent, string? sourceName = null)
        : base(innerAgent)
    {
        _otelClient = new OpenTelemetryChatClient(
            new ForwardingChatClient(this),
            sourceName: sourceName ?? "Experimental.Microsoft.Agents.AI");
    }
}
```

### Setting Up Traces

```csharp
using OpenTelemetry;
using OpenTelemetry.Trace;
using Microsoft.Agents.AI;

// Configure OpenTelemetry
var tracerProvider = Sdk.CreateTracerProviderBuilder()
    .SetResourceBuilder(ResourceBuilder.CreateDefault()
        .AddService("MyAgentService"))

    // Add the agent source
    .AddSource("Experimental.Microsoft.Agents.AI")

    // Export to Jaeger
    .AddJaegerExporter(options =>
    {
        options.AgentHost = "localhost";
        options.AgentPort = 6831;
    })

    // Or export to Zipkin
    // .AddZipkinExporter(options =>
    // {
    //     options.Endpoint = new Uri("http://localhost:9411/api/v2/spans");
    // })

    // Or export to Azure Monitor (Application Insights)
    // .AddAzureMonitorTraceExporter(options =>
    // {
    //     options.ConnectionString = "InstrumentationKey=...";
    // })

    .Build();

// Add telemetry to agent
var agent = baseAgent.AsBuilder()
    .UseOpenTelemetry()  // Uses default source name
    // Or with custom source name:
    // .UseOpenTelemetry(sourceName: "MyApp.Agents")
    .Build();
```

### Captured Spans and Attributes

The agent creates spans with the following attributes:

| Attribute | Description | Example |
|-----------|-------------|---------|
| `gen_ai.operation.name` | Operation type | `invoke_agent` |
| `gen_ai.agent.id` | Agent unique identifier | `agent-123` |
| `gen_ai.agent.name` | Agent display name | `MyAssistant` |
| `gen_ai.agent.description` | Agent description | `A helpful assistant` |
| `gen_ai.provider.name` | AI provider name | `openai`, `anthropic` |

**Span Name Format**: `invoke_agent {AgentName}({AgentId})`

### Enabling Sensitive Data in Telemetry

By default, message content is not captured. To enable (for debugging only):

```csharp
// Via property
var otelAgent = new OpenTelemetryAgent(innerAgent);
otelAgent.EnableSensitiveData = true;

// Or via environment variable
Environment.SetEnvironmentVariable(
    "OTEL_INSTRUMENTATION_GENAI_CAPTURE_MESSAGE_CONTENT",
    "true");
```

### Viewing Traces

**Jaeger** (http://localhost:16686):
```bash
docker run -d --name jaeger \
  -p 16686:16686 \
  -p 6831:6831/udp \
  jaegertracing/all-in-one:latest
```

**Zipkin** (http://localhost:9411):
```bash
docker run -d -p 9411:9411 openzipkin/zipkin
```

**Azure Application Insights**:
```csharp
services.AddOpenTelemetry()
    .WithTracing(builder => builder
        .AddSource("Experimental.Microsoft.Agents.AI")
        .AddAzureMonitorTraceExporter());
```

---

## Inspecting Agent State

### Accessing Thread State

```csharp
// Get thread from agent
var thread = agent.GetNewThread();

// After running, inspect thread services
if (thread is ChatClientAgentThread chatThread)
{
    // Get conversation ID (for server-managed threads)
    Console.WriteLine($"Conversation ID: {chatThread.ConversationId}");

    // Get message store (for client-managed threads)
    var messageStore = chatThread.MessageStore;
    if (messageStore != null)
    {
        var messages = await messageStore.GetMessagesAsync();
        Console.WriteLine($"Message count: {messages.Count}");
    }

    // Get context provider state
    var contextProvider = chatThread.AIContextProvider;
}
```

### Examining Message History

```csharp
// For InMemoryAgentThread-based threads
if (thread is InMemoryAgentThread inMemoryThread)
{
    // Access messages directly
    foreach (var message in inMemoryThread.MessageStore)
    {
        Console.WriteLine($"[{message.Role}] {message.AuthorName}: {message.Text}");

        // Inspect tool calls
        foreach (var content in message.Contents.OfType<FunctionCallContent>())
        {
            Console.WriteLine($"  Tool call: {content.Name}({content.Arguments})");
        }

        // Inspect tool results
        foreach (var content in message.Contents.OfType<FunctionResultContent>())
        {
            Console.WriteLine($"  Tool result: {content.Name} = {content.Result}");
        }
    }
}

// For generic ChatMessageStore
var store = thread.GetService<ChatMessageStore>();
if (store != null)
{
    var history = await store.GetMessagesAsync();
    foreach (var msg in history)
    {
        Console.WriteLine($"{msg.Role}: {msg.Text?.Substring(0, Math.Min(100, msg.Text.Length ?? 0))}...");
    }
}
```

### Debugging Context Providers

```csharp
// Create a debugging context provider wrapper
public class DebugContextProvider : AIContextProvider
{
    private readonly AIContextProvider _inner;
    private readonly ILogger _logger;

    public DebugContextProvider(AIContextProvider inner, ILogger logger)
    {
        _inner = inner;
        _logger = logger;
    }

    public override async Task<AIContext> InvokingAsync(
        InvokingContext context,
        CancellationToken cancellationToken)
    {
        _logger.LogDebug("Context provider invoked with {MessageCount} input messages",
            context.InputMessages.Count());

        var result = await _inner.InvokingAsync(context, cancellationToken);

        _logger.LogDebug("Context provider returned: {ToolCount} tools, {MessageCount} messages",
            result.Tools?.Count ?? 0,
            result.Messages?.Count ?? 0);

        if (result.Instructions != null)
        {
            _logger.LogTrace("Additional instructions: {Instructions}", result.Instructions);
        }

        return result;
    }

    public override async Task InvokedAsync(
        InvokedContext context,
        CancellationToken cancellationToken)
    {
        if (context.InvokeException != null)
        {
            _logger.LogError(context.InvokeException, "Context provider notified of failure");
        }
        else
        {
            _logger.LogDebug("Context provider notified of success with {ResponseCount} response messages",
                context.ResponseMessages?.Count() ?? 0);
        }

        await _inner.InvokedAsync(context, cancellationToken);
    }
}
```

### Serializing Thread for Debugging

```csharp
// Serialize thread state for inspection
var serialized = thread.Serialize();
var json = JsonSerializer.Serialize(serialized, new JsonSerializerOptions
{
    WriteIndented = true
});
Console.WriteLine(json);

// Save to file for analysis
await File.WriteAllTextAsync("thread-state.json", json);
```

---

## Provider-Specific Debugging

### OpenAI

**Common Issues**:

1. **Invalid API Key**
   ```
   HTTP 401: Incorrect API key provided
   ```
   - Verify `OPENAI_API_KEY` environment variable
   - Check key hasn't been rotated or revoked
   - Ensure key has required permissions

2. **Model Not Found**
   ```
   HTTP 404: The model 'gpt-5' does not exist
   ```
   - Check model name spelling (`gpt-4o`, `gpt-4o-mini`, etc.)
   - Verify your account has access to the model

3. **Context Length Exceeded**
   ```
   HTTP 400: maximum context length exceeded
   ```
   - Reduce message history
   - Use a model with larger context window
   - Implement chat history summarization

**Debugging Setup**:
```csharp
var client = new OpenAIClient(apiKey);

// Enable HTTP logging (for debugging only)
var httpClient = new HttpClient(new LoggingHandler(new HttpClientHandler()));
// Pass custom HttpClient to OpenAI client
```

### Azure OpenAI

**Common Issues**:

1. **Endpoint Configuration**
   ```
   HTTP 404: Resource not found
   ```
   - Verify endpoint format: `https://{resource-name}.openai.azure.com`
   - Check deployment name matches exactly

2. **Deployment Not Ready**
   ```
   HTTP 400: The deployment is not ready
   ```
   - Wait for deployment to complete in Azure Portal
   - Check deployment status via Azure CLI

3. **Region Availability**
   - Some models only available in specific regions
   - Check [Azure OpenAI model availability](https://learn.microsoft.com/azure/ai-services/openai/concepts/models)

**Debugging Setup**:
```csharp
var endpoint = new Uri(Environment.GetEnvironmentVariable("AZURE_OPENAI_ENDPOINT")!);
var credential = new AzureKeyCredential(
    Environment.GetEnvironmentVariable("AZURE_OPENAI_KEY")!);

var client = new AzureOpenAIClient(endpoint, credential);

// Verify connectivity
try
{
    var deployments = await client.GetChatClient("deployment-name")
        .CompleteChatAsync([new UserChatMessage("test")]);
}
catch (Azure.RequestFailedException ex)
{
    Console.WriteLine($"Azure error: {ex.Status} - {ex.ErrorCode}");
    Console.WriteLine($"Message: {ex.Message}");
}
```

### Anthropic

**Common Issues**:

1. **Missing API Key**
   ```csharp
   throw new InvalidOperationException("ANTHROPIC_API_KEY is required")
   ```
   - Set `ANTHROPIC_API_KEY` environment variable
   - For Azure-hosted Anthropic, use `ANTHROPIC_RESOURCE` instead

2. **Max Tokens Required**
   ```
   HTTP 400: max_tokens is required
   ```
   - Anthropic requires explicit `max_tokens` setting
   - Default is set to 4096 in the framework

3. **Model Name Format**
   - Use format: `claude-3-5-sonnet-20241022`, `claude-haiku-4-5`
   - Check [Anthropic model documentation](https://docs.anthropic.com/claude/docs/models-overview)

**Debugging Setup**:
```csharp
// For direct Anthropic API
var client = new AnthropicClient()
{
    APIKey = Environment.GetEnvironmentVariable("ANTHROPIC_API_KEY")!
};

// For Azure-hosted Anthropic (AI Foundry)
var resource = Environment.GetEnvironmentVariable("ANTHROPIC_RESOURCE")!;
var apiKey = Environment.GetEnvironmentVariable("ANTHROPIC_API_KEY")!;
var client = new AnthropicFoundryClient(
    new AnthropicFoundryApiKeyCredentials(apiKey, resource));

// Test connectivity
try
{
    var agent = client.CreateAIAgent("claude-haiku-4-5", "test");
    var response = await agent.RunAsync("Hello");
}
catch (Exception ex)
{
    Console.WriteLine($"Anthropic error: {ex.GetType().Name} - {ex.Message}");
}
```

### Copilot Studio

**Common Issues**:

1. **Bot ID Not Found**
   - Verify bot ID from Copilot Studio portal
   - Ensure bot is published

2. **Authentication Scope**
   - For Azure AD auth, use scope: `https://api.powerplatform.com/.default`
   - Check tenant configuration

3. **Region Mismatch**
   - Ensure connecting to correct Power Platform region

---

## Performance Diagnostics

### Measuring Agent Response Time

```csharp
using System.Diagnostics;

var stopwatch = Stopwatch.StartNew();
var response = await agent.RunAsync(messages, thread);
stopwatch.Stop();

Console.WriteLine($"Agent response time: {stopwatch.ElapsedMilliseconds}ms");
Console.WriteLine($"Token usage: {response.GetService<UsageContent>()?.TotalTokenCount}");
```

### Memory Usage Monitoring

```csharp
// Monitor memory during large conversations
var before = GC.GetTotalMemory(false);

var response = await agent.RunAsync(messages, thread);

var after = GC.GetTotalMemory(false);
Console.WriteLine($"Memory delta: {(after - before) / 1024:N0} KB");

// Force cleanup if needed
GC.Collect();
```

### Streaming Performance

```csharp
var firstTokenReceived = false;
var stopwatch = Stopwatch.StartNew();

await foreach (var update in agent.RunStreamingAsync(messages, thread))
{
    if (!firstTokenReceived)
    {
        Console.WriteLine($"Time to first token: {stopwatch.ElapsedMilliseconds}ms");
        firstTokenReceived = true;
    }

    // Process update...
}

Console.WriteLine($"Total streaming time: {stopwatch.ElapsedMilliseconds}ms");
```

---

## Quick Troubleshooting Checklist

When an agent isn't working as expected:

1. **Enable Debug Logging**
   ```csharp
   builder.SetMinimumLevel(LogLevel.Debug);
   ```

2. **Check Authentication**
   - Verify API keys/credentials are set
   - Check token expiration for Azure AD

3. **Verify Thread Compatibility**
   - Use threads from the same agent
   - Check server-managed vs. client-managed requirements

4. **Inspect the Pipeline**
   - Use `GetService<T>()` to examine wrapped components
   - Add debugging middleware

5. **Check Provider Status**
   - OpenAI: https://status.openai.com
   - Azure: https://status.azure.com

6. **Enable OpenTelemetry**
   - View spans and timing
   - Check for error attributes

7. **Review Exception Details**
   - Inner exceptions often contain provider-specific errors
   - HTTP status codes indicate the category of issue

---

*Last updated: 2025-12-26*
