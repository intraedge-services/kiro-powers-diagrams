# kiro-powers-diagrams

A Kiro Power for generating **professional, RFP-ready diagrams** from natural language. Prioritizes the official draw.io MCP server for polished output with 10,000+ shapes, supplemented by cloud architecture, Mermaid, and PlantUML engines.

## Why This Power?

Most AI-generated diagrams look basic — fine for developer docs but not suitable for RFPs, proposals, or client-facing documentation. This power solves that by using **draw.io as the primary engine**, producing:

- **Professional quality** — Proper spacing, alignment, icons, and styling
- **Editable output** — Native .drawio files you can refine in diagrams.net
- **10,000+ shapes** — AWS, Azure, GCP, Cisco, Kubernetes, UML, BPMN, P&ID, electrical, and more
- **Multiple formats** — Export to PNG, SVG, PDF from draw.io

For quick developer-facing diagrams, the power also includes cloud architecture (Python-based with real icons), Mermaid, and 30+ diagram types via Kroki.

## Rendering Engines

| Priority | Engine | Capabilities | Quality |
|----------|--------|-------------|---------|
| **1st** | **drawio** (official) | All diagram types, 10,000+ shapes, XML/CSV/Mermaid input | ⭐⭐⭐⭐⭐ RFP-ready |
| **2nd** | **drawio-aws** (AWS samples) | AWS/K8s architecture with official icons, .drawio output | ⭐⭐⭐⭐⭐ Professional |
| **3rd** | **diagrams-mcp** | Cloud architecture with real provider icons (Python) | ⭐⭐⭐⭐ Good |
| **4th** | **mcp-mermaid** | Mermaid with themes, offline support | ⭐⭐⭐ Developer docs |
| **5th** | **uml-mcp** | 30+ types via Kroki (D2, Graphviz, BPMN, C4, TikZ) | ⭐⭐⭐ Functional |

## Prerequisites

