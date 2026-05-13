---
name: "diagrams"
displayName: "Diagrams — Professional Draw.io & Multi-Engine Diagramming"
description: "Generate polished, RFP-ready diagrams from natural language. Primary engine: official draw.io MCP with 10,000+ shapes (AWS, Azure, GCP, Cisco, Kubernetes, UML, BPMN, P&ID, electrical). Secondary engines: cloud architecture with real provider icons, Mermaid, PlantUML, and 30+ diagram types via Kroki. Outputs editable .drawio files, PNG, SVG, PDF."
keywords: ["diagram", "flowchart", "sequence diagram", "architecture", "UML", "mermaid", "plantuml", "ERD", "cloud diagram", "AWS architecture", "GCP architecture", "Azure architecture", "kubernetes", "C4", "BPMN", "class diagram", "state diagram", "Gantt", "mindmap", "D2", "graphviz", "kroki", "infrastructure diagram", "component diagram", "deployment diagram", "draw.io", "drawio", "RFP", "professional diagram"]
author: "Community"
---

# Diagrams — Professional Draw.io & Multi-Engine Diagramming Power

Generate **polished, RFP-ready diagrams** from natural language. This power prioritizes the official draw.io MCP server for professional-quality output with 10,000+ shapes, proper styling, and editable .drawio files — supplemented by cloud architecture, Mermaid, and PlantUML engines for specialized use cases.

## Why Draw.io First?

The official draw.io MCP produces diagrams that are:
- **Professional quality** — Proper spacing, alignment, styling, and icons suitable for RFPs, proposals, and client-facing documentation
- **Editable** — Output is native .drawio XML that can be opened and refined in diagrams.net
- **Icon-rich** — Access to 10,000+ shapes including AWS, Azure, GCP, Cisco, Kubernetes, UML, BPMN, P&ID, electrical, network, and more
- **Template-based** — Leverages draw.io's built-in templates and shape libraries for consistent, polished output

---

## Rendering Engines (Priority Order)

| Priority | Engine | Best For | Output Quality |
|----------|--------|----------|----------------|
| **1st (Primary)** | **drawio** | All professional diagrams — architecture, flowcharts, network, UML, BPMN, org charts | ⭐⭐⭐⭐⭐ RFP-ready, editable .drawio files |
| **2nd** | **drawio-aws** | AWS/Kubernetes architecture with official icons, VPC/subnet layouts | ⭐⭐⭐⭐⭐ Professional with official AWS icons |
| **3rd** | **diagrams-mcp** | Quick cloud architecture diagrams with real provider icons (Python-based) | ⭐⭐⭐⭐ Good for quick visualization |
| **4th** | **mcp-mermaid** | Quick Mermaid diagrams with themes, offline use | ⭐⭐⭐ Good for developer docs |
| **5th** | **uml-mcp** | Exotic types (D2, Graphviz, BPMN, C4, TikZ, Nomnoml, etc.) | ⭐⭐⭐ Functional but basic styling |

---

# Onboarding

## Step 1: Verify MCP Server Connectivity

The power uses five MCP servers — two draw.io servers (primary), two remote servers, and one local:

**Draw.io servers (primary — professional quality):**
- `drawio` — Official draw.io MCP Tool Server via `npx @drawio/mcp` (opens in draw.io editor, supports XML/CSV/Mermaid)
- `drawio-aws` — AWS-focused draw.io MCP via npx from GitHub release (generates .drawio files with official AWS icons)

**Remote servers (secondary — no install needed):**
- `diagrams-mcp` at `https://diagrams-mcp-production.up.railway.app/mcp`
- `uml-mcp` at `https://uml-mcp.vercel.app/mcp`

**Local server (offline fallback):**
- `mcp-mermaid` via `npx -y mcp-mermaid`

**Prerequisites:**
- Node.js v18+ required: `node --version`
- npx available: `npx --version`

## Step 2: Understand the Server Roles — Decision Tree

**ALWAYS start with draw.io servers for professional output:**

1. **Need a polished, RFP-ready diagram of ANY type?** → Use `drawio` (`open_drawio_xml` with proper XML)
2. **Need AWS architecture with official icons, VPCs, subnets?** → Use `drawio-aws` (generates .drawio files locally)
3. **Need to find the right shape/icon before creating?** → Use `drawio` (`search_shapes` to find from 10,000+ shapes)
4. **Need quick cloud architecture visualization with Python?** → Use `diagrams-mcp` (`render_diagram`)
5. **Need quick Mermaid with custom themes?** → Use `mcp-mermaid` (`mermaid_to_image`)
6. **Need exotic diagram types (D2, Graphviz, BPMN via Kroki)?** → Use `uml-mcp` (`generate_uml`)
7. **Need to convert Mermaid to editable draw.io?** → Use `drawio` (`open_drawio_mermaid`)
8. **Need to convert CSV/tabular data to diagram?** → Use `drawio` (`open_drawio_csv`)
9. **Working offline?** → Use `mcp-mermaid` (local npx)

