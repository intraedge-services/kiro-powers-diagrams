# Code Summary — Kiro Diagramming Power

## Generated Files

| File | Path | Purpose |
|------|------|---------|
| POWER.md | `power-diagrams/POWER.md` | Power metadata, onboarding, steering mappings, tool guide |
| mcp.json | `power-diagrams/mcp.json` | MCP server configuration (3 servers) |
| Diagram Type Selection | `power-diagrams/steering/diagram-type-selection.md` | Decision matrix for choosing diagram types |
| Mermaid Reference | `power-diagrams/steering/mermaid-reference.md` | Mermaid syntax quick-reference |
| PlantUML Reference | `power-diagrams/steering/plantuml-reference.md` | PlantUML/Kroki syntax quick-reference |
| Cloud Architecture Guide | `power-diagrams/steering/cloud-architecture-guide.md` | Cloud diagram patterns (AWS/GCP/Azure/K8s) |
| Workflow Patterns | `power-diagrams/steering/workflow-patterns.md` | Diagramming workflow patterns |

## Power Structure

```
kiro-powers-diagrams/                     # Repository root (= power root)
├── POWER.md                              # Metadata, onboarding, steering mappings
├── mcp.json                              # MCP server configuration (3 servers)
├── steering/                             # Steering files (loaded contextually by Kiro)
│   ├── diagram-type-selection.md         # Decision matrix with 20+ diagram types
│   ├── mermaid-reference.md              # 10 diagram types with syntax examples
│   ├── plantuml-reference.md             # 7 diagram types with full examples
│   ├── cloud-architecture-guide.md       # 5 architecture patterns with code
│   └── workflow-patterns.md              # 5 workflow patterns for different scenarios
├── README.md                             # User documentation
├── LICENSE                               # MIT license
└── aidlc-docs/                           # AI-DLC development process documentation
```

**Critical**: `POWER.md` MUST be at the repository root. Kiro looks for it there when installing from GitHub.

## MCP Servers

| Server | Type | URL/Command | Capabilities |
|--------|------|-------------|-------------|
| diagrams-mcp | Remote HTTP | `https://diagrams-mcp-production.up.railway.app/mcp` | Cloud architecture (real icons), Mermaid, PlantUML |
| uml-mcp | Remote HTTP | `https://uml-mcp.vercel.app/mcp` | 30+ diagram types via Kroki |
| mcp-mermaid | Local stdio | `npx -y mcp-mermaid` | Mermaid with themes, offline fallback |

## Installation

### From Local Path
1. Open Kiro → Powers panel → Add power from Local Path
2. Select the `power-diagrams/` directory
3. Power activates automatically when you mention diagram-related keywords

### From GitHub (after pushing)
1. Push `power-diagrams/` to a public GitHub repository
2. Others install via: Add power from GitHub → enter repository URL

## Keywords That Activate This Power
diagram, flowchart, sequence diagram, architecture, UML, mermaid, plantuml, ERD, cloud diagram, AWS architecture, GCP architecture, Azure architecture, kubernetes, C4, BPMN, class diagram, state diagram, Gantt, mindmap, D2, graphviz, kroki, infrastructure diagram, component diagram, deployment diagram
