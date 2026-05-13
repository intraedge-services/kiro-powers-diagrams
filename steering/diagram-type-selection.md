# Diagram Type Selection Guide

Use this guide to choose the right diagram type and rendering engine for your task.

## Golden Rule: Draw.io First

**For any diagram that will be seen by clients, stakeholders, or included in RFPs/proposals — ALWAYS use the `drawio` or `drawio-aws` server.** These produce professional, editable output with proper icons and styling.

Use the other engines (diagrams-mcp, mcp-mermaid, uml-mcp) only for:
- Quick developer-facing documentation
- Rapid prototyping/iteration
- Exotic diagram types not supported by draw.io
- Offline scenarios (mcp-mermaid)

---

## Quick Decision Matrix

| I need to show... | Diagram Type | Engine | Tool | Quality |
|-------------------|-------------|--------|------|---------|
| AWS/Azure/GCP architecture (RFP-ready) | Cloud Architecture | **drawio-aws** | (auto) | ⭐⭐⭐⭐⭐ |
| Any professional diagram with icons | Architecture/Flow | **drawio** | `open_drawio_xml` | ⭐⭐⭐⭐⭐ |
| Network topology with Cisco/vendor icons | Network Diagram | **drawio** | `open_drawio_xml` + `search_shapes` | ⭐⭐⭐⭐⭐ |
| Org chart from data | Org Chart | **drawio** | `open_drawio_csv` | ⭐⭐⭐⭐⭐ |
| Process flow or decision logic | Flowchart | **drawio** | `open_drawio_xml` or `open_drawio_mermaid` | ⭐⭐⭐⭐⭐ |
| How components communicate over time | Sequence Diagram | **drawio** | `open_drawio_mermaid` or `open_drawio_xml` | ⭐⭐⭐⭐⭐ |
| Object relationships and inheritance | Class Diagram | **drawio** | `open_drawio_xml` (UML shapes) | ⭐⭐⭐⭐⭐ |
| Database schema and relationships | ERD | **drawio** | `open_drawio_xml` | ⭐⭐⭐⭐⭐ |
| System states and transitions | State Diagram | **drawio** | `open_drawio_mermaid` | ⭐⭐⭐⭐ |
| High-level system architecture (C4) | C4 Diagram | **drawio** | `open_drawio_xml` | ⭐⭐⭐⭐⭐ |
| Business process workflow | BPMN | **drawio** | `open_drawio_xml` + `search_shapes("bpmn")` | ⭐⭐⭐⭐⭐ |
| Quick cloud viz (developer docs) | Cloud Architecture | diagrams-mcp | `render_diagram` | ⭐⭐⭐⭐ |
| Quick Mermaid (developer docs) | Flowchart/Sequence | mcp-mermaid | `mermaid_to_image` | ⭐⭐⭐ |
| Project timeline | Gantt Chart | mcp-mermaid | `mermaid_to_image` | ⭐⭐⭐ |
| Concept relationships | Mind Map | mcp-mermaid | `mermaid_to_image` | ⭐⭐⭐ |
| Exotic types (D2, Graphviz, TikZ) | Specialized | uml-mcp | `generate_uml` | ⭐⭐⭐ |
| Batch generation (multiple at once) | Multiple | uml-mcp | `generate_uml_batch` | ⭐⭐⭐ |

---

## Engine Selection by Use Case

### RFP / Proposal / Client-Facing Documentation
**ALWAYS use `drawio` or `drawio-aws`**

Why: Professional styling, proper icons, editable output, consistent branding

| Diagram Need | Approach |
|-------------|----------|
| AWS architecture | `drawio-aws` — generates .drawio with official AWS icons |
| Multi-cloud architecture | `drawio` — use `search_shapes` for each provider's icons |
| System architecture | `drawio` — `open_drawio_xml` with containers and proper layout |
| Network topology | `drawio` — `search_shapes("cisco")` for network icons |
| Business process | `drawio` — `search_shapes("bpmn")` for BPMN shapes |
| Data flow | `drawio` — `open_drawio_xml` with proper edge routing |
| Org chart | `drawio` — `open_drawio_csv` with hierarchical data |

### Developer Documentation (Internal)
**Use any engine — speed over polish**

| Diagram Need | Fastest Approach |
|-------------|-----------------|
| Quick flowchart | `mcp-mermaid` or `drawio` (`open_drawio_mermaid`) |
| API sequence diagram | `mcp-mermaid` (Mermaid sequence syntax) |
| ERD for README | `mcp-mermaid` (Mermaid ER syntax) |
| Architecture overview | `diagrams-mcp` (`render_diagram` with Python) |
| State machine | `mcp-mermaid` (Mermaid state syntax) |

### Architecture Design Sessions
**Use `drawio` for the final version, `diagrams-mcp` for quick iterations**

1. **Iterate quickly** with `diagrams-mcp` (Python code → PNG)
2. **Finalize** with `drawio` (`open_drawio_xml` with proper styling)
3. **Share** the .drawio file for team editing

---

## Shape Discovery Workflow

Before creating any draw.io XML diagram, discover the right shapes:

```
1. search_shapes("aws lambda")     → Get exact style string for Lambda icon
2. search_shapes("aws vpc")        → Get VPC container style
3. search_shapes("kubernetes pod") → Get K8s pod icon
4. search_shapes("cisco router")   → Get Cisco router icon
5. search_shapes("bpmn gateway")   → Get BPMN decision gateway
```

The `search_shapes` tool returns exact style strings that you paste directly into the `style` attribute of `mxCell` elements. This ensures you get the correct, high-quality icons every time.

---

## Output Format Selection

| Need | Best Engine | Format |
|------|-------------|--------|
| Editable diagram for team | **drawio** | .drawio file (XML) |
| Embed in RFP/proposal | **drawio** → export to PNG/PDF | PNG or PDF |
| Embed in markdown/docs | diagrams-mcp or mcp-mermaid | PNG |
| Scalable for presentations | **drawio** → export to SVG | SVG |
| Share via link | mcp-mermaid | URL (svg_url, png_url) |
| Print-quality | **drawio** → export to PDF | PDF |
| Inline in chat | mcp-mermaid or uml-mcp | base64 |
| Save to project repo | **drawio** or mcp-mermaid (file) | .drawio or PNG |

---

## When to Use Multiple Engines Together

For comprehensive documentation projects:

1. **Final deliverable diagrams** → `drawio` or `drawio-aws` (professional quality)
2. **Quick iteration/drafts** → `diagrams-mcp` or `mcp-mermaid` (fast feedback)
3. **Batch generation for dev docs** → `uml-mcp` (`generate_uml_batch`)
4. **Convert draft to polished** → Generate in Mermaid, then use `open_drawio_mermaid` to get editable draw.io version
