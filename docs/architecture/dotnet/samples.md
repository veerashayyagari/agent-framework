# .NET Agent Framework Samples Guide

This document provides comprehensive guidance for all samples in the .NET Agent Framework. Use it to find the right sample for your learning goals and understand what each sample demonstrates.

---

## Overview

The Agent Framework samples are organized into **progressive learning paths** that help developers:

1. **Get Started** - Build your first agent with minimal configuration
2. **Explore Providers** - Learn how to use different AI providers (OpenAI, Azure, Anthropic, etc.)
3. **Add Capabilities** - Integrate tools, memory, RAG, and workflows
4. **Build for Production** - Understand hosting, observability, and protocol integration

### Sample Organization

```
dotnet/samples/
├── GettingStarted/           # Step-by-step learning samples
│   ├── Agents/               # Core agent concepts (19 progressive steps)
│   ├── FoundryAgents/        # Azure Foundry-specific samples (15 steps)
│   ├── AgentProviders/       # Provider integration examples
│   ├── AgentWithMemory/      # Memory integration patterns
│   ├── AgentWithRAG/         # Retrieval-augmented generation
│   ├── Workflows/            # Multi-agent orchestration
│   ├── ModelContextProtocol/ # MCP integration
│   ├── AGUI/                 # AG-UI protocol samples
│   ├── A2A/                  # Agent-to-Agent protocol samples
│   ├── AgentWithOpenAI/      # OpenAI SDK type integration
│   ├── AgentWithAnthropic/   # Anthropic Claude samples
│   ├── AgentOpenTelemetry/   # Observability demo
│   ├── DevUI/                # Developer UI tools
│   └── DeclarativeAgents/    # YAML/JSON-based agent definitions
├── AzureFunctions/           # Durable Functions hosting
├── HostedAgents/             # Production hosting patterns
├── A2AClientServer/          # Full A2A client/server demo
├── AGUIClientServer/         # Full AG-UI client/server demo
├── AGUIWebChat/              # Blazor-based chat UI
├── AgentWebChat/             # .NET Aspire web chat
├── M365Agent/                # Microsoft 365 SDK integration
└── Purview/                  # Microsoft Purview compliance
```

### Prerequisites (Common)

Most samples require:

| Requirement | Notes |
|-------------|-------|
| **.NET 10 SDK** | Some samples work with .NET 8.0+ |
| **Azure CLI** | For `DefaultAzureCredential` authentication |
| **Azure OpenAI** | Default AI provider for most samples |
| **IDE** | Visual Studio 2022, VS Code, or Rider |

Provider-specific samples may require additional API keys or resources.

---

## Learning Paths

### Path 1: New to Agent Framework

**Goal**: Understand core concepts and build your first agent

| Step | Sample | Concepts | Difficulty |
|------|--------|----------|------------|
| 1 | [Agent_Step01_Running](../../../dotnet/samples/GettingStarted/Agents/Agent_Step01_Running/) | Create and run a basic agent | Beginner |
| 2 | [Agent_Step02_MultiturnConversation](../../../dotnet/samples/GettingStarted/Agents/Agent_Step02_MultiturnConversation/) | Maintain conversation context | Beginner |
| 3 | [Agent_Step03_UsingFunctionTools](../../../dotnet/samples/GettingStarted/Agents/Agent_Step03_UsingFunctionTools/) | Add function calling | Beginner |
| 4 | [Agent_Step05_StructuredOutput](../../../dotnet/samples/GettingStarted/Agents/Agent_Step05_StructuredOutput/) | Get typed responses | Beginner |
| 5 | [Agent_Step09_DependencyInjection](../../../dotnet/samples/GettingStarted/Agents/Agent_Step09_DependencyInjection/) | Use with DI container | Intermediate |

### Path 2: Provider Integration

**Goal**: Learn how to use different AI providers

