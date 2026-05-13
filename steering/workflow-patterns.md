# Diagramming Workflow Patterns

Patterns for integrating diagrams into your development workflow effectively.

## Workflow 1: RFP / Proposal Quality (Draw.io First)

Best for: Client-facing documents, RFPs, proposals, executive presentations

### Steps
1. **Understand requirements** — What architecture/process needs to be shown
2. **Search shapes** — Use `search_shapes` to find correct icons for all services
3. **Generate draw.io XML** — Build professional XML with proper containers, styling, and layout
4. **Open in draw.io** — Use `open_drawio_xml` to render
5. **Refine** — User can manually adjust in draw.io editor
6. **Export** — Save as PNG/SVG/PDF for the document

### Example Prompt Sequence
```
1. "Search for AWS shapes: EC2, RDS, Lambda, S3, CloudFront, API Gateway, VPC"
2. "Create a professional AWS architecture diagram showing our 3-tier application with CloudFront, ALB, ECS in private subnet, Aurora PostgreSQL, and ElastiCache. Use proper VPC/subnet containers."
3. "Now create a sequence diagram showing the authentication flow for the same system"
```

### Key Principles
- Always use `search_shapes` before generating XML
- Use proper container hierarchy (Cloud → Region → VPC → Subnet → Service)
- Apply consistent color scheme (see drawio-xml-guide.md)
- Use orthogonal edge routing for clean connections
- Add titles and legends for complex diagrams

---

## Workflow 2: AWS Architecture (drawio-aws)

Best for: AWS-specific architecture diagrams with official icons

### Steps
1. **Describe architecture** — Clearly state services, connections, and groupings
2. **Let drawio-aws generate** — It produces .drawio files with official AWS icons
3. **Open and refine** — Edit in draw.io desktop if needed
4. **Export** — PNG/SVG/PDF for documentation

### Example Prompts
```
"Create an AWS serverless architecture with CloudFront, API Gateway, Lambda (container-based from ECR), Aurora PostgreSQL, OpenSearch, S3, and Bedrock. Include Cognito for auth, IAM roles, and CodePipeline for deployment. Show VPC and private subnet structure."

"Create a multi-AZ AWS deployment with ALB, ECS Fargate across 2 AZs, Aurora with read replicas, ElastiCache cluster, and S3 for static assets."

"Create an AWS event-driven architecture with EventBridge, multiple Lambda functions, SQS queues, DynamoDB streams, and SNS for notifications."
```

---

## Workflow 3: Architecture-First (Design → Diagram → Code)

Best for: New projects, major features, system redesigns

### Steps
1. **Define requirements** — Understand what the system needs to do
2. **Create C4 Context diagram** — System boundaries and external actors (drawio XML)
3. **Create C4 Container diagram** — Major containers/services (drawio XML)
4. **Create Cloud Architecture diagram** — Map to infrastructure (drawio-aws)
5. **Create Sequence diagrams** — Detail key interactions (drawio `open_drawio_mermaid`)
6. **Create ERD** — Define data model (drawio XML or mcp-mermaid)
7. **Implement** — Use diagrams as implementation guide
8. **Update diagrams** — Reflect implementation changes

### Example Prompt Sequence
```
1. "Create a C4 context diagram for an e-commerce platform with users, payment provider, and shipping service"
2. "Now create a C4 container diagram showing the internal services"
3. "Create an AWS architecture diagram for deploying this on ECS with RDS and ElastiCache"
4. "Create a sequence diagram for the checkout flow"
5. "Create an ERD for the order management data model"
```

---

## Workflow 4: Code-First (Code → Analyze → Diagram)

Best for: Documenting existing systems, onboarding, code reviews

### Steps
1. **Analyze codebase** — Understand existing structure
2. **Create Component diagram** — Show discovered components (drawio XML)
3. **Create Class diagram** — Document key domain models (drawio XML with UML shapes)
4. **Create Sequence diagrams** — Document critical flows (drawio `open_drawio_mermaid`)
5. **Create Deployment diagram** — Document current infrastructure (drawio-aws)

