# kiro-powers-diagrams

A reusable Kiro Power designed to standardize and automate diagram generation for software architecture, cloud infrastructure, workflows, and engineering documentation across development teams.

## Overview

This power combines **three rendering engines** into a single Kiro Power, giving you access to 30+ diagram types with zero-friction setup:

| Engine | Capabilities | Deployment |
|--------|-------------|------------|
| **diagrams-mcp** | Cloud architecture with real AWS/GCP/Azure/K8s icons + Mermaid + PlantUML | Remote (Railway) |
| **uml-mcp** | 30+ diagram types via Kroki/PlantUML/Mermaid, batch generation, validation | Remote (Vercel) |
| **mcp-mermaid** | Mermaid with themes, multiple output formats | Local (npx) |

## Prerequisites

- **Kiro IDE** — [Download from kiro.dev](https://kiro.dev/downloads/)
- **Node.js v18+** — Required for the local `mcp-mermaid` server (offline fallback)
  - Verify: `node --version`
  - Install: [nodejs.org](https://nodejs.org/) or `brew install node`
- **Internet connection** — Required for the two remote MCP servers (diagrams-mcp, uml-mcp)

> **Note**: If you only have internet access (no Node.js), the remote servers still cover all diagram types including Mermaid. Node.js is only needed for the offline local fallback.

## Installation

### From Local Path (Development)

1. Clone this repository:
   ```bash
   git clone https://github.com/<your-org>/kiro-powers-diagrams.git
   ```
2. Open Kiro IDE
3. Open the Powers panel (click the Ghosty icon with lightning bolt ⚡)
4. Click **"Add power from Local Path"**
5. Select the `power-diagrams/` directory
6. The power is now installed and activates automatically on keyword mention

### From GitHub (Recommended)

1. Open Kiro IDE
2. Open the Powers panel
3. Click **"Add power from GitHub"**
4. Enter the repository URL
5. Done — the power installs and activates automatically

## Usage

Once installed, simply mention anything diagram-related in your Kiro chat and the power activates automatically.

### Example Prompts

```
"Create a flowchart showing user registration flow"
"Draw an AWS architecture with ECS, RDS, and ElastiCache"
"Generate a sequence diagram for the checkout API"
"Create a C4 container diagram for our microservices"
"Make an ERD for the order management system"
"Create a Kubernetes deployment diagram"
"Generate a Gantt chart for the project timeline"
```

### Activation Keywords

The power activates when you mention: diagram, flowchart, sequence diagram, architecture, UML, mermaid, plantuml, ERD, cloud diagram, AWS architecture, GCP architecture, Azure architecture, kubernetes, C4, BPMN, class diagram, state diagram, Gantt, mindmap, D2, graphviz, kroki, infrastructure diagram, component diagram, deployment diagram

## Supported Diagram Types

### Cloud Architecture (diagrams-mcp)
- AWS, GCP, Azure, Kubernetes, On-Premise with **real provider icons**
- Supports all providers from [mingrammer/diagrams](https://diagrams.mingrammer.com/)

### UML & General (uml-mcp via Kroki)
- **UML**: Class, Sequence, Activity, Use Case, State, Component, Deployment, Object
- **General**: Mermaid, D2, Graphviz, ERD, BlockDiag, BPMN, C4
- **Specialized**: TikZ, Excalidraw, Nomnoml, Pikchr, Structurizr, SVGBob, WaveDrom, WireViz

### Mermaid (mcp-mermaid)
- Flowcharts, Sequence, Class, ER, State, Gantt, Pie, Mindmap, Timeline, Quadrant

## Output Formats

| Format | Use Case | Supported By |
|--------|----------|-------------|
| PNG | Documentation, PRs, wikis | All engines |
| SVG | Scalable, web embedding | uml-mcp, mcp-mermaid |
| PDF | Print-quality documents | uml-mcp |
| Base64 | Inline embedding | uml-mcp, mcp-mermaid |
| URL | Shareable links | mcp-mermaid |
| File | Save to disk | mcp-mermaid, diagrams-mcp |

## Power Structure

```
power-diagrams/
├── POWER.md                              # Metadata, onboarding, tool guide
├── mcp.json                              # MCP server configuration
└── steering/
    ├── diagram-type-selection.md         # When to use which diagram type
    ├── mermaid-reference.md              # Mermaid syntax quick-reference
    ├── plantuml-reference.md             # PlantUML syntax quick-reference
    ├── cloud-architecture-guide.md       # Cloud architecture patterns
    └── workflow-patterns.md              # Diagramming workflow patterns
```

## Steering Files

The power includes comprehensive steering files that Kiro loads contextually:

| File | Loaded When |
|------|-------------|
| `diagram-type-selection.md` | Choosing which diagram type to use |
| `mermaid-reference.md` | Writing Mermaid diagrams |
| `plantuml-reference.md` | Writing PlantUML/UML diagrams |
| `cloud-architecture-guide.md` | Creating cloud architecture diagrams |
| `workflow-patterns.md` | Planning multi-diagram documentation |

## MCP Server Details

### diagrams-mcp (Remote — No Install)
- **URL**: `https://diagrams-mcp-production.up.railway.app/mcp`
- **Tools**: `render_diagram`, `render_mermaid`, `render_plantuml`, `list_providers`, `list_services`, `list_nodes`, `search_nodes`, `find_equivalent`, `list_categories`
- **Source**: [ByteOverDev/diagrams-mcp](https://github.com/ByteOverDev/diagrams-mcp)

### uml-mcp (Remote — No Install)
- **URL**: `https://uml-mcp.vercel.app/mcp`
- **Tools**: `generate_uml`, `validate_uml`, `list_diagram_types`, `generate_uml_batch`
- **Source**: [antoinebou12/uml-mcp](https://github.com/antoinebou12/uml-mcp)

### mcp-mermaid (Local — Requires Node.js)
- **Command**: `npx -y mcp-mermaid`
- **Tools**: `mermaid_to_image`
- **Source**: [hustcc/mcp-mermaid](https://github.com/hustcc/mcp-mermaid)

## Troubleshooting

### Remote servers not responding
- Check your internet connection
- The servers may be temporarily down — try again in a few minutes
- Verify URLs are accessible: `curl -s -o /dev/null -w "%{http_code}" https://diagrams-mcp-production.up.railway.app/mcp`

### mcp-mermaid not working
- Ensure Node.js v18+ is installed: `node --version`
- Ensure npx is available: `npx --version`
- Try running manually: `npx -y mcp-mermaid` (should start without errors)

### Power not activating
- Verify the power is installed in Kiro's Powers panel
- Try using explicit keywords like "create a diagram" or "generate a flowchart"
- Restart Kiro if the power was just installed

## Contributing

1. Fork this repository
2. Make your changes in the `power-diagrams/` directory
3. Test locally by installing from local path
4. Submit a pull request

## License

MIT