| Step | Sample | Provider | Difficulty |
|------|--------|----------|------------|
| 1 | [Agent_With_AzureOpenAIChatCompletion](../../../dotnet/samples/GettingStarted/AgentProviders/Agent_With_AzureOpenAIChatCompletion/) | Azure OpenAI | Beginner |
| 2 | [Agent_With_OpenAIChatCompletion](../../../dotnet/samples/GettingStarted/AgentProviders/Agent_With_OpenAIChatCompletion/) | OpenAI | Beginner |
| 3 | [Agent_With_Anthropic](../../../dotnet/samples/GettingStarted/AgentProviders/Agent_With_Anthropic/) | Anthropic Claude | Beginner |
| 4 | [Agent_With_Ollama](../../../dotnet/samples/GettingStarted/AgentProviders/Agent_With_Ollama/) | Ollama (local) | Beginner |
| 5 | [Agent_With_GoogleGemini](../../../dotnet/samples/GettingStarted/AgentProviders/Agent_With_GoogleGemini/) | Google Gemini | Beginner |
| 6 | [Agent_With_CustomImplementation](../../../dotnet/samples/GettingStarted/AgentProviders/Agent_With_CustomImplementation/) | Custom provider | Advanced |

### Path 3: Multi-Agent Workflows

**Goal**: Build orchestrated multi-agent systems

| Step | Sample | Concepts | Difficulty |
|------|--------|----------|------------|
| 1 | [01_ExecutorsAndEdges](../../../dotnet/samples/GettingStarted/Workflows/_Foundational/01_ExecutorsAndEdges/) | Basic executors and edges | Beginner |
| 2 | [02_Streaming](../../../dotnet/samples/GettingStarted/Workflows/_Foundational/02_Streaming/) | Event streaming | Beginner |
| 3 | [03_AgentsInWorkflows](../../../dotnet/samples/GettingStarted/Workflows/_Foundational/03_AgentsInWorkflows/) | Using agents as executors | Intermediate |
| 4 | [04_AgentWorkflowPatterns](../../../dotnet/samples/GettingStarted/Workflows/_Foundational/04_AgentWorkflowPatterns/) | Common workflow patterns | Intermediate |
| 5 | [Concurrent](../../../dotnet/samples/GettingStarted/Workflows/Concurrent/) | Fan-out/fan-in | Intermediate |
| 6 | [HumanInTheLoop](../../../dotnet/samples/GettingStarted/Workflows/HumanInTheLoop/) | Human approval flows | Advanced |

### Path 4: Production Deployment

**Goal**: Deploy agents to production environments

| Step | Sample | Concepts | Difficulty |
|------|--------|----------|------------|
| 1 | [AgentOpenTelemetry](../../../dotnet/samples/GettingStarted/AgentOpenTelemetry/) | Observability with OpenTelemetry | Intermediate |
| 2 | [01_SingleAgent](../../../dotnet/samples/AzureFunctions/01_SingleAgent/) | Azure Functions hosting | Intermediate |
| 3 | [02_AgentOrchestration_Chaining](../../../dotnet/samples/AzureFunctions/02_AgentOrchestration_Chaining/) | Durable orchestration | Intermediate |
| 4 | [DevUI_Step01_BasicUsage](../../../dotnet/samples/GettingStarted/DevUI/DevUI_Step01_BasicUsage/) | Developer debugging UI | Intermediate |
| 5 | [A2AClientServer](../../../dotnet/samples/A2AClientServer/) | A2A protocol integration | Advanced |

---

## Sample Catalog

### GettingStarted/Agents

Core agent concepts using Azure OpenAI ChatCompletion. These samples form the foundation for understanding the framework.

