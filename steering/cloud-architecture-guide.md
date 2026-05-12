# Cloud Architecture Diagram Guide

Use `diagrams-mcp` to create professional cloud architecture diagrams with real provider icons from AWS, GCP, Azure, Kubernetes, and more.

## How It Works

The `diagrams-mcp` server uses the [mingrammer/diagrams](https://diagrams.mingrammer.com/) Python library. You write Python code that describes your architecture, and the server renders it as a PNG with real cloud provider icons.

## Supported Providers

| Provider | Import Path | Examples |
|----------|-------------|----------|
| AWS | `diagrams.aws.*` | EC2, Lambda, S3, RDS, ECS, ALB, CloudFront |
| GCP | `diagrams.gcp.*` | GCE, CloudFunctions, GCS, CloudSQL, GKE |
| Azure | `diagrams.azure.*` | VMs, Functions, BlobStorage, SQLDatabase, AKS |
| Kubernetes | `diagrams.k8s.*` | Pod, Service, Deployment, Ingress, ConfigMap |
| On-Premise | `diagrams.onprem.*` | Nginx, Docker, Jenkins, Kafka, PostgreSQL |
| Generic | `diagrams.generic.*` | Generic compute, storage, network icons |

## Discovery Workflow

Before creating a diagram, discover available nodes:

1. **List providers**: `list_providers()` → aws, gcp, azure, k8s, onprem, ...
2. **List services**: `list_services("aws")` → compute, database, network, storage, ...
3. **List nodes**: `list_nodes("aws", "compute")` → EC2, Lambda, ECS, Fargate, ...
4. **Search**: `search_nodes("postgres")` → finds PostgreSQL across all providers
5. **Find equivalent**: `find_equivalent("EC2", "gcp")` → ComputeEngine

## Basic Patterns

### Simple Three-Tier Architecture

```python
from diagrams import Diagram, Cluster
from diagrams.aws.network import ALB, CloudFront
from diagrams.aws.compute import ECS
from diagrams.aws.database import RDS, ElastiCache

with Diagram("Three-Tier Architecture", direction="LR"):
    cdn = CloudFront("CDN")
    lb = ALB("Load Balancer")
    
    with Cluster("Application Tier"):
        services = [ECS("Service 1"), ECS("Service 2"), ECS("Service 3")]
    
    with Cluster("Data Tier"):
        db = RDS("PostgreSQL")
        cache = ElastiCache("Redis")
    
    cdn >> lb >> services
    services >> db
    services >> cache
```

### Microservices Architecture

```python
from diagrams import Diagram, Cluster, Edge
from diagrams.aws.network import ALB, APIGateway
from diagrams.aws.compute import Lambda, ECS
from diagrams.aws.database import RDS, DynamoDB
from diagrams.aws.integration import SQS, SNS
from diagrams.aws.storage import S3

with Diagram("Microservices Architecture", direction="TB"):
    gw = APIGateway("API Gateway")
    
    with Cluster("Services"):
        with Cluster("User Service"):
            user_svc = ECS("User API")
            user_db = RDS("Users DB")
            user_svc >> user_db
        
        with Cluster("Order Service"):
            order_svc = ECS("Order API")
            order_db = DynamoDB("Orders")
            order_svc >> order_db
        
        with Cluster("Notification Service"):
            notify = Lambda("Notifier")
    
    queue = SQS("Event Queue")
    topic = SNS("Events")
    
    gw >> user_svc
    gw >> order_svc
    order_svc >> topic
    topic >> queue
    queue >> notify
```

### Kubernetes Deployment

```python
from diagrams import Diagram, Cluster
from diagrams.k8s.network import Ingress, Service
from diagrams.k8s.compute import Deployment, Pod, ReplicaSet
from diagrams.k8s.storage import PersistentVolume
from diagrams.k8s.podconfig import ConfigMap, Secret

with Diagram("K8s Deployment", direction="LR"):
    ingress = Ingress("ingress")
    
    with Cluster("Namespace: production"):
        svc = Service("api-service")
        
        with Cluster("Deployment"):
            pods = [Pod("pod-1"), Pod("pod-2"), Pod("pod-3")]
        
        config = ConfigMap("config")
        secret = Secret("db-credentials")
        pv = PersistentVolume("data-volume")
    
    ingress >> svc >> pods
    pods[0] - config
    pods[0] - secret
    pods[0] - pv
```

### Serverless Architecture

```python
from diagrams import Diagram, Cluster
from diagrams.aws.network import APIGateway, CloudFront
from diagrams.aws.compute import Lambda
from diagrams.aws.database import DynamoDB
from diagrams.aws.storage import S3
from diagrams.aws.integration import SQS, Eventbridge
from diagrams.aws.security import Cognito

with Diagram("Serverless Architecture", direction="LR"):
    cf = CloudFront("CDN")
    s3 = S3("Static Assets")
    auth = Cognito("Auth")
    
    with Cluster("API"):
        gw = APIGateway("REST API")
        
        with Cluster("Functions"):
            create = Lambda("Create")
            read = Lambda("Read")
            update = Lambda("Update")
            delete = Lambda("Delete")
    
    db = DynamoDB("Data Store")
    queue = SQS("Async Queue")
    events = Eventbridge("Event Bus")
    processor = Lambda("Processor")
    
    cf >> s3
    cf >> gw
    auth >> gw
    gw >> [create, read, update, delete]
    [create, read, update, delete] >> db
    create >> queue
    queue >> processor
    processor >> events
```

## Advanced Patterns

### Edge Labels and Styling

```python
from diagrams import Diagram, Edge
from diagrams.aws.compute import ECS
from diagrams.aws.database import RDS

with Diagram("Labeled Connections"):
    app = ECS("App")
    db = RDS("Database")
    
    app >> Edge(label="SQL/TLS", color="blue", style="bold") >> db
```

### Multi-Cloud / Hybrid

```python
from diagrams import Diagram, Cluster
from diagrams.aws.compute import ECS
from diagrams.gcp.compute import GKE
from diagrams.onprem.network import Nginx
from diagrams.onprem.compute import Server

with Diagram("Hybrid Architecture", direction="LR"):
    with Cluster("On-Premise"):
        lb = Nginx("Load Balancer")
        legacy = Server("Legacy System")
    
    with Cluster("AWS"):
        aws_app = ECS("Primary App")
    
    with Cluster("GCP"):
        gcp_app = GKE("DR App")
    
    lb >> aws_app
    lb >> gcp_app
    aws_app >> legacy
```

### Diagram Parameters

```python
with Diagram(
    "My Architecture",
    direction="LR",          # LR, RL, TB, BT
    filename="output_name",  # Output filename (without extension)
    outformat="png",         # png, jpg, svg, pdf, dot
    show=False,              # Don't auto-open
    graph_attr={
        "fontsize": "20",
        "bgcolor": "white"
    }
):
    ...
```

## Cross-Provider Equivalence

Use `find_equivalent()` to map services across clouds:

| AWS | GCP | Azure |
|-----|-----|-------|
| EC2 | Compute Engine | Virtual Machines |
| Lambda | Cloud Functions | Azure Functions |
| S3 | Cloud Storage | Blob Storage |
| RDS | Cloud SQL | SQL Database |
| ECS/EKS | GKE | AKS |
| SQS | Pub/Sub | Service Bus |
| DynamoDB | Firestore/Bigtable | Cosmos DB |
| CloudFront | Cloud CDN | Azure CDN |
| API Gateway | API Gateway | API Management |
| Cognito | Identity Platform | Azure AD B2C |

## Best Practices

1. **Use Clusters** to group related services (VPCs, subnets, namespaces)
2. **Direction matters**: `LR` for data flow, `TB` for hierarchies
3. **Limit complexity**: Max 20-25 nodes per diagram. Split into multiple views.
4. **Label edges** for protocols and data types (REST, gRPC, SQL, etc.)
5. **Use discovery tools** (`search_nodes`, `list_nodes`) to find correct node names
6. **Consistent naming**: Match your actual service names for documentation accuracy
7. **Multiple views**: Create separate diagrams for network, compute, data, and security layers
