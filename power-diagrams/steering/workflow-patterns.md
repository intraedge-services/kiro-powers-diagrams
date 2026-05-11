# Diagramming Workflow Patterns

Patterns for integrating diagrams into your development workflow effectively.

## Workflow 1: Architecture-First (Design → Diagram → Code)

Best for: New projects, major features, system redesigns

### Steps
1. **Define requirements** — Understand what the system needs to do
2. **Create C4 Context diagram** — Show system boundaries and external actors (uml-mcp, c4plantuml)
3. **Create C4 Container diagram** — Show major containers/services (uml-mcp, c4plantuml)
4. **Create Cloud Architecture diagram** — Map to actual infrastructure (diagrams-mcp)
5. **Create Sequence diagrams** — Detail key interactions (uml-mcp or mcp-mermaid)
6. **Create ERD** — Define data model (mcp-mermaid)
7. **Implement** — Use diagrams as implementation guide
8. **Update diagrams** — Reflect any implementation changes

### Example Prompt Sequence
```
1. "Create a C4 context diagram for an e-commerce platform with users, payment provider, and shipping service"
2. "Now create a C4 container diagram showing the internal services"
3. "Create an AWS architecture diagram for deploying this on ECS with RDS and ElastiCache"
4. "Create a sequence diagram for the checkout flow"
5. "Create an ERD for the order management data model"
```

## Workflow 2: Code-First (Code → Analyze → Diagram)

Best for: Documenting existing systems, onboarding, code reviews

### Steps
1. **Analyze codebase** — Understand existing structure
2. **Create Component diagram** — Show discovered components (uml-mcp, plantuml)
3. **Create Class diagram** — Document key domain models (uml-mcp, plantuml)
4. **Create Sequence diagrams** — Document critical flows (mcp-mermaid)
5. **Create Deployment diagram** — Document current infrastructure (diagrams-mcp)

### Example Prompt Sequence
```
1. "Analyze the src/ directory and create a component diagram showing the main modules"
2. "Create a class diagram for the domain models in src/models/"
3. "Create a sequence diagram showing how the authentication flow works based on the code"
4. "Create an AWS architecture diagram showing our current deployment"
```

## Workflow 3: Documentation Sprint

Best for: Creating comprehensive docs for a project or feature

### Steps
1. **Plan diagram set** — Decide which diagrams are needed
2. **Batch generate** — Use `generate_uml_batch` for multiple diagrams at once
3. **Review and refine** — Iterate on individual diagrams
4. **Export** — Save in appropriate formats for your docs platform

### Recommended Diagram Set for a Service

| Diagram | Type | Purpose |
|---------|------|---------|
| System Context | C4 | Where this service fits in the ecosystem |
| Container View | C4 | Internal structure of the service |
| Cloud Architecture | diagrams-mcp | Infrastructure and deployment |
| Key Sequence Diagrams (2-3) | Mermaid/PlantUML | Critical user flows |
| ERD | Mermaid | Data model |
| State Diagram | Mermaid | Key state machines |

### Example Batch Generation
```
"Generate a batch of diagrams for the Order Service:
1. A sequence diagram for order creation
2. A sequence diagram for order cancellation
3. A state diagram for order lifecycle
4. An ERD for order-related tables"
```

## Workflow 4: Iterative Refinement

Best for: Getting a diagram just right through multiple passes

### Pattern
1. **Generate initial version** — Quick first draft
2. **Validate** — Use `validate_uml` to check syntax
3. **Refine content** — Add missing elements, fix relationships
4. **Style** — Apply themes, adjust layout direction
5. **Export** — Choose final format

### Tips for Iteration
- Start simple, add complexity incrementally
- Use validation between iterations to catch errors early
- Change `direction` (LR vs TD) to find best layout
- Try different themes with mcp-mermaid (`dark`, `forest`, `neutral`)
- Split overly complex diagrams into focused sub-diagrams

## Workflow 5: Multi-View Architecture Documentation

Best for: Comprehensive architecture documentation (like arc42 or C4)

### Views to Create

| View | Diagram Types | Engine |
|------|--------------|--------|
| Context | C4 Context | uml-mcp |
| Containers | C4 Container | uml-mcp |
| Components | C4 Component, Component Diagram | uml-mcp |
| Runtime | Sequence Diagrams | mcp-mermaid or uml-mcp |
| Deployment | Cloud Architecture | diagrams-mcp |
| Data | ERD, Class Diagram | mcp-mermaid, uml-mcp |
| Crosscutting | Flowcharts (auth, error handling) | mcp-mermaid |

### Example Prompt Sequence
```
1. "Create a C4 context diagram for our platform"
2. "Create a C4 container diagram showing all microservices"
3. "Create a C4 component diagram for the Order Service"
4. "Create sequence diagrams for: login, place order, process payment"
5. "Create an AWS deployment diagram showing ECS, RDS, and networking"
6. "Create an ERD for the complete data model"
7. "Create a flowchart showing the error handling strategy"
```

## Output Format Strategy

### For GitHub/GitLab Documentation
- Use **PNG** saved to a `docs/diagrams/` folder
- Reference in markdown: `![Architecture](docs/diagrams/architecture.png)`
- Use `file` output type with mcp-mermaid or render_diagram with diagrams-mcp

### For Confluence/Wiki
- Use **PNG** for static pages
- Use **SVG** for interactive/zoomable diagrams
- Use **URLs** (mcp-mermaid `png_url`/`svg_url`) for always-up-to-date links

### For Presentations
- Use **SVG** for scalable slides
- Use **PNG** at high resolution for static slides
- Keep diagrams simple (max 10-12 nodes for readability at presentation scale)

### For Code Reviews / PRs
- Use **URLs** for quick sharing in PR comments
- Use **PNG** embedded in PR description for architecture changes
- Include diagram source in a `docs/` folder for version control

## Naming Conventions

Consistent naming helps organize diagram files:

```
docs/diagrams/
├── architecture/
│   ├── system-context.png
│   ├── container-view.png
│   └── aws-deployment.png
├── flows/
│   ├── auth-sequence.png
│   ├── checkout-sequence.png
│   └── order-state.png
├── data/
│   ├── order-erd.png
│   └── user-domain-class.png
└── processes/
    ├── ci-cd-pipeline.png
    └── incident-response.png
```

## When to Update Diagrams

- **Always update** when: Architecture changes, new services added, APIs modified
- **Consider updating** when: Significant refactoring, new integrations, deployment changes
- **Skip updating** when: Bug fixes, minor UI changes, internal refactoring within a service