| Sample | Purpose | Key Concepts | Difficulty |
|--------|---------|--------------|------------|
| **Agent_Step01_Running** | Create and run a basic agent | `ChatClientAgent`, `RunAsync`, instructions | Beginner |
| **Agent_Step02_MultiturnConversation** | Implement multi-turn conversations | `AgentThread`, conversation state | Beginner |
| **Agent_Step03_UsingFunctionTools** | Add function tools to agents | `AIFunctionFactory`, tool calling | Beginner |
| **Agent_Step04_UsingFunctionToolsWithApprovals** | Human-in-the-loop tool approvals | `ApprovalRequiredAIFunction`, HITL | Intermediate |
| **Agent_Step05_StructuredOutput** | Get typed/structured responses | `RunAsync<T>()`, JSON schema | Beginner |
| **Agent_Step06_PersistedConversations** | Persist and reload conversations | `ChatMessageStore`, serialization | Intermediate |
| **Agent_Step07_3rdPartyThreadStorage** | Use external storage for threads | Custom storage providers | Intermediate |
| **Agent_Step08_Observability** | Add telemetry to agents | OpenTelemetry, logging | Intermediate |
| **Agent_Step09_DependencyInjection** | Integrate with DI container | `AddAIAgent`, keyed services | Intermediate |
| **Agent_Step10_AsMcpTool** | Expose agent as MCP tool | MCP server integration | Advanced |
| **Agent_Step11_UsingImages** | Multi-modal image input | Image content, vision models | Intermediate |
| **Agent_Step12_AsFunctionTool** | Expose agent as function tool | Agent composition | Advanced |
| **Agent_Step13_BackgroundResponsesWithToolsAndPersistence** | Background operations with state | Long-running operations, persistence | Advanced |
| **Agent_Step14_Middleware** | Use middleware pattern | `DelegatingAIAgent`, decorator pattern | Intermediate |
| **Agent_Step15_Plugins** | Use plugins with agents | Plugin architecture | Intermediate |
| **Agent_Step16_ChatReduction** | Reduce chat history size | Memory management, sliding window | Intermediate |
| **Agent_Step17_BackgroundResponses** | Async/background responses | Polling, resumption, continuation tokens | Advanced |
| **Agent_Step18_DeepResearch** | Deep research on complex topics | Research tools, multi-step reasoning | Advanced |
| **Agent_Step19_Declarative** | Declaratively define agents | YAML/JSON configuration | Intermediate |

**Prerequisites**: Azure OpenAI endpoint + deployment, Azure CLI authenticated

**How to Run**:
```powershell
cd dotnet/samples/GettingStarted/Agents/Agent_Step01_Running
$env:AZURE_OPENAI_ENDPOINT="https://your-resource.openai.azure.com/"
$env:AZURE_OPENAI_DEPLOYMENT_NAME="gpt-4o-mini"
dotnet run
```

---

### GettingStarted/FoundryAgents

Azure Foundry-specific agent samples with server-managed state, versioning, and advanced features.

| Sample | Purpose | Key Concepts | Difficulty |
|--------|---------|--------------|------------|
| **FoundryAgents_Step01.1_Basics** | Agent creation and versioning | Agent management, versions | Beginner |
| **FoundryAgents_Step01.2_Running** | Run a basic Foundry agent | `FoundryAgent`, basic invocation | Beginner |
| **FoundryAgents_Step02_MultiturnConversation** | Multi-turn with Foundry | Server-managed threads | Beginner |
| **FoundryAgents_Step03_UsingFunctionTools** | Function tools with Foundry | Tool integration | Beginner |
| **FoundryAgents_Step04_UsingFunctionToolsWithApprovals** | HITL approvals | Approval workflows | Intermediate |
| **FoundryAgents_Step05_StructuredOutput** | Structured output | JSON schema output | Beginner |
| **FoundryAgents_Step06_PersistedConversations** | Persist conversations | Server-side persistence | Intermediate |
| **FoundryAgents_Step07_Observability** | Telemetry integration | OpenTelemetry | Intermediate |
| **FoundryAgents_Step08_DependencyInjection** | DI integration | Service registration | Intermediate |
| **FoundryAgents_Step09_UsingMcpClientAsTools** | MCP client as tools | MCP integration | Advanced |
| **FoundryAgents_Step10_UsingImages** | Image input | Multi-modal | Intermediate |
| **FoundryAgents_Step11_AsFunctionTool** | Agent as function tool | Composition | Advanced |
| **FoundryAgents_Step12_Middleware** | Middleware pattern | Decorators | Intermediate |
| **FoundryAgents_Step13_Plugins** | Plugin architecture | Extensibility | Intermediate |
| **FoundryAgents_Step14_CodeInterpreter** | Code interpreter tool | Sandboxed code execution | Advanced |
| **FoundryAgents_Step15_ComputerUse** | Computer use capabilities | UI automation | Advanced |

