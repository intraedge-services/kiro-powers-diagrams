# PlantUML & Kroki Syntax Quick-Reference

Use `uml-mcp` for PlantUML rendering via Kroki, or `diagrams-mcp` for PlantUML via its built-in engine.

## Sequence Diagram

```plantuml
@startuml
title Authentication Flow

actor User
participant "API Gateway" as GW
participant "Auth Service" as Auth
database "User DB" as DB

User -> GW: POST /login (credentials)
activate GW

GW -> Auth: Validate(credentials)
activate Auth

Auth -> DB: SELECT user WHERE email = ?
DB --> Auth: User record

alt Valid credentials
    Auth --> GW: JWT token
    GW --> User: 200 OK + token
else Invalid
    Auth --> GW: Error
    GW --> User: 401 Unauthorized
end

deactivate Auth
deactivate GW
@enduml
```

**Arrow types**:
- `->` — Synchronous
- `-->` — Dotted (return)
- `->>` — Async
- `->x` — Lost message
- `->o` — Endpoint

## Class Diagram

```plantuml
@startuml
title Domain Model

abstract class Entity {
    - id: UUID
    - createdAt: DateTime
    - updatedAt: DateTime
    + getId(): UUID
}

class User extends Entity {
    - email: String
    - passwordHash: String
    - role: Role
    + authenticate(password): Boolean
    + hasPermission(action): Boolean
}

class Order extends Entity {
    - status: OrderStatus
    - items: List<OrderItem>
    + calculateTotal(): Money
    + cancel(): void
}

class OrderItem {
    - quantity: int
    - unitPrice: Money
    + subtotal(): Money
}

enum OrderStatus {
    PENDING
    CONFIRMED
    SHIPPED
    DELIVERED
    CANCELLED
}

enum Role {
    ADMIN
    USER
    GUEST
}

User "1" --> "*" Order : places
Order "1" *-- "*" OrderItem : contains
Order --> OrderStatus
User --> Role
@enduml
```

**Visibility**:
- `+` Public
- `-` Private
- `#` Protected
- `~` Package

**Relationships**:
- `--|>` — Inheritance
- `--*` — Composition
- `--o` — Aggregation
- `-->` — Association
- `..>` — Dependency
- `..|>` — Implementation

## Activity Diagram

```plantuml
@startuml
title Order Processing

start

:Receive Order;

if (Payment valid?) then (yes)
    :Process Payment;
    
    fork
        :Update Inventory;
    fork again
        :Send Confirmation Email;
    end fork
    
    :Ship Order;
    
    if (Domestic?) then (yes)
        :Standard Shipping;
    else (no)
        :International Shipping;
    endif
    
    :Mark Delivered;
else (no)
    :Reject Order;
    :Notify Customer;
endif

stop
@enduml
```

## Component Diagram

```plantuml
@startuml
title System Components

package "Frontend" {
    [Web App] as webapp
    [Mobile App] as mobile
}

package "API Layer" {
    [API Gateway] as gateway
    [Auth Service] as auth
}

package "Business Logic" {
    [Order Service] as orders
    [User Service] as users
    [Notification Service] as notify
}

package "Data Layer" {
    database "PostgreSQL" as db
    database "Redis Cache" as cache
    queue "Message Queue" as mq
}

webapp --> gateway
mobile --> gateway
gateway --> auth
gateway --> orders
gateway --> users
orders --> db
orders --> mq
users --> db
users --> cache
mq --> notify
@enduml
```

## State Diagram

```plantuml
@startuml
title Order State Machine

[*] --> Draft

Draft --> Submitted : submit()
Submitted --> Processing : accept()
Submitted --> Cancelled : cancel()

Processing --> Shipped : ship()
Processing --> Cancelled : cancel()

Shipped --> Delivered : confirm_delivery()
Shipped --> Returned : return_request()

Delivered --> [*]
Returned --> Refunded : process_refund()
Refunded --> [*]
Cancelled --> [*]

state Processing {
    [*] --> Validating
    Validating --> Picking : validated
    Picking --> Packing : picked
    Packing --> [*] : packed
}
@enduml
```

