# Requirements Document — Kiro Diagramming Power

## Intent Analysis

| Attribute | Value |
|-----------|-------|
| **User Request** | Create the best Kiro Power for diagramming with deep research and development |
| **Request Type** | New Project |
| **Scope Estimate** | Single Component (Kiro Power package) |
| **Complexity Estimate** | Moderate (multi-server MCP configuration + comprehensive steering) |

---

## Functional Requirements

### FR-1: Multi-Engine Diagram Support
The power MUST support ALL major diagram types through multiple MCP rendering engines:
- **Mermaid**: Flowcharts, sequence, class, ER, state, Gantt, pie, mindmap, timeline, quadrant
- **PlantUML**: Class, sequence, activity, use case, state, component, deployment, object
- **Kroki**: D2, Graphviz, ERD, BPMN, C4, TikZ, BlockDiag, Nomnoml, Pikchr, Structurizr, SVGBob, WaveDrom
- **Cloud Architecture**: AWS, GCP, Azure, Kubernetes, On-Premise with real provider icons (mingrammer/diagrams)

### FR-2: Multiple Output Formats
The power MUST support all major output formats:
- PNG images (saved to disk for documentation, PRs, wikis)
- SVG files (scalable, editable, embeddable)
- Interactive URLs/links (shareable, editable online via playground)
- Base64 encoded output (for inline embedding)

### FR-3: Diagram Validation
The power MUST validate diagram syntax before rendering to enable iterative correction by the AI agent.

### FR-4: Batch Diagram Generation
The power SHOULD support generating multiple diagrams in a single request for efficiency.

### FR-5: Cloud Provider Node Discovery
The power MUST provide tools to discover available cloud provider nodes (AWS, GCP, Azure, K8s) and search for specific services.

### FR-6: Cross-Provider Equivalence
The power SHOULD support finding equivalent services across cloud providers (e.g., EC2 → ComputeEngine).

---

## Non-Functional Requirements

### NFR-1: Hybrid Deployment
- **Primary**: Remote hosted MCP servers (zero installation, fastest setup)
- **Fallback**: Local execution via npx/uvx for offline use
- MCP servers configured:
  - `diagrams-mcp` (ByteOverDev) — remote via Railway URL + local via uvx
  - `uml-mcp` (antoinebou12) — remote via Vercel URL
  - `mcp-mermaid` (hustcc) — local via npx (lightweight Mermaid-specific)

### NFR-2: Zero-Friction Setup
- Remote servers require NO local installation (just HTTP URLs)
- Local fallback requires only `npx` or `uvx` (standard dev tools)
- No API keys required for core functionality

### NFR-3: Comprehensive Steering
The power MUST include steering files covering:
- Diagram type selection guide (when to use which engine/type)
- Syntax quick-reference for each major diagram type
- Workflow patterns (architecture-first, code-first, documentation)
- Best practices for diagram quality and readability

### NFR-4: General-Purpose Use Cases
The power MUST serve all diagramming use cases:
- Software architecture documentation
- Development workflow diagrams
- Project documentation (ERDs, user flows, processes)
- Cloud infrastructure visualization
- System design and component diagrams

---

## MCP Server Architecture

### Primary Servers (Remote — No Install)

| Server | URL | Purpose |
|--------|-----|---------|
| diagrams-mcp | `https://diagrams-mcp-production.up.railway.app/mcp` | Cloud architecture with real icons + Mermaid + PlantUML |
| uml-mcp | `https://uml-mcp.vercel.app/mcp` | 30+ diagram types via Kroki/PlantUML/Mermaid |

### Fallback Server (Local — Offline)

| Server | Command | Purpose |
|--------|---------|---------|
| mcp-mermaid | `npx -y mcp-mermaid` | Lightweight Mermaid rendering with themes |

---

## Power Structure

```
power-diagrams/
├── POWER.md                          # Metadata, onboarding, steering mappings
├── mcp.json                          # MCP server configuration (3 servers)
└── steering/
    ├── diagram-type-selection.md     # When to use which diagram type/engine
    ├── mermaid-reference.md          # Mermaid syntax quick-reference
    ├── plantuml-reference.md         # PlantUML syntax quick-reference
    ├── cloud-architecture-guide.md   # Cloud diagram patterns and best practices
    └── workflow-patterns.md          # Diagramming workflow patterns
```

---

## Acceptance Criteria

1. Power activates when user mentions keywords: diagram, flowchart, sequence, architecture, UML, ERD, mermaid, plantuml, cloud diagram, etc.
2. All three MCP servers connect successfully (remote primary, local fallback)
3. User can generate diagrams of any supported type through natural language
4. Output is available in PNG, SVG, URL, and base64 formats
5. Steering files provide actionable guidance for diagram type selection
6. Power works with zero installation for remote mode
7. Power works offline with npx for local mode