**Prerequisites**: Azure Foundry project endpoint, Azure CLI authenticated

**How to Run**:
```powershell
cd dotnet/samples/GettingStarted/FoundryAgents/FoundryAgents_Step01.2_Running
$env:AZURE_FOUNDRY_PROJECT_ENDPOINT="https://your-foundry.services.ai.azure.com/api/projects/your-project"
dotnet run
```

---

### GettingStarted/AgentProviders

Demonstrates creating agents with various AI providers.

| Sample | Provider | Prerequisites | Difficulty |
|--------|----------|---------------|------------|
| **Agent_With_A2A** | A2A remote agent | Running A2A server | Advanced |
| **Agent_With_Anthropic** | Anthropic Claude | `ANTHROPIC_API_KEY` | Beginner |
| **Agent_With_AzureAIAgentsPersistent** | Azure AI Persistent | Azure AI project | Intermediate |
| **Agent_With_AzureAIProject** | Azure AI Project | Azure AI project | Intermediate |
| **Agent_With_AzureFoundryModel** | Azure Foundry Model | Foundry deployment | Intermediate |
| **Agent_With_AzureOpenAIChatCompletion** | Azure OpenAI Chat | Azure OpenAI | Beginner |
| **Agent_With_AzureOpenAIResponses** | Azure OpenAI Responses | Azure OpenAI | Beginner |
| **Agent_With_CustomImplementation** | Custom provider | None (local) | Advanced |
| **Agent_With_GoogleGemini** | Google Gemini | Google API key | Beginner |
| **Agent_With_Ollama** | Ollama (local) | Ollama installed | Beginner |
| **Agent_With_ONNX** | ONNX Runtime | ONNX model | Intermediate |
| **Agent_With_OpenAIAssistants** | OpenAI Assistants | OpenAI API key | Intermediate |
| **Agent_With_OpenAIChatCompletion** | OpenAI Chat | OpenAI API key | Beginner |
| **Agent_With_OpenAIResponses** | OpenAI Responses | OpenAI API key | Beginner |

**Note**: `Agent_With_OpenAIAssistants` uses the deprecated Assistants API. Consider using Responses API instead.

---

### GettingStarted/AgentWithMemory

Memory integration patterns for agents.

| Sample | Purpose | Key Concepts | Difficulty |
|--------|---------|--------------|------------|
| **AgentWithMemory_Step01_ChatHistoryMemory** | Remember previous conversations | `ChatHistoryMemoryProvider`, sliding window | Beginner |
| **AgentWithMemory_Step02_MemoryUsingMem0** | Mem0 service integration | External memory service, fact extraction | Intermediate |
| **AgentWithMemory_Step03_CustomMemory** | Custom memory implementation | `ChatMessageStore`, custom providers | Advanced |

**Prerequisites**: Mem0 service for Step02, Azure OpenAI for all

---

### GettingStarted/AgentWithRAG

Retrieval-Augmented Generation patterns.

| Sample | Purpose | Key Concepts | Difficulty |
|--------|---------|--------------|------------|
| **AgentWithRAG_Step01_BasicTextRAG** | Basic text RAG | Simple text retrieval | Beginner |
| **AgentWithRAG_Step02_CustomVectorStoreRAG** | Vector store with custom schema | Embeddings, vector search | Intermediate |
| **AgentWithRAG_Step03_CustomRAGDataSource** | Custom RAG data source | Custom retrieval logic | Advanced |
| **AgentWithRAG_Step04_FoundryServiceRAG** | Azure Foundry VectorStore | Foundry integration | Intermediate |