- **Kiro IDE** — [Download from kiro.dev](https://kiro.dev/downloads/)
- **Node.js v18+** — Required for draw.io MCP servers and mcp-mermaid
  - Verify: `node --version`
  - Install: [nodejs.org](https://nodejs.org/) or `brew install node`
- **Internet connection** — Required for the two remote MCP servers (diagrams-mcp, uml-mcp)

## Installation

### From GitHub (Recommended)

1. Open Kiro IDE
2. Open the Powers panel (click the Powers icon ⚡)
3. Click **"Add power from GitHub"**
4. Enter the repository URL: `https://github.com/intraedge-services/kiro-powers-diagrams`
5. Click Install — the power installs and MCP servers register automatically

### From Local Path (Development)

1. Clone this repository:
   ```bash
   git clone https://github.com/intraedge-services/kiro-powers-diagrams.git
   ```
2. Open Kiro IDE → Powers panel → **"Add power from Local Path"**
3. Select the repository root directory (the folder containing `POWER.md`)

## Usage

Once installed, mention anything diagram-related in your Kiro chat and the power activates automatically.

### Example Prompts — Professional Quality (Draw.io)

```
"Create a professional AWS architecture diagram with CloudFront, API Gateway, Lambda, DynamoDB, and S3. Show VPC with public and private subnets."

"Create an RFP-ready network topology diagram with Cisco routers, switches, and firewalls"

"Create a polished BPMN diagram for the order fulfillment process"

"Create a C4 container diagram for our microservices platform"

"Convert this Mermaid flowchart to a professional draw.io diagram: flowchart LR; A-->B-->C"
```

### Example Prompts — Quick Developer Diagrams

```
"Quick flowchart showing user registration flow"
"Generate a Mermaid sequence diagram for the checkout API"
"Create a quick AWS architecture with diagrams-mcp showing ECS, RDS, and ElastiCache"
"Create a Gantt chart for the project timeline"
```

### Activation Keywords

diagram, flowchart, sequence diagram, architecture, UML, mermaid, plantuml, ERD, cloud diagram, AWS architecture, GCP architecture, Azure architecture, kubernetes, C4, BPMN, class diagram, state diagram, Gantt, mindmap, D2, graphviz, kroki, infrastructure diagram, component diagram, deployment diagram, draw.io, drawio, RFP, professional diagram

## MCP Server Details

### drawio — Official Draw.io MCP Tool Server (PRIMARY)
- **Command**: `npx -y @drawio/mcp`
- **Tools**: `open_drawio_xml`, `open_drawio_csv`, `open_drawio_mermaid`, `search_shapes`
- **Source**: [jgraph/drawio-mcp](https://github.com/jgraph/drawio-mcp)
- **Quality**: ⭐⭐⭐⭐⭐ — Professional, editable .drawio output with 10,000+ shapes

### drawio-aws — AWS Draw.io MCP Server
- **Command**: `npx -y https://github.com/aws-samples/sample-drawio-mcp/releases/latest/download/drawio-mcp-server-latest.tgz --no-cache`
- **Source**: [aws-samples/sample-drawio-mcp](https://github.com/aws-samples/sample-drawio-mcp)
- **Quality**: ⭐⭐⭐⭐⭐ — Official AWS icons, generates .drawio files locally

### diagrams-mcp — Cloud Architecture (Remote)
- **URL**: `https://diagrams-mcp-production.up.railway.app/mcp`
- **Tools**: `render_diagram`, `render_mermaid`, `render_plantuml`, `list_providers`, `list_services`, `list_nodes`, `search_nodes`, `find_equivalent`, `list_categories`
- **Source**: [ByteOverDev/diagrams-mcp](https://github.com/ByteOverDev/diagrams-mcp)

### uml-mcp — 30+ Diagram Types (Remote)
- **URL**: `https://uml-mcp.vercel.app/mcp`
- **Tools**: `generate_uml`, `validate_uml`, `list_diagram_types`, `generate_uml_batch`
- **Source**: [antoinebou12/uml-mcp](https://github.com/antoinebou12/uml-mcp)

### mcp-mermaid — Themed Mermaid (Local)
- **Command**: `npx -y mcp-mermaid`
- **Tools**: `mermaid_to_image`
- **Source**: [hustcc/mcp-mermaid](https://github.com/hustcc/mcp-mermaid)

## Power Structure

```
kiro-powers-diagrams/
├── POWER.md                              # Metadata, onboarding, tool guide
├── mcp.json                              # MCP server configuration (5 servers)
├── README.md                             # This file
├── LICENSE                               # License
├── steering/                             # Steering files (loaded contextually)
│   ├── diagram-type-selection.md         # When to use which diagram type/engine
│   ├── drawio-xml-guide.md              # Draw.io XML generation reference (KEY)
│   ├── mermaid-reference.md              # Mermaid syntax quick-reference
│   ├── plantuml-reference.md             # PlantUML syntax quick-reference
│   ├── cloud-architecture-guide.md       # Cloud architecture patterns
│   └── workflow-patterns.md              # Diagramming workflow patterns
└── aidlc-docs/                           # Development documentation
```

## Steering Files

| File | Loaded When |
|------|-------------|
| `diagram-type-selection.md` | Choosing which diagram type/engine to use |
| `drawio-xml-guide.md` | Creating professional draw.io XML diagrams |
| `mermaid-reference.md` | Writing Mermaid diagrams |
| `plantuml-reference.md` | Writing PlantUML/UML diagrams |
| `cloud-architecture-guide.md` | Creating cloud architecture diagrams |
| `workflow-patterns.md` | Planning multi-diagram documentation |

## Troubleshooting

### drawio server not working
- Ensure Node.js v18+ is installed: `node --version`
- Test manually: `npx -y @drawio/mcp` (should start without errors)
- The server opens diagrams in your default browser via draw.io

### drawio-aws server not working
- Ensure Node.js v18+ and npx are available
- Test: `npx -y https://github.com/aws-samples/sample-drawio-mcp/releases/latest/download/drawio-mcp-server-latest.tgz --no-cache`
- This server generates .drawio files locally — check your working directory

### Remote servers not responding
- Check your internet connection
- Verify URLs: `curl -s -o /dev/null -w "%{http_code}" https://diagrams-mcp-production.up.railway.app/mcp`

### Power not activating
- Verify the power is installed in Kiro's Powers panel
- Try explicit keywords: "create a professional diagram" or "generate a draw.io architecture"
- Restart Kiro if the power was just installed

## Contributing

1. Fork this repository
2. Make your changes
3. Test locally by installing from local path
4. Submit a pull request

## License

MIT
