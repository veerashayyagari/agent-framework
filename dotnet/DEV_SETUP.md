# Dev Setup

This document describes how to set up your environment for developing the .NET Agent Framework, whether you're working on new features, bug fixes, or simply want to run the included tests.

For coding standards and architecture documentation, see [.github/instructions/architecture-dotnet.instructions.md](../.github/instructions/architecture-dotnet.instructions.md).

## System Setup

The .NET Agent Framework uses MSBuild for project management and the .NET CLI for building, testing, and running applications. For a deep dive into the build system architecture, see [docs/development/dotnet-build-system.md](../docs/development/dotnet-build-system.md).

## Prerequisites

### Required

- **.NET SDK 10.0.100** or later (specified in [global.json](global.json))
  - Supports multi-targeting: net10.0, net9.0, net8.0, netstandard2.0, net472
  - Download from [dotnet.microsoft.com](https://dotnet.microsoft.com/download)

### Recommended IDEs

- **Visual Studio 2025** (Windows/Mac) - Full-featured IDE with excellent .NET support
- **JetBrains Rider** (Cross-platform) - Powerful cross-platform .NET IDE
- **Visual Studio Code** (Cross-platform) - Lightweight with C# DevKit extension

### Optional

- **Cosmos DB Emulator** (Windows only) - Required for Cosmos DB integration tests
  - Download from [Azure Cosmos DB Emulator](https://learn.microsoft.com/azure/cosmos-db/local-emulator)
  - Linux/macOS: Use Docker-based emulator or skip Cosmos tests
- **API Keys** - For running integration tests with AI providers:
  - `AZURE_OPENAI_ENDPOINT` and `AZURE_OPENAI_API_KEY` (Azure OpenAI)
  - `OPENAI_API_KEY` (OpenAI)
  - `ANTHROPIC_API_KEY` (Anthropic)

## If You're on WSL

Check that you've cloned the repository to `~/workspace` or a similar folder. Avoid `/mnt/c/` and prefer using your WSL user's home directory for better performance.

Ensure you have the WSL extension for Visual Studio Code installed if using VS Code.

## Quick Start

Navigate to the `dotnet` directory and build the solution:

```bash
# Clone the repository
git clone https://github.com/microsoft/agent-framework.git
cd agent-framework/dotnet

# Build the entire solution (all target frameworks)
dotnet build agent-framework-dotnet.slnx

# Run unit tests (fast, no external dependencies)
dotnet test --filter "FullyQualifiedName~UnitTests"
```

You should see a successful build and all unit tests passing.

## Project Structure

The .NET codebase is organized as follows:

```
dotnet/
├── src/                          # Core libraries (organized by layer)
│   ├── Microsoft.Agents.AI/      # Core abstractions
│   ├── Microsoft.Agents.AI.Framework/  # Framework implementation
│   ├── Microsoft.Agents.AI.OpenAI/     # OpenAI provider
│   ├── Microsoft.Agents.AI.AzureAI/    # Azure AI provider
│   ├── Microsoft.Agents.AI.Workflows/  # Workflow engine
│   ├── Microsoft.Agents.AI.Hosting/    # DI and hosting
│   └── ...                       # Other packages
├── samples/                      # Example applications
├── tests/                        # Unit and integration tests
├── eng/                          # Build infrastructure
│   └── MSBuild/                  # Shared .props and .targets files
├── agent-framework-dotnet.slnx   # Solution file
├── Directory.Build.props         # Shared build configuration
├── Directory.Packages.props      # Central Package Management
└── global.json                   # .NET SDK version
```

## IDE Setup

### Visual Studio 2025

1. Open `agent-framework-dotnet.slnx` in Visual Studio 2025
2. Restore NuGet packages (should happen automatically)
3. Build → Build Solution (Ctrl+Shift+B)
4. Test → Run All Tests (Ctrl+R, A)

### JetBrains Rider

1. Open `agent-framework-dotnet.slnx` in Rider
2. Rider will automatically restore NuGet packages
3. Build → Build Solution (Ctrl+Shift+F9 / Cmd+Shift+F9)
4. Run → Run Unit Tests (Ctrl+T, R / Cmd+T, R)

### Visual Studio Code

1. Install the [C# Dev Kit extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit)
2. Open the `dotnet` folder in VS Code
3. The C# extension will automatically detect the solution
4. Use the integrated terminal for `dotnet` commands

## Common Tasks

### Building

#### Build Entire Solution

Build all projects for all target frameworks:

```bash
dotnet build agent-framework-dotnet.slnx
```

#### Build Specific Target Framework

Build only for .NET 10.0:

```bash
dotnet build agent-framework-dotnet.slnx --framework net10.0
```

#### Clean Build

Remove all build artifacts and rebuild:

```bash
dotnet clean agent-framework-dotnet.slnx
dotnet build agent-framework-dotnet.slnx
```

### Testing

#### Unit Tests

Run fast, isolated unit tests (no external dependencies):

```bash
dotnet test --filter "FullyQualifiedName~UnitTests"
```

#### Integration Tests

Run integration tests (requires environment variables and external services):

```bash
# Set up environment variables first (see "Environment Variables" section)
dotnet test --filter "FullyQualifiedName~IntegrationTests"
```

#### Test Specific Project

Run tests for a single project:

```bash
dotnet test tests/Microsoft.Agents.AI.UnitTests/
```

#### Test with Coverage

Generate code coverage reports:

```bash
dotnet test --collect:"XPlat Code Coverage"
```

### Code Formatting

The solution uses `.editorconfig` and `dotnet format` for consistent code formatting.

#### Format Entire Solution

```bash
dotnet format agent-framework-dotnet.slnx
```

#### Verify Formatting (CI Check)

Check if formatting is correct without making changes:

```bash
dotnet format agent-framework-dotnet.slnx --verify-no-changes
```

This command is used in CI/CD pipelines to ensure code is properly formatted.

### Adding NuGet Packages

The solution uses **Central Package Management (CPM)**, which centralizes package versions in `Directory.Packages.props`.

#### Steps to Add a Package

1. Open `Directory.Packages.props` in the `dotnet` folder
2. Add a `<PackageVersion>` entry:
   ```xml
   <PackageVersion Include="Newtonsoft.Json" Version="13.0.3" />
   ```
3. In your project's `.csproj` file, reference the package **without a version**:
   ```xml
   <PackageReference Include="Newtonsoft.Json" />
   ```

The version is automatically inherited from `Directory.Packages.props`.

#### Why CPM?

- **Single source of truth** for package versions across the entire solution
- **Easy upgrades** - change version in one place
- **Consistent dependencies** - all projects use the same version
- **Transitive dependency pinning** - control indirect dependencies

For more details, see [docs/development/dotnet-build-system.md](../docs/development/dotnet-build-system.md#central-package-management).

### Adding New Projects

#### Create a New Library

```bash
# Create a new class library
dotnet new classlib -n Microsoft.Agents.AI.NewFeature -o src/Microsoft.Agents.AI.NewFeature

# Add to solution
dotnet sln agent-framework-dotnet.slnx add src/Microsoft.Agents.AI.NewFeature/
```

#### Configure Target Frameworks

Projects inherit target frameworks from `Directory.Build.props`. To override for a specific project, edit the `.csproj`:

```xml
<PropertyGroup>
  <!-- Override inherited TargetFrameworks -->
  <TargetFrameworks>net10.0;net9.0</TargetFrameworks>
</PropertyGroup>
```

Most projects target: `net10.0;net9.0;net8.0;netstandard2.0;net472`

### Multi-Targeting

The framework supports multiple target frameworks (TFMs) to maximize compatibility:

- **net10.0, net9.0, net8.0** - Modern .NET (recommended)
- **netstandard2.0** - Cross-platform compatibility (.NET Framework 4.7.2+, .NET Core 2.0+)
- **net472** - Legacy .NET Framework support

#### Conditional Compilation

Use preprocessor directives for framework-specific code:

```csharp
#if NET472
    // .NET Framework 4.7.2 specific code
#elif NETSTANDARD2_0
    // .NET Standard 2.0 specific code
#else
    // Modern .NET (net8.0+) specific code
#endif
```

#### Polyfills for Modern C# Features

The build system provides polyfills for modern C# features on older frameworks. See [LegacySupport.props](eng/MSBuild/LegacySupport.props) for available polyfills:

- `IsExternalInit` (for `init` properties)
- `CallerArgumentExpression` (for argument validation)
- `Required` attribute (for required members)
- And more...

To enable polyfills in your project, set properties in your `.csproj`:

```xml
<PropertyGroup>
  <InjectIsExternalInitOnLegacy>true</InjectIsExternalInitOnLegacy>
</PropertyGroup>
```

## Environment Variables

### For Integration Tests

Integration tests require environment variables to connect to external services. You can set these in your shell or use a `.env` file (not checked into source control).

| Variable | Purpose | Example |
|----------|---------|---------|
| `AZURE_OPENAI_ENDPOINT` | Azure OpenAI service endpoint | `https://your-resource.openai.azure.com` |
| `AZURE_OPENAI_API_KEY` | Azure OpenAI API key | `abcd1234...` |
| `OPENAI_API_KEY` | OpenAI API key | `sk-...` |
| `ANTHROPIC_API_KEY` | Anthropic API key | `sk-ant-...` |

### Setting Environment Variables

**Windows (PowerShell):**
```powershell
$env:AZURE_OPENAI_ENDPOINT = "https://your-resource.openai.azure.com"
$env:AZURE_OPENAI_API_KEY = "your-api-key"
```

**Linux/macOS (Bash):**
```bash
export AZURE_OPENAI_ENDPOINT="https://your-resource.openai.azure.com"
export AZURE_OPENAI_API_KEY="your-api-key"
```

**Using .env file (recommended):**

Create a `.env` file in the `dotnet` folder (add to `.gitignore`):

```env
AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com
AZURE_OPENAI_API_KEY=your-api-key
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
```

Then load it in your shell before running tests.

## Troubleshooting

### "Package version conflict" Errors

**Symptom:** Build fails with package version conflicts.

**Solution:**

```bash
# Force restore packages
dotnet restore --force

# Check Directory.Packages.props for centralized versions
# Ensure all projects reference packages WITHOUT version attributes
```

### "Modern C# features don't compile on net472"

**Symptom:** Features like `init` properties or `required` members fail to compile for .NET Framework 4.7.2.

**Solution:**

Enable polyfills in your `.csproj`:

```xml
<PropertyGroup>
  <InjectIsExternalInitOnLegacy>true</InjectIsExternalInitOnLegacy>
  <InjectRequiredAttributeOnLegacy>true</InjectRequiredAttributeOnLegacy>
</PropertyGroup>
```

See [docs/architecture/dotnet/dotnet-build-system.md](../docs/architecture/dotnet/dotnet-build-system.md#polyfill-system) for all available polyfills.

### "Directory.Build.props changes not picked up"

**Symptom:** Changes to `Directory.Build.props` or other build files don't seem to apply.

**Solution:**

```bash
# Clean all build artifacts
dotnet clean

# Remove bin/obj directories (more thorough)
rm -rf */bin */obj

# Restore and rebuild
dotnet restore
dotnet build
```

### "Tests fail with 'Cosmos DB not available'"

**Symptom:** Integration tests fail because Cosmos DB emulator is not running.

**Solution:**

- **Windows:** Start the Cosmos DB Emulator from the Start menu
- **Linux/macOS:** Use Docker-based Cosmos DB emulator:
  ```bash
  docker run -p 8081:8081 -p 10251:10251 mcr.microsoft.com/cosmosdb/linux/azure-cosmos-emulator
  ```
- **Alternative:** Skip Cosmos DB tests:
  ```bash
  dotnet test --filter "FullyQualifiedName!~Cosmos"
  ```

### "Build is slow on WSL"

**Symptom:** Builds take significantly longer on WSL compared to Windows.

**Solution:**

- Clone the repository to WSL's native filesystem (`~/workspace`), not `/mnt/c/`
- Use WSL 2 (faster than WSL 1)
- Disable Windows Defender real-time scanning for WSL directories

## Code Quality Checks

Before committing code, ensure it meets quality standards:

```bash
# Format code
dotnet format agent-framework-dotnet.slnx

# Build entire solution
dotnet build agent-framework-dotnet.slnx

# Run unit tests
dotnet test --filter "FullyQualifiedName~UnitTests"

# Verify formatting (CI check)
dotnet format agent-framework-dotnet.slnx --verify-no-changes
```

## Catching Up with the Latest Changes

When working with a team, keep your local repository synchronized:

### Using Rebase (Recommended)

```bash
git fetch upstream main
git rebase upstream/main
git push --force-with-lease
```

### Using Merge

```bash
git fetch upstream main
git merge upstream/main
git push
```

**Note:** Replace `upstream` with your remote name if different (commonly `origin` for main repo).

After rebasing or merging, you may need to resolve conflicts. See [GitHub's documentation on resolving conflicts](https://docs.github.com/get-started/using-git/resolving-merge-conflicts-after-a-git-rebase) or [VS Code's merge conflict guide](https://code.visualstudio.com/docs/sourcecontrol/overview#_merge-conflicts).

## Deep Dive

### Build System Architecture

For a comprehensive understanding of MSBuild, .props/.targets files, Central Package Management, polyfills, and code injection patterns, see:

- [docs/architecture/dotnet/dotnet-build-system.md](../docs/architecture/dotnet/dotnet-build-system.md)

### Architecture Documentation

For framework architecture, design patterns, and extension guides, see:

- [docs/architecture/dotnet/overview.md](../docs/architecture/dotnet/overview.md)
- [.github/instructions/architecture-dotnet.instructions.md](../.github/instructions/architecture-dotnet.instructions.md)

### Sample Applications

For example applications demonstrating framework features:

- [samples/](samples/) - Various sample applications
- Each sample has a README with usage instructions

## Getting Help

- **Issues:** Report bugs or request features at [github.com/microsoft/agent-framework/issues](https://github.com/microsoft/agent-framework/issues)
- **Discussions:** Ask questions at [github.com/microsoft/agent-framework/discussions](https://github.com/microsoft/agent-framework/discussions)
- **Architecture:** Review [docs/architecture/dotnet/](../docs/architecture/dotnet/)
- **ADRs:** See [docs/decisions/](../docs/decisions/) for architecture decision records

---

*For Python development setup, see [python/DEV_SETUP.md](../python/DEV_SETUP.md).*
