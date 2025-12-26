# Provider Adapters

> **Purpose**: This document explains how AI services integrate with the framework.

## Overview

[To be documented: Summary of provider integration strategy]

---

## Provider Integration Pattern

### Strategy

[To be documented: The general pattern all providers follow]

1. Provider-specific client
2. Adapter to IChatClient
3. Extension method CreateAIAgent()
4. Provider-specific options

### Benefits

[To be documented: Consistent API, shared middleware, easy switching]

---

## OpenAI & Azure OpenAI

**Package**: `Microsoft.Agents.AI.OpenAI`

### Integration Points

[To be documented: How OpenAI integrates]

### Provider-Specific Features

[To be documented: Structured outputs, function calling, streaming]

### Example

```csharp
// To be documented: Minimal usage example
```

### Configuration Options

[To be documented: Table of options with defaults]

### Tool Support

[To be documented: What tools are supported]

### Design Considerations

[To be documented: Quirks, limitations, best practices]

**See Also**: [Link to samples]

---

## Azure AI Foundry

**Package**: `Microsoft.Agents.AI.AzureAI`

[To be documented: Similar structure as OpenAI section]

---

## Azure AI Agents (Persistent)

**Package**: `Microsoft.Agents.AI.AzureAI.Persistent`

[To be documented: Managed agents, long-running operations]

---

## Anthropic Claude

**Package**: `Microsoft.Agents.AI.Anthropic`

[To be documented: Claude-specific features]

---

## Copilot Studio

**Package**: `Microsoft.Agents.AI.CopilotStudio`

[To be documented: Copilot Studio integration]

---

## Tool Support Comparison

[To be documented: Table comparing tool support across providers]

---

*Last updated: [Date]*