**Prerequisites**: Azure OpenAI, Foundry project for Step04

---

### GettingStarted/Workflows

Multi-agent workflow orchestration.

#### Foundational Samples (Start Here)

| Sample | Purpose | Key Concepts | Difficulty |
|--------|---------|--------------|------------|
| **_Foundational/01_ExecutorsAndEdges** | Minimal workflow | Executors, edges, basic graph | Beginner |
| **_Foundational/02_Streaming** | Event streaming | Stream events, real-time output | Beginner |
| **_Foundational/03_AgentsInWorkflows** | Agents as executors | Agent integration | Intermediate |
| **_Foundational/04_AgentWorkflowPatterns** | Common patterns | Sequential, concurrent patterns | Intermediate |
| **_Foundational/05_MultiModelService** | Multiple AI services | Service composition | Intermediate |
| **_Foundational/06_SubWorkflows** | Nested workflows | Hierarchical composition | Intermediate |
| **_Foundational/07_MixedWorkflowAgentsAndExecutors** | Mixed composition | Adapter patterns, type conversion | Advanced |
| **_Foundational/08_WriterCriticWorkflow** | Iterative refinement | Quality gates, feedback loops | Advanced |

#### Advanced Workflow Patterns

| Sample | Purpose | Key Concepts | Difficulty |
|--------|---------|--------------|------------|
| **Concurrent** | Fan-out/fan-in | Parallel processing | Intermediate |
| **Loop** | Looping patterns | Iteration, conditions | Intermediate |
| **SharedStates** | Shared state | State coordination | Intermediate |
| **ConditionalEdges/01_EdgeCondition** | Conditional routing | Dynamic edges | Intermediate |
| **ConditionalEdges/02_SwitchCase** | Switch-case routing | Multiple paths | Intermediate |
| **ConditionalEdges/03_MultiSelection** | Multi-selection | Multiple downstream executors | Advanced |
| **Checkpoint/CheckpointAndResume** | State checkpointing | Save/restore state | Advanced |
| **Checkpoint/CheckpointWithHumanInTheLoop** | HITL with checkpoints | Approval with state | Advanced |
| **HumanInTheLoop/HumanInTheLoopBasic** | Basic HITL | Input ports, external requests | Intermediate |
| **Declarative** | Declarative workflows | YAML definitions | Intermediate |
| **Visualization** | Workflow visualization | Mermaid diagrams | Beginner |
| **Agents/FoundryAgent** | Foundry in workflows | Azure Foundry integration | Intermediate |
| **Agents/CustomAgentExecutors** | Custom executors | Extension patterns | Advanced |
| **Agents/WorkflowAsAnAgent** | Workflow as agent | Encapsulation | Advanced |

---

### GettingStarted/ModelContextProtocol

MCP (Model Context Protocol) integration.

| Sample | Purpose | Key Concepts | Difficulty |
|--------|---------|--------------|------------|
| **Agent_MCP_Server** | MCP server tools | MCP client, tool discovery | Intermediate |
| **Agent_MCP_Server_Auth** | Protected MCP server | MCP authentication | Advanced |
| **ResponseAgent_Hosted_MCP** | Hosted MCP tool | Server-side tool invocation | Advanced |
| **FoundryAgent_Hosted_MCP** | Foundry with hosted MCP | Foundry + MCP | Advanced |

**Prerequisites**: Azure OpenAI, MCP server for some samples

---

### GettingStarted/AGUI

AG-UI (Agent UI Protocol) samples for streaming UI events.

| Sample | Purpose | Key Concepts | Difficulty |
|--------|---------|--------------|------------|
| **Step01_GettingStarted** | Basic AG-UI server/client | `MapAGUI`, SSE streaming | Beginner |
| **Step02_BackendTools** | Backend function tools | Server-side tool execution | Intermediate |
| **Step03_FrontendTools** | Frontend tool definitions | Client-defined tools | Intermediate |
| **Step04_HumanInLoop** | Approval workflows | `FunctionApprovalRequestContent` | Advanced |
| **Step05_StateManagement** | Shared state | Predictive state updates | Advanced |