**Key Principle**: For anything client-facing, RFP, or presentation — ALWAYS use `drawio` or `drawio-aws`. The other engines are for quick developer-facing diagrams.

## Step 3: Quick Test

Try generating a professional diagram:
- Ask: "Create a professional AWS architecture diagram with CloudFront, API Gateway, Lambda, and DynamoDB"
- The agent should use `drawio-aws` for this and produce an editable .drawio file with official AWS icons

---

# When to Load Steering Files

- Choosing which diagram type/engine to use → `diagram-type-selection.md`
- Creating draw.io XML diagrams (professional quality) → `drawio-xml-guide.md`
- Writing Mermaid diagrams (flowcharts, sequence, class, ER, state, Gantt) → `mermaid-reference.md`
- Writing PlantUML or UML diagrams → `plantuml-reference.md`
- Creating cloud architecture diagrams (AWS, GCP, Azure, K8s) → `cloud-architecture-guide.md`
- Planning a documentation project with multiple diagrams → `workflow-patterns.md`

---

# Tool Usage Guide

## drawio — Official Draw.io MCP Tool Server (PRIMARY)

The official draw.io MCP server. Produces professional-quality diagrams that open directly in the draw.io editor. Supports XML, CSV, and Mermaid input formats.

### Tools

- **`open_drawio_xml`** — Opens draw.io editor with XML content. This is the primary tool for professional diagrams.
  - Parameters: `content` (string, required — draw.io XML), `lightbox` (boolean, optional — read-only view), `dark` (string, optional — "auto"/"true"/"false")
  
- **`open_drawio_csv`** — Opens draw.io editor with CSV data converted to a diagram (org charts, flowcharts from tabular data).
  - Parameters: `content` (string, required — CSV content), `lightbox` (boolean, optional), `dark` (string, optional)

- **`open_drawio_mermaid`** — Opens draw.io editor with a Mermaid.js diagram converted to editable draw.io format.
  - Parameters: `content` (string, required — Mermaid.js syntax), `lightbox` (boolean, optional), `dark` (string, optional)

- **`search_shapes`** — Searches 10,000+ shapes across ALL draw.io libraries by keyword. Returns exact style strings for use in XML.
  - Use this BEFORE creating diagrams to find the correct shape/icon
  - Libraries include: AWS, Azure, GCP, Cisco, Kubernetes, UML, BPMN, P&ID, electrical, network, flowchart, and more

### How It Works
1. Generate draw.io XML (using the xml-reference patterns in the steering file)
2. Call `open_drawio_xml` with the XML content
3. The diagram opens in the draw.io editor in the user's browser
4. User can edit, export to PNG/SVG/PDF, or save as .drawio file

### Quality Tips
- Always use `search_shapes` first to find correct icon style strings
- Use proper containers/groups for visual hierarchy
- Apply consistent styling (colors, fonts, spacing)
- Use edge routing for clean connections (orthogonal for architecture, curved for flows)

## drawio-aws — AWS Draw.io MCP Server

Generates .drawio files locally with official AWS architecture icons. Produces files that can be opened and edited in diagrams.net/draw.io desktop.

### Key Features
- Creates architecture diagrams with official AWS, Kubernetes icons
- Generates native .drawio files on your local machine
- Supports VPC layouts, subnet structures, multi-AZ deployments
- 100% local operation — no network dependencies for generation
- Output files are fully editable in draw.io

### Usage
Ask for AWS architecture diagrams and this server generates proper .drawio files with official icons, proper grouping (VPCs, subnets, AZs), and professional layout.

## diagrams-mcp — Cloud Architecture + Mermaid + PlantUML (Secondary)

### Discovery Tools
- `list_providers()` — List all cloud providers (aws, gcp, azure, k8s, onprem, etc.)
- `list_services(provider)` — List service categories
- `list_nodes(provider, service)` — List node classes with import paths
- `search_nodes(query)` — Search nodes by keyword
- `find_equivalent(node, target_provider)` — Find equivalent across providers
- `list_categories()` — List all infrastructure role categories

### Rendering Tools
- `render_diagram(code)` — Execute Python diagrams script, returns PNG
- `render_mermaid(definition)` — Render Mermaid definition, returns PNG/SVG
- `render_plantuml(definition)` — Render PlantUML definition, returns PNG

## uml-mcp — 30+ Diagram Types via Kroki (Specialized)

### Tools
- `generate_uml(code, diagram_type, output_format)` — Render any diagram type
- `validate_uml(code, diagram_type)` — Validate syntax before rendering
- `list_diagram_types()` — List all supported types
- `generate_uml_batch(diagrams)` — Generate multiple diagrams in one call

### Supported Types
UML (PlantUML): Class, Sequence, Activity, Use Case, State, Component, Deployment, Object
General: Mermaid, D2, Graphviz, ERD, BlockDiag, BPMN, C4
Specialized: TikZ, Excalidraw, Nomnoml, Pikchr, Structurizr, SVGBob, WaveDrom, WireViz

## mcp-mermaid — Themed Mermaid (Offline Fallback)

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
