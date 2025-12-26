# Claude Code Skills

This directory contains skills (executable scripts) for Claude Code to automate common repository tasks.

## Available Skills

### check-docs

Validates documentation completeness and quality across the repository.

**Usage:**
```bash
./.claude/skills/check-docs
```

**What it checks:**
1. **Broken internal links** - Validates all relative links in markdown files point to existing files
2. **ADR references** - Ensures architecture docs reference relevant Architecture Decision Records
3. **Overview completeness** - Verifies overview.md files link to all drill-down documentation
4. **Mermaid diagrams** - Basic syntax validation for mermaid diagrams
5. **Documentation placeholders** - Finds `[To be documented` placeholders that need completion
6. **Documentation coverage** - Checks for presence of required documentation files

**Exit codes:**
- `0` - Validation passed
- `1` - Validation failed with errors

**Example output:**
```
=== Documentation Validation ===
Repository: /Users/va/r/agent-framework

[1/6] Checking for broken internal links...
ERROR: Broken link in docs/architecture/dotnet/overview.md: missing-file.md

[2/6] Checking ADR references in architecture docs...
INFO: Architecture docs reference ADRs (3 files)

[3/6] Checking overview.md completeness...
WARNING: .NET overview.md doesn't link to new-feature.md

[4/6] Validating mermaid diagrams...

[5/6] Checking for documentation placeholders...
WARNING: Found 5 placeholder(s) in docs/architecture/dotnet/extension-guide.md

[6/6] Checking documentation coverage...

Documentation Coverage:
  ✓ docs/architecture/dotnet/overview.md
  ✓ dotnet/DEV_SETUP.md
  ✓ CLAUDE.md

=== Summary ===
Errors:   1
Warnings: 2
Info:     1
Documentation validation FAILED
```

**When to run:**
- Before committing documentation changes
- During PR reviews for documentation
- As part of CI/CD documentation checks
- After creating new architecture documentation

**Claude Code integration:**

Claude Code can automatically discover and use this skill when:
1. You ask: "validate the documentation"
2. You ask: "check docs for broken links"
3. You ask: "are there any documentation gaps?"

The skill will be invoked automatically and results will be presented to you.

## Creating New Skills

Skills are executable scripts (bash, python, etc.) placed in `.claude/skills/`.

**Requirements:**
1. Must be executable (`chmod +x`)
2. Should have a descriptive name (lowercase, hyphen-separated)
3. Should include usage instructions in comments at the top
4. Should provide clear error messages
5. Should use appropriate exit codes (0 = success, non-zero = failure)

**Example skill structure:**
```bash
#!/usr/bin/env bash

# skill-name: Brief description
#
# Detailed description of what this skill does
# and when to use it

set -euo pipefail

# Implementation here
```

## Why `.claude/` is Committed

Unlike user-specific `.claude/` directories (which contain personal settings), this repository's `.claude/` directory contains:
- **Shared skills** - Useful for all contributors and CI/CD
- **Repository-specific automation** - Not user-specific

This allows:
- Consistent tooling across all developers
- Skills usable in CI/CD pipelines
- Version control of automation scripts
- Team collaboration on repository workflows