**How to Run (Client/Server)**:
```powershell
# Terminal 1: Start server
cd dotnet/samples/GettingStarted/AGUI/Step01_GettingStarted/Server
dotnet run --urls http://localhost:8888

# Terminal 2: Start client
cd dotnet/samples/GettingStarted/AGUI/Step01_GettingStarted/Client
dotnet run
```

---

### GettingStarted/AgentWithOpenAI

OpenAI SDK type integration for developers using native OpenAI types.

| Sample | Purpose | Key Concepts | Difficulty |
|--------|---------|--------------|------------|
| **Agent_OpenAI_Step01_Running** | Basic agent with OpenAI types | Native SDK types | Beginner |
| **Agent_OpenAI_Step02_Reasoning** | Reasoning models | Extended thinking | Intermediate |
| **Agent_OpenAI_Step03_CreateFromChatClient** | Create from ChatClient | `OpenAIChatClientAgent` | Beginner |
| **Agent_OpenAI_Step04_CreateFromOpenAIResponseClient** | Create from ResponseClient | `OpenAIResponseClientAgent` | Beginner |
| **Agent_OpenAI_Step05_Conversation** | Conversation state | Thread management | Beginner |

---

### GettingStarted/AgentWithAnthropic

Anthropic Claude-specific samples.

| Sample | Purpose | Key Concepts | Difficulty |
|--------|---------|--------------|------------|
| **Agent_Anthropic_Step01_Running** | Basic Claude agent | Anthropic integration | Beginner |
| **Agent_Anthropic_Step02_Reasoning** | Extended thinking | Claude reasoning | Intermediate |
| **Agent_Anthropic_Step03_UsingFunctionTools** | Function tools | Tool calling with Claude | Beginner |

**Prerequisites**: `ANTHROPIC_API_KEY` environment variable

---

### GettingStarted/A2A

Agent-to-Agent protocol samples.

| Sample | Purpose | Key Concepts | Difficulty |
|--------|---------|--------------|------------|
| **A2AAgent_AsFunctionTools** | A2A agent as function tools | Skills as tools | Advanced |
| **A2AAgent_PollingForTaskCompletion** | Long-running task polling | Continuation tokens | Advanced |

---

### GettingStarted/AgentOpenTelemetry

Comprehensive observability demo with Aspire Dashboard.

**Key Features**:
- OpenTelemetry integration with Agent Framework
- .NET Aspire Dashboard for visualization
- Distributed tracing across agent operations
- Token usage metrics

**Prerequisites**: Docker (for Aspire Dashboard), Azure OpenAI

**How to Run**:
```powershell
cd dotnet/samples/GettingStarted/AgentOpenTelemetry
.\start-demo.ps1
```

---

### GettingStarted/DevUI

Developer UI for testing and debugging agents.

| Sample | Purpose | Key Concepts | Difficulty |
|--------|---------|--------------|------------|
| **DevUI_Step01_BasicUsage** | Add DevUI to ASP.NET Core | `MapDevUI`, agent testing | Intermediate |

**How to Run**:
```powershell
cd dotnet/samples/GettingStarted/DevUI/DevUI_Step01_BasicUsage
dotnet run
# Navigate to https://localhost:50516/devui
```

---

### AzureFunctions

Azure Functions with Durable Task for production hosting.

