# Hosting & Runtime

> **Purpose**: This document explains how to integrate agents into .NET hosting environments.

## Overview

[To be documented: Summary of hosting patterns and runtime integration]

---

## Dependency Injection

**Package**: `Microsoft.Agents.AI.Hosting`

### Registration Patterns

[To be documented: AddAIAgent(), keyed services]

```csharp
// To be documented: DI registration example
```

### Key Features

[To be documented: Keyed services, factory delegates, tool injection, IHostedAgentBuilder]

### Resolution

[To be documented: How to resolve agents by name]

---

## ASP.NET Core Hosting

### Protocol Endpoints

[To be documented: A2A endpoints, AGUI streaming endpoints]

### Middleware Registration

[To be documented: How to add protocol endpoints]

### Example

```csharp
// To be documented: Minimal API setup
```

**See Also**: [Link to ASP.NET Core samples]

---

## Azure Functions Hosting

**Package**: `Microsoft.Agents.AI.Hosting.AzureFunctions`

### Durable Functions Integration

[To be documented: Long-running agents with Durable Functions]

### Triggers

[To be documented: HTTP, Queue, Timer triggers]

### State Persistence

[To be documented: How state is persisted]

### Example

```csharp
// To be documented: Function with agent example
```

**See Also**: [Link to Azure Functions samples]

---

## DurableTask Integration

**Package**: `Microsoft.Agents.AI.DurableTask`

### Entity Pattern

[To be documented: Agents as durable entities]

#### Key Types

[To be documented: DurableAIAgent, DurableAIAgentProxy, AgentEntity, DurableAgentState, IDurableAgentClient, DurableAgentContext]

### Long-Running Operations

[To be documented: Agent sessions, response resumption, state checkpointing]

### Example

```csharp
// To be documented: Durable entity example
```

---

## Generic Host

[To be documented: Using agents with IHostBuilder]

---

*Last updated: [Date]*
