# Extension Guide

> **Purpose**: This document provides step-by-step guides for extending the framework.

## Overview

[To be documented: Summary of extension patterns and when to use them]

---

## Creating a Custom Provider

### When to Create One

[To be documented: Scenarios requiring custom provider]

### Prerequisites

[To be documented: Required knowledge, dependencies]

### Step-by-Step Guide

**Step 1**: [To be documented]

**Step 2**: [To be documented]

### Complete Working Example

```csharp
// To be documented: Complete, runnable code
```

### Testing Your Implementation

[To be documented: How to verify it works]

### Common Pitfalls

[To be documented: What to avoid]

**See Also**: [Link to provider samples]

---

## Creating Custom Middleware

### When to Create One

[To be documented: Scenarios requiring middleware]

### Step-by-Step Guide

[To be documented: Inherit from DelegatingAIAgent, override methods, register via Use()]

### Complete Working Example

```csharp
// To be documented: Rate-limiting middleware example
```

**See Also**: [Link to middleware samples]

---

## Creating a Custom Context Provider

[To be documented: Implementing AIContextProvider]

---

## Creating Custom Memory

[To be documented: Implementing ChatMessageStore]

---

## Creating Custom Thread Storage

[To be documented: Implementing IAgentThreadStore]

---

## Creating Custom Tools

[To be documented: Deriving from AITool]

**See Also**: [Link to ADR-0002: Agent Tools]

---

## Creating Custom Executors

[To be documented: Implementing Executor<TInput, TOutput>]

---

## Creating Custom Protocol Adapters

[To be documented: Wrapping protocol client in AIAgent]

---

*Last updated: [Date]*