| Sample | Purpose | Key Concepts | Prerequisites |
|--------|---------|--------------|---------------|
| **01_SingleAgent** | Single agent in Functions | HTTP trigger, basic hosting | Azure OpenAI, Docker |
| **02_AgentOrchestration_Chaining** | Sequential orchestration | Durable Functions, chaining | Azure OpenAI, Docker |
| **03_AgentOrchestration_Concurrency** | Parallel agents | Concurrent execution | Azure OpenAI, Docker |
| **04_AgentOrchestration_Conditionals** | Conditional routing | Decision logic | Azure OpenAI, Docker |
| **05_AgentOrchestration_HITL** | Human-in-the-loop | External events, approvals | Azure OpenAI, Docker |
| **06_LongRunningTools** | Long-running tools | Durable orchestrations from tools | Azure OpenAI, Docker |
| **07_AgentAsMcpTool** | Agent as MCP tool | MCP protocol | Azure OpenAI, Docker |
| **08_ReliableStreaming** | Reliable streaming | Redis Streams, reconnection | Azure OpenAI, Docker, Redis |

**Prerequisites**:
- .NET 10 SDK
- Azure Functions Core Tools v4+
- Docker (for Durable Task Scheduler emulator)
- Azure OpenAI with deployed model
- Azurite (Azure Storage Emulator)

**How to Run**:
```powershell
# Start Durable Task Scheduler
docker run -d --name dts-emulator -p 8080:8080 -p 8082:8082 mcr.microsoft.com/dts/dts-emulator:latest

# Start Azure Storage Emulator
docker run -d --name storage-emulator -p 10000:10000 -p 10001:10001 -p 10002:10002 mcr.microsoft.com/azure-storage/azurite

# Configure and run sample
cd dotnet/samples/AzureFunctions/01_SingleAgent
$env:AZURE_OPENAI_ENDPOINT="https://your-resource.openai.azure.com/"
$env:AZURE_OPENAI_DEPLOYMENT="gpt-4o-mini"
func start
```

---

### HostedAgents

Production hosting patterns.

| Sample | Purpose | Key Concepts | Difficulty |
|--------|---------|--------------|------------|
| **AgentWithHostedMCP** | Hosted MCP server tools | Microsoft Learn MCP, auto-approval | Intermediate |
| **AgentWithTextSearchRag** | Text search RAG | Search integration | Intermediate |
| **AgentsInWorkflows** | Agents in workflow executors | Translation chain | Intermediate |

---

### Protocol Samples (Full Client/Server)

Complete client/server implementations for protocol integration.

#### A2AClientServer

Full A2A (Agent-to-Agent) protocol demonstration with multiple specialized agents.

**Components**:
- `A2AServer` - Hosts Invoice, Policy, and Logistics agents
- `A2AClient` - CLI client that orchestrates remote agents

**Key Features**:
- Multi-agent orchestration via A2A protocol
- Agent discovery via agent cards
- Skill-based delegation

**How to Run**:
```powershell
# Terminal 1: Start Invoice Agent
cd dotnet/samples/A2AClientServer/A2AServer
dotnet run --urls "http://localhost:5000" --agentType "invoice"

# Terminal 2: Start Policy Agent
dotnet run --urls "http://localhost:5001" --agentType "policy"

# Terminal 3: Start Logistics Agent
dotnet run --urls "http://localhost:5002" --agentType "logistics"

# Terminal 4: Run Client
cd ../A2AClient
dotnet run
```

#### AGUIClientServer

Full AG-UI protocol demonstration with streaming updates.

**Components**:
- `AGUIServer` - Basic AG-UI server
- `AGUIDojoServer` - Advanced features (tools, state, HITL)
- `AGUIClient` - Console client

#### AGUIWebChat

Blazor-based chat UI using AG-UI protocol.

**Components**:
- `Server` - AG-UI server with Azure OpenAI
- `Client` - Blazor Server chat interface

**Features**:
- Streaming responses
- Conversation suggestions
- Rich chat UI

**How to Run**:
```powershell
# Terminal 1: Start server
cd dotnet/samples/AGUIWebChat/Server
dotnet run

# Terminal 2: Start client
cd ../Client
dotnet run
# Open http://localhost:5000
```

---

### M365Agent

Microsoft 365 Agents SDK integration with Agent Framework.

**Purpose**: Host Agent Framework agents in Microsoft 365 (Teams, WebChat, Copilot)