---

## Workflow 5: Quick Developer Documentation

Best for: Internal docs, README diagrams, PR descriptions

### Steps
1. **Draft with Mermaid** — Quick syntax, fast iteration
2. **Render** — Use `mcp-mermaid` for themed output or `diagrams-mcp` for cloud diagrams
3. **Embed** — PNG in markdown, URL in PR comments

### Example Prompts
```
"Create a quick flowchart showing the CI/CD pipeline"
"Generate a Mermaid sequence diagram for the login flow"
"Create a quick AWS architecture diagram with diagrams-mcp showing our ECS setup"
```

---

## Workflow 6: Draft-to-Polish Pipeline

Best for: Starting quick, then upgrading to professional quality

### Steps
1. **Quick draft** — Use Mermaid or diagrams-mcp for rapid iteration
2. **Review** — Confirm the content and structure is correct
3. **Convert to draw.io** — Use `open_drawio_mermaid` to convert Mermaid to editable draw.io
4. **Polish** — Refine in draw.io editor (adjust layout, add icons, apply styling)
5. **Export** — Final PNG/SVG/PDF

This workflow is ideal when you need to iterate on content quickly but deliver polished output.

---

## Workflow 7: Documentation Sprint (Batch)

Best for: Creating comprehensive docs for a project or feature

### Recommended Diagram Set for a Service

| Diagram | Engine | Purpose |
|---------|--------|---------|
| System Context (C4) | drawio | Where this service fits in the ecosystem |
| Container View (C4) | drawio | Internal structure |
| Cloud Architecture | drawio-aws | Infrastructure and deployment |
| Key Sequence Diagrams (2-3) | drawio (mermaid) | Critical user flows |
| ERD | drawio or mcp-mermaid | Data model |
| State Diagram | mcp-mermaid | Key state machines |

### For Batch Generation (Developer Docs)
Use `uml-mcp` with `generate_uml_batch` for multiple diagrams at once:
```
"Generate a batch of diagrams for the Order Service:
1. A sequence diagram for order creation
2. A sequence diagram for order cancellation  
3. A state diagram for order lifecycle
4. An ERD for order-related tables"
```

---

## Output Format Strategy

### For RFPs / Proposals / Client Documents
- Generate with `drawio` or `drawio-aws`
- Export to **PDF** (print-quality) or **PNG** (high-res)
- Keep .drawio source file for future edits

### For GitHub/GitLab Documentation
- Use **PNG** saved to a `docs/diagrams/` folder
- Reference in markdown: `![Architecture](docs/diagrams/architecture.png)`
- Keep .drawio source files in same directory for editability

### For Confluence/Wiki
- Use **PNG** for static pages
- Use **SVG** for interactive/zoomable diagrams
- Keep .drawio files attached for team editing

### For Presentations
- Use **SVG** for scalable slides
- Keep diagrams simple (max 10-12 nodes for readability)
- Use draw.io's export with transparent background

### For Code Reviews / PRs
- Use **URLs** (mcp-mermaid `png_url`/`svg_url`) for quick sharing
- Use **PNG** embedded in PR description for architecture changes

---

## Naming Conventions

```
docs/diagrams/
├── architecture/
│   ├── system-context.drawio          # Editable source
│   ├── system-context.png             # Exported for docs
│   ├── aws-deployment.drawio
│   ├── aws-deployment.png
│   └── container-view.drawio
├── flows/
│   ├── auth-sequence.drawio
│   ├── checkout-sequence.drawio
│   └── order-state.drawio
├── data/
│   ├── order-erd.drawio
│   └── user-domain-class.drawio
└── processes/
    ├── ci-cd-pipeline.drawio
    └── incident-response.drawio
```

---

## When to Update Diagrams

- **Always update** when: Architecture changes, new services added, APIs modified
- **Consider updating** when: Significant refactoring, new integrations, deployment changes
- **Skip updating** when: Bug fixes, minor UI changes, internal refactoring within a service
