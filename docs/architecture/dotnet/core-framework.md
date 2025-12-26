# Core Framework

> **Purpose**: This document explains the framework services, middleware, and agent builder.

## Overview

[To be documented: Summary of core framework layer]

---

## AIAgentBuilder

**Purpose**: [To be documented: Fluent API for composing agent pipelines]

### Architecture

```mermaid
%% To be completed: Show builder pattern and middleware chain
```

### Key Methods

[To be documented: Use(), Build(), extension points]

### Examples

```csharp
// To be documented: Multi-layer middleware example
```

---

## ChatClientAgent

**Purpose**: [To be documented: Bridge to Microsoft.Extensions.AI]

### Microsoft.Extensions.AI Bridge

[To be documented: How ChatClientAgent wraps IChatClient]

### Thread Management

[To be documented: Auto-detection, server vs. client managed]

### Options

[To be documented: ChatClientAgentOptions, factories, merging]

---

## Built-in Middleware

### LoggingAgent

[To be documented: Structured logging with ILogger]

### OpenTelemetryAgent

[To be documented: Distributed tracing, GenAI semantic conventions]

### FunctionInvocationDelegatingAgent

[To be documented: Tool call interception]

---

## Memory Providers

### ChatHistoryMemoryProvider

[To be documented: Sliding window memory]

### Integration with AIContextProvider

[To be documented: Dynamic memory injection]

---

## Anonymous Middleware

[To be documented: AnonymousDelegatingAIAgent, inline middleware]

---

*Last updated: [Date]*
