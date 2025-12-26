# Agent Framework - AI Assistant Guide

This file provides guidance for AI coding assistants (Claude Code, GitHub Copilot) working on the Microsoft Agent Framework repository.

## Repository Overview

The Agent Framework is a multi-language SDK for building AI agents with provider-agnostic abstractions.

- **.NET**: Mature implementation with comprehensive architecture documentation (11 detailed guides)
- **Python**: Growing implementation with architecture documentation in progress

Both implementations follow unified design principles while respecting language-specific idioms.

## For Detailed Guidance

### Architecture & Design

- **.NET Architecture**: See [.github/instructions/architecture-dotnet.instructions.md](.github/instructions/architecture-dotnet.instructions.md)
  - Principle-based, pattern-focused approach
  - 8-phase documentation process
  - Core principles: Depth Over Breadth, Pattern Recognition, Progressive Disclosure, Executable Understanding
  - Covers abstractions, framework, providers, protocols, workflows, hosting
  - Start with [docs/architecture/dotnet/overview.md](docs/architecture/dotnet/overview.md)

- **Python Architecture**: See `.github/instructions/architecture-python.instructions.md` (planned)

### Code Quality Standards

- **Python**: [python/CODING_STANDARD.md](python/CODING_STANDARD.md) (comprehensive, 403 lines)
  - Type hints, docstrings, error handling, testing conventions
- **General**: [.github/copilot-instructions.md](.github/copilot-instructions.md) (C# and Python guidelines)

### Development Setup

- **.NET**: [dotnet/DEV_SETUP.md](dotnet/DEV_SETUP.md) (prerequisites, build, test, common workflows)
- **Python**: [python/DEV_SETUP.md](python/DEV_SETUP.md) (uv-based package manager, poe tasks)

### Build Systems

- **.NET MSBuild**: [docs/architecture/dotnet/dotnet-build-system.md](docs/architecture/dotnet/dotnet-build-system.md) (deep-dive into .props/.targets files)
  - Explains Directory.Build.props, Central Package Management
  - Multi-targeting strategy (5 TFMs: net10.0, net9.0, net8.0, netstandard2.0, net472)
  - Polyfill system for modern C# on legacy frameworks
  - Code injection patterns
- **Python Poetry/uv**: Covered in [python/DEV_SETUP.md](python/DEV_SETUP.md)

## Common Tasks

### Creating Documentation

1. **Follow existing patterns** in [docs/architecture/dotnet/](docs/architecture/dotnet/) (templates with `[To be documented: ...]` placeholders)
2. **Reference ADRs** from [docs/decisions/](docs/decisions/) when documenting design decisions
3. **Use principle-based approach** from architecture-dotnet.instructions.md:
   - Depth over breadth (explain why, how, trade-offs)
   - Identify patterns, not just instances
   - Progressive disclosure (overview → concepts → details → extension)
   - Include diagrams + working code examples

### Adding AI Providers

- **.NET**: Follow pattern in [src/Microsoft.Agents.AI.OpenAI/](dotnet/src/Microsoft.Agents.AI.OpenAI/)
  - Implement provider-specific agent inheriting from base abstractions
  - Add extension methods for `AIAgentBuilder`
  - Include OpenTelemetry integration
  - Add unit + integration tests
- **Python**: Follow lazy-loading pattern in [python/packages/](python/packages/)
  - Separate package per provider
  - Use `importlib` for optional dependencies

### Writing Samples

- **Follow guidelines**: [python/samples/SAMPLE_GUIDELINES.md](python/samples/SAMPLE_GUIDELINES.md)
- **Consistent README structure**: Purpose, Prerequisites, Setup, Usage, Key Concepts
- **Include both languages** when demonstrating framework features

### Creating ADRs (Architecture Decision Records)

- **Template**: [docs/decisions/TEMPLATE.md](docs/decisions/TEMPLATE.md)
- **Auto-number**: Use next available number in sequence
- **Validate frontmatter**: title, status, date, deciders, context, decision, consequences
- **Link from architecture docs**: Reference ADR-XXXX when documenting related features

### Building and Testing

- **.NET**:
  ```bash
  # Build entire solution (all TFMs)
  dotnet build dotnet/agent-framework-dotnet.slnx

  # Run unit tests (fast, no external dependencies)
  dotnet test --filter "FullyQualifiedName~UnitTests"

  # Format code
  dotnet format dotnet/agent-framework-dotnet.slnx
  ```

- **Python**:
  ```bash
  # Install dependencies (uv-based)
  uv sync --all-extras

  # Run tests with poe
  poe test

  # Format code
  poe format
  ```

## Key Principles

When contributing to this repository, follow these core principles from the architecture documentation:

### 1. Depth Over Breadth
Analyze thoroughly rather than catalog superficially. For each component, explain **why it exists**, **how it works**, **how to extend it**, and **what trade-offs were made**.

### 2. Pattern Recognition Over Enumeration
Identify and document **patterns** used throughout the codebase, not just specific instances. Example: "The framework uses the Decorator pattern for middleware pipelines" (with examples) rather than listing individual decorators.

### 3. Progressive Disclosure
Structure documentation with layered detail:
- **Overview**: High-level purpose and key concepts
- **Concepts**: Core abstractions and patterns (with diagrams)
- **Details**: API reference, configuration options
- **Extension**: Step-by-step guides for customization

### 4. Executable Understanding
Every concept should include:
- **Mermaid diagram** showing structure/flow
- **Working code example** demonstrating usage
- **Extension point** showing how to customize

## Navigation Guide

### For New Contributors
1. Start with main [README.md](README.md) (project overview)
2. Read [CONTRIBUTING.md](CONTRIBUTING.md) (contribution guidelines)
3. Review your language's DEV_SETUP.md (environment setup)
4. Browse architecture docs for your language (understand design)

### For Documentation Work
1. Review [.github/instructions/architecture-dotnet.instructions.md](.github/instructions/architecture-dotnet.instructions.md) (principles)
2. Check [docs/architecture/dotnet/overview.md](docs/architecture/dotnet/overview.md) (example of completed docs)
3. Use templates in [docs/architecture/dotnet/](docs/architecture/dotnet/) (fill in `[To be documented: ...]`)
4. Reference [docs/decisions/](docs/decisions/) (ADRs for design decisions)

### For Build System Work
1. Read [docs/architecture/dotnet/dotnet-build-system.md](docs/architecture/dotnet/dotnet-build-system.md) (MSBuild deep-dive)
2. Understand Directory.Build.props, Directory.Packages.props, LegacySupport.props
3. See [dotnet/DEV_SETUP.md](dotnet/DEV_SETUP.md) for common tasks

---

*This file complements detailed instructions in `.github/instructions/` and serves as a navigation hub for both Claude Code and GitHub Copilot.*