## Use Case Diagram

```plantuml
@startuml
title E-Commerce Use Cases

left to right direction

actor Customer
actor Admin
actor "Payment Gateway" as PG

rectangle "E-Commerce System" {
    usecase "Browse Products" as UC1
    usecase "Add to Cart" as UC2
    usecase "Checkout" as UC3
    usecase "Process Payment" as UC4
    usecase "Manage Products" as UC5
    usecase "View Reports" as UC6
    usecase "Track Order" as UC7
}

Customer --> UC1
Customer --> UC2
Customer --> UC3
Customer --> UC7
UC3 --> UC4 : <<include>>
UC4 --> PG
Admin --> UC5
Admin --> UC6
@enduml
```

## Deployment Diagram

```plantuml
@startuml
title Production Deployment

node "AWS Cloud" {
    node "VPC" {
        node "Public Subnet" {
            [ALB] as lb
        }
        
        node "Private Subnet - App" {
            node "ECS Cluster" {
                [API Service] as api
                [Worker Service] as worker
            }
        }
        
        node "Private Subnet - Data" {
            database "RDS PostgreSQL" as rds
            database "ElastiCache Redis" as redis
            queue "SQS Queue" as sqs
        }
    }
    
    storage "S3 Bucket" as s3
    [CloudFront CDN] as cdn
}

cdn --> lb
lb --> api
api --> rds
api --> redis
api --> sqs
sqs --> worker
worker --> s3
@enduml
```

## C4 Model (via Kroki/PlantUML)

Use `uml-mcp` with diagram type `c4plantuml`:

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

title Container Diagram

Person(user, "Customer", "A user of the system")

System_Boundary(system, "E-Commerce Platform") {
    Container(webapp, "Web Application", "React", "Provides UI")
    Container(api, "API Service", "Node.js", "Handles business logic")
    ContainerDb(db, "Database", "PostgreSQL", "Stores data")
    Container(cache, "Cache", "Redis", "Session and data cache")
    ContainerQueue(queue, "Message Queue", "SQS", "Async processing")
}

System_Ext(payment, "Payment Provider", "Processes payments")
System_Ext(email, "Email Service", "Sends notifications")

Rel(user, webapp, "Uses", "HTTPS")
Rel(webapp, api, "Calls", "REST/JSON")
Rel(api, db, "Reads/Writes", "SQL")
Rel(api, cache, "Caches", "Redis protocol")
Rel(api, queue, "Publishes", "SQS")
Rel(api, payment, "Processes payments", "HTTPS")
Rel(queue, email, "Triggers", "HTTPS")
@enduml
```

## Using uml-mcp Tools

### generate_uml
```json
{
  "code": "@startuml\n...\n@enduml",
  "diagram_type": "plantuml",
  "output_format": "svg"
}
```

**Diagram types**: `plantuml`, `mermaid`, `d2`, `graphviz`, `erd`, `bpmn`, `c4plantuml`, `blockdiag`, `tikz`, `nomnoml`, `pikchr`, `structurizr`, `svgbob`, `wavedrom`, `wireviz`

### validate_uml
```json
{
  "code": "@startuml\n...\n@enduml",
  "diagram_type": "plantuml"
}
```

Always validate before rendering complex diagrams to catch syntax errors early.

### generate_uml_batch
```json
{
  "diagrams": [
    {"code": "...", "diagram_type": "plantuml", "output_format": "svg"},
    {"code": "...", "diagram_type": "mermaid", "output_format": "png"}
  ]
}
```

Use batch generation when creating multiple related diagrams (e.g., all C4 levels).

## Tips for Better PlantUML Diagrams

1. **Always use `@startuml` / `@enduml`** wrappers
2. **Add titles**: `title My Diagram` at the top
3. **Use aliases**: `participant "Long Name" as LN` for readability
4. **Group with packages**: Organize related elements visually
5. **Use notes**: `note right of X : explanation` for context
6. **Validate first**: Call `validate_uml` before `generate_uml` for complex diagrams
7. **Choose output format**: SVG for web, PNG for docs, PDF for print
