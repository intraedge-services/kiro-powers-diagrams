# Diagram Type Selection Guide

Use this guide to choose the right diagram type and rendering engine for your task.

## Quick Decision Matrix

| I need to show... | Diagram Type | Engine | Tool |
|-------------------|-------------|--------|------|
| Cloud infrastructure (AWS/GCP/Azure) | Cloud Architecture | diagrams-mcp | `render_diagram` |
| Process flow or decision logic | Flowchart | mcp-mermaid or diagrams-mcp | `mermaid_to_image` / `render_mermaid` |
| How components communicate over time | Sequence Diagram | uml-mcp or mcp-mermaid | `generate_uml` / `mermaid_to_image` |
| Object relationships and inheritance | Class Diagram | uml-mcp | `generate_uml` (plantuml) |
| Database schema and relationships | ERD | mcp-mermaid or uml-mcp | `mermaid_to_image` / `generate_uml` |
| System states and transitions | State Diagram | mcp-mermaid or uml-mcp | `mermaid_to_image` / `generate_uml` |
| High-level system architecture (C4) | C4 Diagram | uml-mcp | `generate_uml` (c4plantuml) |
| Business process workflow | BPMN | uml-mcp | `generate_uml` (bpmn) |
| Project timeline | Gantt Chart | mcp-mermaid | `mermaid_to_image` |
| Concept relationships | Mind Map | mcp-mermaid | `mermaid_to_image` |
| Network topology | Network Diagram | diagrams-mcp | `render_diagram` |
| Deployment architecture | Deployment Diagram | uml-mcp or diagrams-mcp | `generate_uml` / `render_diagram` |
| Component dependencies | Component Diagram | uml-mcp | `generate_uml` (plantuml) |
| User interactions with system | Use Case Diagram | uml-mcp | `generate_uml` (plantuml) |
| Activity workflow | Activity Diagram | uml-mcp | `generate_uml` (plantuml) |
| Data flow between systems | D2 Diagram | uml-mcp | `generate_uml` (d2) |
| Graph/network visualization | Graphviz | uml-mcp | `generate_uml` (graphviz) |
| Chronological events | Timeline | mcp-mermaid | `mermaid_to_image` |
| Proportional data | Pie Chart | mcp-mermaid | `mermaid_to_image` |
| Priority/categorization | Quadrant Chart | mcp-mermaid | `mermaid_to_image` |

## Engine Selection Priority

**IMPORTANT**: Always prefer `diagrams-mcp` for PlantUML and Mermaid rendering. It returns reliable hosted image links (Railway). Only use `uml-mcp` for diagram types that `diagrams-mcp` doesn't support.

| Diagram Type | Primary Engine | Fallback |
|-------------|---------------|----------|
| Cloud Architecture | diagrams-mcp (`render_diagram`) | — |
| PlantUML (sequence, class, component, state, deployment, activity, use case) | diagrams-mcp (`render_plantuml`) | uml-mcp (kroki.io links — may have availability issues) |
| Mermaid (flowchart, ER, Gantt, state, sequence, class, pie, mindmap) | diagrams-mcp (`render_mermaid`) | mcp-mermaid (local, themed) |
| D2, Graphviz, BPMN, C4, TikZ, Nomnoml, Pikchr, etc. | uml-mcp (`generate_uml`) | — |
| Batch generation (multiple diagrams) | uml-mcp (`generate_uml_batch`) | — |

## Engine Selection by Scenario

### Software Architecture Documentation
**Primary**: `diagrams-mcp` for cloud architecture, `uml-mcp` for C4 and component diagrams
- System context → C4 Context (uml-mcp)
- Container view → C4 Container (uml-mcp)
- Component view → C4 Component (uml-mcp)
- Deployment view → Cloud Architecture (diagrams-mcp)
- Infrastructure → Cloud Architecture with real icons (diagrams-mcp)

### API and Service Design
**Primary**: `uml-mcp` for sequence and class diagrams, `mcp-mermaid` for quick flows
- API flow → Sequence Diagram
- Data models → Class Diagram or ERD
- State machines → State Diagram
- Error flows → Flowchart

### DevOps and Infrastructure
**Primary**: `diagrams-mcp` for infrastructure, `mcp-mermaid` for CI/CD flows
- AWS/GCP/Azure architecture → Cloud Architecture (diagrams-mcp)
- Kubernetes topology → Cloud Architecture with K8s provider (diagrams-mcp)
- CI/CD pipeline → Flowchart (mcp-mermaid)
- Deployment strategy → Deployment Diagram (uml-mcp)

### Project Planning
**Primary**: `mcp-mermaid` for timelines and flows
- Project schedule → Gantt Chart
- Feature roadmap → Timeline
- Decision process → Flowchart
- Stakeholder analysis → Quadrant Chart

### Database Design
**Primary**: `mcp-mermaid` for ERDs, `uml-mcp` for complex schemas
- Simple schema → Mermaid ERD (mcp-mermaid)
- Complex schema with constraints → PlantUML ERD (uml-mcp)
- Data flow → D2 Diagram (uml-mcp)

## Output Format Selection

| Need | Format | Engine Support |
|------|--------|---------------|
| Embed in markdown/docs | PNG | All engines |
| Scalable for presentations | SVG | uml-mcp, mcp-mermaid |
| Share via link | URL | mcp-mermaid (svg_url, png_url) |
| Print-quality | PDF | uml-mcp |
| Inline in chat | base64 | uml-mcp, mcp-mermaid |
| Save to project | file | mcp-mermaid, diagrams-mcp |

## When to Use Multiple Engines Together

For comprehensive documentation projects, combine engines:

1. **System overview**: diagrams-mcp (cloud architecture with real icons)
2. **Detailed interactions**: uml-mcp (sequence diagrams via PlantUML)
3. **Quick developer docs**: mcp-mermaid (flowcharts, ERDs in markdown)
4. **Batch generation**: uml-mcp (`generate_uml_batch` for multiple diagrams at once)