**Key Features**:
- Weather forecast agent with Adaptive Cards
- Multi-turn conversation handling
- Teams and M365 Copilot deployment

**Prerequisites**:
- Azure Bot Service
- Microsoft 365 Agents Toolkit
- DevTunnel for local development

---

### Purview

Microsoft Purview compliance integration.

| Sample | Purpose | Key Concepts | Difficulty |
|--------|---------|--------------|------------|
| **AgentWithPurview** | Compliance tracking | Data governance, audit | Advanced |

---

## Sample-by-Feature Matrix

| Feature | GettingStarted/Agents | FoundryAgents | AzureFunctions | HostedAgents |
|---------|----------------------|---------------|----------------|--------------|
| **Basic Agent** | Step01 | Step01.2 | 01 | - |
| **Multi-turn** | Step02 | Step02 | - | - |
| **Function Tools** | Step03, Step04 | Step03, Step04 | - | - |
| **Structured Output** | Step05 | Step05 | - | - |
| **Persistence** | Step06, Step07 | Step06 | - | - |
| **Observability** | Step08 | Step07 | - | - |
| **DI Integration** | Step09 | Step08 | - | - |
| **MCP Tools** | Step10 | Step09 | 07 | AgentWithHostedMCP |
| **Images** | Step11 | Step10 | - | - |
| **Middleware** | Step14 | Step12 | - | - |
| **Plugins** | Step15 | Step13 | - | - |
| **Background Ops** | Step13, Step17 | - | - | - |
| **Code Interpreter** | - | Step14 | - | - |
| **Computer Use** | - | Step15 | - | - |
| **Orchestration** | - | - | 02, 03, 04 | - |
| **HITL** | Step04 | Step04 | 05 | - |
| **Streaming** | - | - | 08 | - |

---

## Running Samples: Quick Reference

### Environment Variables

```powershell
# Azure OpenAI (most common)
$env:AZURE_OPENAI_ENDPOINT="https://your-resource.openai.azure.com/"
$env:AZURE_OPENAI_DEPLOYMENT_NAME="gpt-4o-mini"

# OpenAI
$env:OPENAI_API_KEY="sk-..."

# Anthropic
$env:ANTHROPIC_API_KEY="sk-ant-..."

# Azure Foundry
$env:AZURE_FOUNDRY_PROJECT_ENDPOINT="https://your-foundry.services.ai.azure.com/api/projects/your-project"
```

### Common Commands

```powershell
# Navigate to sample
cd dotnet/samples/GettingStarted/Agents/Agent_Step01_Running

# Build
dotnet build

# Run
dotnet run

# Or build and run together
dotnet run
```

### Visual Studio

1. Open `dotnet/agent-framework-dotnet.slnx`
2. Set desired sample project as startup project
3. Press F5 or click Run
4. Provide environment variables when prompted

---

## Troubleshooting

### Authentication Errors

```powershell
# Login to Azure CLI
az login

# Verify account
az account show
```

### Port Conflicts

```powershell
# Find process using port
netstat -ano | findstr :5000

# Or use different port
dotnet run --urls "http://localhost:5001"
```

### Missing Environment Variables

Samples prompt for missing values. Set them beforehand to avoid prompts:

```powershell
[Environment]::SetEnvironmentVariable("AZURE_OPENAI_ENDPOINT", "https://...", "User")
```

### Docker Issues

```powershell
# Restart Docker Desktop
# Then restart emulators
docker start dts-emulator storage-emulator
```

---

## Related Documentation

- [Core Abstractions](core-abstractions.md) - Understanding AIAgent, AgentThread, etc.
- [Core Framework](core-framework.md) - Builder patterns, middleware
- [Providers](providers.md) - Provider integration details
- [Workflows](workflows.md) - Workflow patterns and executors
- [Hosting](hosting.md) - Deployment and hosting options
- [Extension Guide](extension-guide.md) - Creating custom components

---

*Last updated: 2025-12-26*
