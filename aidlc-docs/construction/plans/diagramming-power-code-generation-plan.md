# Code Generation Plan — Kiro Diagramming Power

## Unit Context
- **Unit Name**: diagramming-power
- **Project Type**: Greenfield — Kiro Power package
- **Target Directory**: `power-diagrams/` (workspace root)
- **Deliverable**: Complete Kiro Power ready for installation

## Dependencies
- None (self-contained power package)

## File Generation Sequence

### Step 1: Create Power Directory Structure
- [x] Create `power-diagrams/` directory with all required files

### Step 2: Generate POWER.md
- [x] Create `power-diagrams/POWER.md` with:
  - Frontmatter (name, displayName, description, keywords, author)
  - Onboarding instructions (validate prerequisites, explain servers)
  - Steering file mappings (when to load which steering file)
  - Tool usage guidance for all 3 MCP servers

### Step 3: Generate mcp.json
- [x] Create `power-diagrams/mcp.json` with 3 MCP servers:
  - `diagrams-mcp` — Remote HTTP (Railway) for cloud architecture + Mermaid + PlantUML
  - `uml-mcp` — Remote HTTP (Vercel) for 30+ diagram types via Kroki
  - `mcp-mermaid` — Local npx for lightweight Mermaid rendering (offline fallback)

### Step 4: Generate Steering File — Diagram Type Selection Guide
- [x] Create `power-diagrams/steering/diagram-type-selection.md` with:
  - Decision matrix: which engine/type for which use case
  - Comparison table of all supported diagram types
  - Recommendations by scenario (architecture, workflow, documentation)

### Step 5: Generate Steering File — Mermaid Reference
- [x] Create `power-diagrams/steering/mermaid-reference.md` with:
  - Syntax quick-reference for all Mermaid diagram types
  - Common patterns and examples
  - Theme and styling options
  - Output format options

### Step 6: Generate Steering File — PlantUML Reference
- [x] Create `power-diagrams/steering/plantuml-reference.md` with:
  - Syntax quick-reference for all PlantUML diagram types
  - Common patterns and examples
  - Styling and theming
  - Integration with Kroki rendering

### Step 7: Generate Steering File — Cloud Architecture Guide
- [x] Create `power-diagrams/steering/cloud-architecture-guide.md` with:
  - AWS/GCP/Azure/K8s diagram patterns
  - Provider node discovery workflow
  - Cross-provider equivalence usage
  - Best practices for architecture diagrams

### Step 8: Generate Steering File — Workflow Patterns
- [x] Create `power-diagrams/steering/workflow-patterns.md` with:
  - Architecture-first workflow (design → diagram → code)
  - Code-first workflow (code → analyze → diagram)
  - Documentation workflow (requirements → diagrams → docs)
  - Iterative refinement patterns
  - Multi-diagram project documentation strategy

### Step 9: Generate Documentation Summary
- [x] Create `aidlc-docs/construction/diagramming-power/code/code-summary.md` with:
  - List of all generated files
  - Power structure overview
  - Installation instructions

## Total Steps: 9
## Estimated Scope: All files are configuration/documentation (no compiled code)
