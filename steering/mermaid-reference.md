# Mermaid Syntax Quick-Reference

Use `mcp-mermaid` (local, themed) or `diagrams-mcp` / `uml-mcp` (remote) to render Mermaid diagrams.

## Flowchart

```mermaid
flowchart TD
    A[Start] --> B{Decision}
    B -->|Yes| C[Action 1]
    B -->|No| D[Action 2]
    C --> E[End]
    D --> E
```

**Direction**: `TD` (top-down), `LR` (left-right), `BT` (bottom-top), `RL` (right-left)

**Node shapes**:
- `[text]` — Rectangle
- `(text)` — Rounded
- `{text}` — Diamond (decision)
- `([text])` — Stadium
- `[[text]]` — Subroutine
- `[(text)]` — Cylinder (database)
- `((text))` — Circle
- `>text]` — Asymmetric

**Edge types**:
- `-->` — Arrow
- `---` — Line
- `-.->` — Dotted arrow
- `==>` — Thick arrow
- `-->|label|` — Labeled arrow

## Sequence Diagram

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Server
    participant DB as Database
    
    C->>S: POST /api/login
    activate S
    S->>DB: Query user
    DB-->>S: User record
    alt Valid credentials
        S-->>C: 200 OK + JWT
    else Invalid
        S-->>C: 401 Unauthorized
    end
    deactivate S
```

**Arrow types**:
- `->>` — Solid with arrowhead
- `-->>` — Dotted with arrowhead
- `--)` — Solid with open arrow (async)
- `-->)` — Dotted with open arrow

**Blocks**: `alt/else/end`, `opt/end`, `loop/end`, `par/and/end`, `critical/option/end`

## Class Diagram

```mermaid
classDiagram
    class Animal {
        +String name
        +int age
        +makeSound() void
    }
    class Dog {
        +fetch() void
    }
    class Cat {
        +purr() void
    }
    Animal <|-- Dog
    Animal <|-- Cat
```

**Relationships**:
- `<|--` — Inheritance
- `*--` — Composition
- `o--` — Aggregation
- `-->` — Association
- `..>` — Dependency
- `..|>` — Realization

**Cardinality**: `"1" --> "*"`, `"1" --> "0..1"`

## Entity Relationship Diagram

```mermaid
erDiagram
    USER ||--o{ ORDER : places
    ORDER ||--|{ LINE_ITEM : contains
    PRODUCT ||--o{ LINE_ITEM : "is in"
    
    USER {
        int id PK
        string email UK
        string name
        datetime created_at
    }
    ORDER {
        int id PK
        int user_id FK
        datetime order_date
        string status
    }
```

**Cardinality**:
- `||--||` — One to one
- `||--o{` — One to many
- `}o--o{` — Many to many
- `||--o|` — One to zero or one

## State Diagram

```mermaid
stateDiagram-v2
    [*] --> Idle
    Idle --> Processing : submit
    Processing --> Success : complete
    Processing --> Error : fail
    Error --> Idle : retry
    Success --> [*]
    
    state Processing {
        [*] --> Validating
        Validating --> Executing
        Executing --> [*]
    }
```

## Gantt Chart

```mermaid
gantt
    title Project Timeline
    dateFormat YYYY-MM-DD
    
    section Design
    Requirements     :done, req, 2024-01-01, 7d
    Architecture     :done, arch, after req, 5d
    
    section Development
    Backend API      :active, api, after arch, 14d
    Frontend UI      :ui, after arch, 14d
    
    section Testing
    Integration Tests :test, after api, 7d
    UAT              :uat, after test, 5d
```

## Mind Map

```mermaid
mindmap
    root((Project))
        Frontend
            React
            TypeScript
            Tailwind
        Backend
            Node.js
            Express
            PostgreSQL
        DevOps
            Docker
            AWS
            CI/CD
```

## Timeline

```mermaid
timeline
    title Product Roadmap
    section Q1
        January : Feature A : Feature B
        March : Feature C
    section Q2
        April : Feature D
        June : Launch
```

## Pie Chart

```mermaid
pie title Technology Stack
    "TypeScript" : 45
    "Python" : 30
    "Go" : 15
    "Other" : 10
```

## Quadrant Chart

```mermaid
quadrantChart
    title Feature Priority
    x-axis Low Effort --> High Effort
    y-axis Low Impact --> High Impact
    quadrant-1 Do First
    quadrant-2 Plan
    quadrant-3 Delegate
    quadrant-4 Eliminate
    Feature A: [0.8, 0.9]
    Feature B: [0.3, 0.7]
    Feature C: [0.6, 0.3]
```

## Styling with mcp-mermaid

When using `mcp-mermaid`, you can customize appearance:

**Themes**: `default`, `dark`, `forest`, `neutral`

**Background colors**: Any CSS color value (e.g., `#ffffff`, `transparent`, `#1a1a2e`)

**Output types**:
- `base64` — Returns base64-encoded PNG
- `svg` — Returns SVG markup
- `file` — Saves PNG to disk (specify path)
- `svg_url` — Returns public mermaid.ink SVG URL
- `png_url` — Returns public mermaid.ink PNG URL

## Tips for Better Diagrams

1. **Keep it readable**: Limit nodes to 15-20 per diagram. Split complex diagrams.
2. **Use meaningful IDs**: `auth_service` not `A1`
3. **Add labels to edges**: Always label decision branches and important connections
4. **Choose direction wisely**: `LR` for processes, `TD` for hierarchies
5. **Use subgraphs**: Group related nodes for clarity
6. **Consistent naming**: Use the same style (camelCase, snake_case) throughout
