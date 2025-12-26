# Build & Test

> **Purpose**: This document explains how to build, test, and troubleshoot the SDK.

## Prerequisites

[To be documented: .NET SDK version from global.json, optional dependencies]

---

## Build Commands

### Build Entire Solution

```bash
# To be documented: Build all TFMs
dotnet build dotnet/agent-framework-dotnet.slnx
```

### Build Specific Framework

```bash
# To be documented: Build specific TFM
dotnet build dotnet/agent-framework-dotnet.slnx --framework net10.0
```

### Clean Build

[To be documented: Clean and rebuild commands]

---

## Test Commands

### Unit Tests

```bash
# To be documented: Unit tests (no external dependencies)
dotnet test dotnet/agent-framework-dotnet.slnx --filter "FullyQualifiedName~UnitTests"
```

### Integration Tests

```bash
# To be documented: Integration tests (requires env vars)
dotnet test dotnet/agent-framework-dotnet.slnx --filter "FullyQualifiedName~IntegrationTests"
```

### Test Specific Project

[To be documented: How to test specific projects]

---

## Environment Variables

### Required for Integration Tests

[To be documented: Comprehensive table of environment variables]

| Variable | Purpose | Example | Required For |
|----------|---------|---------|--------------|
| `AZURE_OPENAI_ENDPOINT` | [To be documented] | `https://....openai.azure.com` | OpenAI integration tests |

---

## CI/CD

### GitHub Actions Workflow

[To be documented: .github/workflows/dotnet-build-and-test.yml]

### Triggers

[To be documented: Pull request, push to main]

### Matrix

[To be documented: TFMs tested, platforms]

### Test Categories

[To be documented: What tests run in CI]

---

## Troubleshooting

### Common Build Issues

[To be documented: Common build errors and solutions]

### Test Failures

[To be documented: Common test failures and solutions]

### Platform-Specific Issues

[To be documented: Windows/macOS/Linux quirks]

### Cosmos Emulator

[To be documented: Cosmos emulator timeouts, workarounds]

---

## Advanced Scenarios

### Multi-Targeting

[To be documented: Building for specific TFMs]

### Code Coverage

[To be documented: How to generate code coverage reports]

### Performance Testing

[To be documented: Performance test infrastructure]

---

*Last updated: [Date]*
