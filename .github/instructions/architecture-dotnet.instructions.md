---
applyTo: "dotnet/**,docs/**"
---

# Dotnet architecture documentation instructions

- Scope: dotnet SDK code, hosting, samples, and architecture docs; prioritize mermaid diagrams (fallback to ASCII only if rendering blocks).
- Deliverables and file map (create if missing):
	- Index: docs/architecture/dotnet/overview.md.
	- Drill-downs (linked from overview): docs/architecture/dotnet/core.md, hosting.md, adapters.md, cross-cutting.md, samples.md, tests.md (add more as needed; keep names descriptive and consistent).
- Overview content: short repo/solution map, project roles, integration flows, key cross-cutting concerns; include a mermaid diagram near the top plus a brief explanation; list and link all drill-down pages.
- Drill-down page content: scope statement; purpose and responsibilities; key assemblies/projects and main namespaces; primary abstractions/interfaces and extension points (include “how to extend” notes); data/contracts/configuration knobs (env vars/appsettings and defaults); lifecycle or request/operation flows; CI/build/test notes relevant to that area; focused mermaid diagram and short explanation; links to notable classes/entry points (relative paths).
- Diagram conventions: keep nodes concise; group by layer (core, hosting, adapters, samples); prefer one system/context view and, only if needed, one component/flow view per page.
- Linking and references: use relative links to drill-downs and source files; highlight primary classes or Program.cs entry points rather than dumping large code blocks; include short pseudo/structure snippets only when they clarify non-obvious flows.
- Process (cold start and repeatable):
	- Inventory: solutions (.sln/.slnx), Directory.Build.* files, csproj references, top-level README/docs, eng/ scripts, CI workflows.
	- Record build/test commands for dotnet and note prerequisites (SDK version, env vars) and any quirks/timeouts.
	- Identify integration points and extension seams (interfaces to implement, middleware/hosting hooks, provider adapters); capture them explicitly.
	- Place diagrams near the top with a one-paragraph walkthrough.
	- Keep the overview updated whenever adding drill-downs or finding new cross-cutting concerns.
- Validation: run documented build/test commands once per area when feasible and note pitfalls or workarounds.
- Style: concise, imperative guidance; avoid task-specific text; keep instructions under two pages; do not include secrets; keep comments in code minimal and only for non-obvious logic.
