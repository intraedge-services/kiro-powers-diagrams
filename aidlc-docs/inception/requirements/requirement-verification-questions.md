# Requirements Verification Questions - Kiro Diagramming Power

Based on my deep research into available diagramming MCP servers, I've identified the top candidates. Please answer the following questions to help me build the best Kiro Power for diagramming.

## Research Summary

I evaluated the following MCP diagramming servers:

| Server | Strengths | Diagram Types | Setup |
|--------|-----------|---------------|-------|
| **Mermaid MCP (Official)** | Cloud-hosted, no install, validation + rendering, playground links | Mermaid only (flowcharts, sequence, class, ER, state, Gantt, pie, etc.) | HTTP remote - zero install |
| **mcp-mermaid (hustcc)** | 214⭐, full Mermaid support, themes, multiple output formats (base64, SVG, file, URL) | Mermaid only | npx - lightweight |
| **UML-MCP (antoinebou12)** | 30+ diagram types, Kroki+PlantUML+Mermaid, batch generation, validation | UML, Mermaid, D2, Graphviz, ERD, BPMN, C4, TikZ, and more | HTTP remote or local Python |
| **diagrams-mcp (ByteOverDev)** | Cloud architecture with real provider icons (AWS/GCP/Azure/K8s), plus Mermaid + PlantUML | Cloud architecture, flowcharts, sequence, class, ER, state, Gantt, UML | HTTP remote or uvx |

---

## Question 1
Which diagramming approach best fits your needs?

A) **Comprehensive multi-engine** — Support ALL diagram types (UML, Mermaid, PlantUML, cloud architecture, D2, Graphviz, ERD, BPMN, C4) via multiple MCP servers combined into one power
B) **Cloud architecture focused** — Primarily AWS/GCP/Azure/K8s architecture diagrams with real provider icons, plus basic flowcharts
C) **Mermaid-focused** — Primarily Mermaid diagrams (flowcharts, sequence, class, ER, state, Gantt) with validation and rendering
D) **UML-focused** — Primarily UML diagrams (class, sequence, activity, use case, state, component, deployment) via PlantUML/Kroki
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 2
What output format matters most to you?

A) PNG images saved to disk (for documentation, PRs, wikis)
B) SVG files (scalable, editable, embeddable in web)
C) Interactive links/URLs (shareable, editable online)
D) All of the above — maximum flexibility
X) Other (please describe after [Answer]: tag below)

[Answer]: D

## Question 3
How should the MCP server be deployed?

A) **Remote/hosted only** — Zero installation, connect to cloud-hosted servers (fastest setup, requires internet)
B) **Local only** — Run locally via npx/uvx (works offline, more control)
C) **Hybrid** — Use remote hosted servers as primary with local fallback option
X) Other (please describe after [Answer]: tag below)

[Answer]: C

## Question 4
Should the power include steering files with best practices for diagram creation?

A) Yes — Include comprehensive steering with diagram type selection guides, syntax references, and workflow patterns
B) Minimal — Just include basic onboarding and tool descriptions
C) No — Just the MCP server configuration, no steering files
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 5
What's the primary use case for this diagramming power?

A) Software architecture documentation (system design, component diagrams, deployment views)
B) Development workflow diagrams (flowcharts, sequence diagrams, state machines)
C) Project documentation (ERDs, user flows, process diagrams, Gantt charts)
D) All of the above — general-purpose diagramming for any software project
X) Other (please describe after [Answer]: tag below)

[Answer]: D

## Question 6: Security Extensions
Should security extension rules be enforced for this project?

A) Yes — enforce all SECURITY rules as blocking constraints (recommended for production-grade applications)
B) No — skip all SECURITY rules (suitable for PoCs, prototypes, and experimental projects)
X) Other (please describe after [Answer]: tag below)

[Answer]: B

## Question 7: Property-Based Testing Extension
Should property-based testing (PBT) rules be enforced for this project?

A) Yes — enforce all PBT rules as blocking constraints (recommended for projects with business logic, data transformations, serialization, or stateful components)
B) Partial — enforce PBT rules only for pure functions and serialization round-trips (suitable for projects with limited algorithmic complexity)
C) No — skip all PBT rules (suitable for simple CRUD applications, UI-only projects, or thin integration layers with no significant business logic)
X) Other (please describe after [Answer]: tag below)

[Answer]: C
