# Core Abstractions

> **Purpose**: This document explains the foundational types that all other code depends on.

## Overview

[To be documented: One-paragraph summary of core abstractions layer]

## Architecture Diagram

```mermaid
classDiagram
    %% To be completed: Show AIAgent hierarchy and key abstractions
```

[To be documented: One-paragraph walkthrough of diagram]

---

## AIAgent Base Class

**Purpose**: [To be documented]

**File**: [AIAgent.cs](../../dotnet/src/Microsoft.Agents.AI.Abstractions/AIAgent.cs)

### Key Members

[To be documented: Properties, methods, extension points]

### Extension Points

[To be documented: How to extend, when to extend]

### Examples

```csharp
// To be documented: Minimal example
```

**See Also**: [Related docs]

---

## DelegatingAIAgent

**Purpose**: [To be documented]

[To be documented: Base class for decorator pattern, middleware usage]

---

## AgentRunResponse & AgentRunResponseUpdate

**Purpose**: [To be documented]

### Design Decision

> **Design Decision**: [Link to ADR-0001]

---

## AgentThread

**Purpose**: [To be documented]

### Thread Types

[To be documented: Server-managed vs. client-managed]

---

## ChatMessageStore

**Purpose**: [To be documented]

[To be documented: Repository pattern for message persistence]

---

## AIContextProvider

**Purpose**: [To be documented]

[To be documented: Dynamic context injection, lifecycle hooks]

---

## AgentRunOptions

**Purpose**: [To be documented]

### Continuation Tokens

[To be documented: Long-running operations support]

---

*Last updated: [Date]*
