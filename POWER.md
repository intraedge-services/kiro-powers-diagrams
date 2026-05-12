---
name: "diagrams"
displayName: "Diagrams — Multi-Engine Diagramming"
description: "Generate any diagram type from natural language — cloud architecture with real AWS/GCP/Azure icons, UML, Mermaid flowcharts, sequence diagrams, ERDs, C4, BPMN, D2, Graphviz, and 30+ more. Powered by three rendering engines with hybrid deployment (remote + local offline)."
keywords: ["diagram", "flowchart", "sequence diagram", "architecture", "UML", "mermaid", "plantuml", "ERD", "cloud diagram", "AWS architecture", "GCP architecture", "Azure architecture", "kubernetes", "C4", "BPMN", "class diagram", "state diagram", "Gantt", "mindmap", "D2", "graphviz", "kroki", "infrastructure diagram", "component diagram", "deployment diagram"]
author: "Community"
---

# Diagrams — Multi-Engine Diagramming Power

Generate professional diagrams from natural language using three complementary rendering engines. This power covers every diagramming need — from cloud architecture with real provider icons to UML, flowcharts, ERDs, and 30+ specialized diagram types.

## Rendering Engines

| Engine | Server | Diagram Types | Best For |
|--------|--------|---------------|----------|
| **diagrams-mcp** | Cloud architecture + Mermaid + PlantUML | AWS/GCP/Azure/K8s with real icons, flowcharts, sequence, UML | Infrastructure and architecture documentation |
| **uml-mcp** | 30+ types via Kroki/PlantUML/Mermaid | UML, D2, Graphviz, ERD, BPMN, C4, TikZ, BlockDiag, Nomnoml, Pikchr, Structurizr, SVGBob, WaveDrom | Comprehensive diagramming, batch generation |
| **mcp-mermaid** | Mermaid-focused with themes | Flowcharts, sequence, class, ER, state, Gantt, pie, mindmap, timeline | Quick Mermaid diagrams with styling, offline use |

---

# Onboarding

## Step 1: Verify MCP Server Connectivity

The power uses two remote hosted servers (zero installation) and one local server:

**Remote servers (no install needed):**
- `diagrams-mcp` at `https://diagrams-mcp-production.up.railway.app/mcp`
- `uml-mcp` at `https://uml-mcp.vercel.app/mcp`

**Local server (requires Node.js):**
- `mcp-mermaid` via `npx -y mcp-mermaid`
- Verify Node.js is installed: `node --version` (v18+ required)
- If Node.js is not installed, the remote servers still provide full Mermaid support

## Step 2: Understand the Server Roles

Use this decision tree to pick the right server for each task:

1. **Need cloud architecture with real provider icons?** → Use `diagrams-mcp` (`render_diagram`)
2. **Need PlantUML diagrams (sequence, class, component, state, etc.)?** → Use `diagrams-mcp` (`render_plantuml`) — gives reliable hosted links
3. **Need Mermaid diagrams (flowchart, ER, Gantt, etc.)?** → Use `diagrams-mcp` (`render_mermaid`) — gives reliable hosted links
4. **Need quick Mermaid with custom themes/colors?** → Use `mcp-mermaid` (`generate_mermaid_diagram`)
5. **Need exotic types (D2, Graphviz, BPMN, C4, TikZ, etc.)?** → Use `uml-mcp` (`generate_uml`) — note: returns kroki.io links which may have availability issues
6. **Need to validate syntax before rendering?** → Use `uml-mcp` (`validate_uml`)
7. **Need batch generation (multiple diagrams at once)?** → Use `uml-mcp` (`generate_uml_batch`)
8. **Working offline?** → Use `mcp-mermaid` (local npx)

**Important**: Prefer `diagrams-mcp` for PlantUML and Mermaid rendering — it returns reliable hosted image links. Use `uml-mcp` only for diagram types that `diagrams-mcp` doesn't support (D2, Graphviz, BPMN, C4, TikZ, Nomnoml, etc.).

## Step 3: Quick Test

Try generating a simple diagram to verify everything works:
- Ask: "Create a flowchart showing user login flow"
- The agent will automatically select the best engine and render the diagram

---

# When to Load Steering Files

- Choosing which diagram type to use → `diagram-type-selection.md`
- Writing Mermaid diagrams (flowcharts, sequence, class, ER, state, Gantt) → `mermaid-reference.md`
- Writing PlantUML or UML diagrams → `plantuml-reference.md`
- Creating cloud architecture diagrams (AWS, GCP, Azure, K8s) → `cloud-architecture-guide.md`
- Planning a documentation project with multiple diagrams → `workflow-patterns.md`

---

# Tool Usage Guide

## diagrams-mcp (Cloud Architecture + Mermaid + PlantUML)

### Discovery Tools
- `list_providers()` — List all cloud providers (aws, gcp, azure, k8s, onprem, etc.)
- `list_services(provider)` — List service categories (e.g., aws → compute, database, network)
- `list_nodes(provider, service)` — List node classes with import paths
- `search_nodes(query)` — Search nodes by keyword (e.g., "postgres", "lambda")
- `find_equivalent(node, target_provider)` — Find equivalent across providers
- `list_categories()` — List all 30 infrastructure role categories

### Rendering Tools
- `render_diagram(code)` — Execute Python diagrams script, returns PNG
- `render_mermaid(definition)` — Render Mermaid definition, returns PNG/SVG
- `render_plantuml(definition)` — Render PlantUML definition, returns PNG

## uml-mcp (30+ Diagram Types)

### Tools
- `generate_uml(code, diagram_type, output_format)` — Render any diagram type
- `validate_uml(code, diagram_type)` — Validate syntax before rendering
- `list_diagram_types()` — List all supported types with formats
- `generate_uml_batch(diagrams)` — Generate multiple diagrams in one call

### Supported Types
UML (PlantUML): Class, Sequence, Activity, Use Case, State, Component, Deployment, Object
General: Mermaid, D2, Graphviz, ERD, BlockDiag, BPMN, C4
Specialized: TikZ, Excalidraw, Nomnoml, Pikchr, Structurizr, SVGBob, WaveDrom, WireViz

### Output Formats
SVG, PNG, PDF, JPEG, base64 (availability varies by diagram type)

## mcp-mermaid (Themed Mermaid)

### Tools
- `mermaid_to_image(code, theme, backgroundColor, outputType)` — Render Mermaid with styling

### Output Types
- `base64` — Inline image data
- `svg` — SVG markup
- `file` — Save PNG to disk
- `svg_url` — Public shareable SVG URL
- `png_url` — Public shareable PNG URL

### Themes
`default`, `dark`, `forest`, `neutral`
