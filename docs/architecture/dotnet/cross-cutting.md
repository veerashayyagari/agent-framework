# Cross-Cutting Concerns

> **Purpose**: This document explains shared infrastructure and support services.

## Overview

[To be documented: Summary of cross-cutting concerns]

---

## Memory & Context

### Mem0 Integration

**Package**: `Microsoft.Agents.AI.Mem0`

[To be documented: Mem0 memory service integration]

### ChatHistoryMemoryProvider

[To be documented: Framework-native memory provider]

### AIContextProvider Integration

[To be documented: Dynamic context injection pattern]

---

## State Storage

### CosmosDB Integration

**Package**: `Microsoft.Agents.AI.CosmosNoSql`

[To be documented: Cosmos DB thread/state storage]

### Custom Storage

[To be documented: Implementing IAgentThreadStore]

---

## Compliance & Governance

### Purview Integration

**Package**: `Microsoft.Agents.AI.Purview`

[To be documented: Microsoft Purview integration, data governance, compliance tracking]

---

## Declarative Agents

**Package**: `Microsoft.Agents.AI.Declarative`

### JSON/YAML Definitions

[To be documented: Agent definition format]

### Schema Validation

[To be documented: Schema requirements]

### Loading

[To be documented: Declarative agent loaders, runtime construction]

### Example

```yaml
# To be documented: YAML agent definition example
```

---

## Developer Tools

### DevUI

**Package**: `Microsoft.Agents.AI.DevUI`

[To be documented: Developer-facing UI helpers, debugging utilities, agent introspection]

---

## Legacy Support

**Package**: `LegacySupport`

[To be documented: .NET Framework 4.7.2 polyfills]

---

*Last updated: [Date]*
