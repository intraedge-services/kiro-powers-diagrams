# Cloud Architecture Diagram Guide

This guide covers creating professional cloud architecture diagrams. For RFP-quality output, use the draw.io servers. For quick developer visualizations, use diagrams-mcp.

## Engine Selection for Cloud Diagrams

| Scenario | Engine | Why |
|----------|--------|-----|
| RFP/proposal/client-facing | **drawio-aws** or **drawio** | Professional quality, editable, official icons |
| Quick visualization for team | **diagrams-mcp** | Fast Python-based generation with real icons |
| Multi-cloud comparison | **drawio** | Search shapes for each provider |
| Kubernetes topology | **drawio-aws** or **diagrams-mcp** | Both support K8s icons |

---

## Method 1: Draw.io AWS MCP (RFP-Quality)

The `drawio-aws` server generates native .drawio files with official AWS architecture icons. Simply describe your architecture and it produces a professional diagram.

### Best Practices with drawio-aws
- Describe your architecture clearly: services, connections, groupings
- Mention VPC structure, subnets, availability zones
- Specify data flow direction
- Include security boundaries (security groups, NACLs)

### Example Prompts for drawio-aws
```
"Create an AWS serverless architecture with CloudFront, API Gateway, Lambda, DynamoDB, and S3. Show the VPC with public and private subnets."

"Create a 3-tier AWS architecture: ALB in public subnet, ECS Fargate in private subnet, Aurora PostgreSQL in data subnet. Include NAT Gateway and Internet Gateway."

"Create an AWS microservices architecture with API Gateway, multiple Lambda functions, SQS queues, SNS topics, and DynamoDB tables. Show event-driven communication."
```

---

## Method 2: Draw.io XML with Shape Search (Maximum Control)

For maximum control over layout and styling, generate draw.io XML directly using the `drawio` server.

### Step 1: Search for Shapes
```
search_shapes("aws ec2")
search_shapes("aws rds")
search_shapes("aws lambda")
search_shapes("aws s3")
search_shapes("aws vpc")
search_shapes("aws alb")
search_shapes("azure virtual machine")
search_shapes("gcp compute engine")
search_shapes("kubernetes pod")
```

### Step 2: Build XML with Proper Structure

Use containers for AWS organizational boundaries:

```xml
<mxGraphModel>
  <root>
    <mxCell id="0" />
    <mxCell id="1" parent="0" />
    
    <!-- AWS Cloud boundary -->
    <mxCell id="aws" value="AWS Cloud" style="rounded=1;whiteSpace=wrap;html=1;
      arcSize=3;verticalAlign=top;fillColor=#FF9900;fillOpacity=8;
      strokeColor=#FF9900;fontStyle=1;fontSize=16;fontColor=#FF9900;
      container=1;collapsible=0;" vertex="1" parent="1">
      <mxGeometry x="50" y="30" width="800" height="550" as="geometry" />
    </mxCell>
    
    <!-- Region -->
    <mxCell id="region" value="us-east-1" style="rounded=1;whiteSpace=wrap;html=1;
      arcSize=3;verticalAlign=top;fillColor=none;strokeColor=#147EBA;
      strokeDasharray=8 4;fontStyle=2;fontSize=11;fontColor=#147EBA;
      container=1;collapsible=0;" vertex="1" parent="aws">
      <mxGeometry x="20" y="50" width="760" height="480" as="geometry" />
    </mxCell>
    
    <!-- VPC -->
    <mxCell id="vpc" value="VPC (10.0.0.0/16)" style="rounded=1;whiteSpace=wrap;html=1;
      arcSize=3;verticalAlign=top;fillColor=#E8F5E9;strokeColor=#43A047;
      fontStyle=1;fontSize=13;container=1;collapsible=0;" vertex="1" parent="region">
      <mxGeometry x="20" y="40" width="720" height="420" as="geometry" />
    </mxCell>
    
    <!-- Public Subnet AZ-1 -->
    <mxCell id="pub1" value="Public Subnet (AZ-1)&#xa;10.0.1.0/24" 
      style="rounded=1;whiteSpace=wrap;html=1;arcSize=3;verticalAlign=top;
      fillColor=#E3F2FD;strokeColor=#1976D2;fontSize=10;
      container=1;collapsible=0;" vertex="1" parent="vpc">
      <mxGeometry x="20" y="40" width="330" height="160" as="geometry" />
    </mxCell>
    
    <!-- Private Subnet AZ-1 -->
    <mxCell id="priv1" value="Private Subnet (AZ-1)&#xa;10.0.3.0/24" 
      style="rounded=1;whiteSpace=wrap;html=1;arcSize=3;verticalAlign=top;
      fillColor=#FFF3E0;strokeColor=#F57C00;fontSize=10;
      container=1;collapsible=0;" vertex="1" parent="vpc">
      <mxGeometry x="20" y="230" width="330" height="160" as="geometry" />
    </mxCell>
    
    <!-- Services go inside their respective subnets -->
  </root>
</mxGraphModel>
```

### AWS Container Color Conventions
| Element | Fill Color | Stroke Color | Notes |
|---------|-----------|--------------|-------|
| AWS Cloud | `#FF9900` opacity 8% | `#FF9900` | Orange border, nearly transparent fill |
| Region | none | `#147EBA` dashed | Blue dashed border |
| VPC | `#E8F5E9` | `#43A047` | Green |
| Public Subnet | `#E3F2FD` | `#1976D2` | Blue |
| Private Subnet | `#FFF3E0` | `#F57C00` | Orange |
| Availability Zone | `#F3E5F5` | `#7B1FA2` | Purple (optional) |
| Security Group | none | `#E53935` dashed | Red dashed border |

---

## Method 3: diagrams-mcp (Quick Python-Based)

Use `diagrams-mcp` for quick cloud architecture visualizations using the [mingrammer/diagrams](https://diagrams.mingrammer.com/) Python library.

### Discovery Workflow
1. `list_providers()` → aws, gcp, azure, k8s, onprem, ...
2. `list_services("aws")` → compute, database, network, storage, ...
3. `list_nodes("aws", "compute")` → EC2, Lambda, ECS, Fargate, ...
4. `search_nodes("postgres")` → finds PostgreSQL across all providers
5. `find_equivalent("EC2", "gcp")` → ComputeEngine

### Common Patterns

#### Three-Tier Architecture
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

#### Microservices Architecture
```python
from diagrams import Diagram, Cluster, Edge
from diagrams.aws.network import APIGateway
from diagrams.aws.compute import Lambda, ECS
from diagrams.aws.database import RDS, DynamoDB
from diagrams.aws.integration import SQS, SNS

with Diagram("Microservices", direction="TB"):
    gw = APIGateway("API Gateway")
    
    with Cluster("Services"):
        user_svc = ECS("User Service")
        order_svc = ECS("Order Service")
        notify = Lambda("Notifications")
    
    queue = SQS("Event Queue")
    topic = SNS("Events")
    
    gw >> [user_svc, order_svc]
    order_svc >> topic >> queue >> notify
```

#### Serverless Architecture
```python
from diagrams import Diagram, Cluster
from diagrams.aws.network import APIGateway, CloudFront
from diagrams.aws.compute import Lambda
from diagrams.aws.database import DynamoDB
from diagrams.aws.storage import S3
from diagrams.aws.security import Cognito

with Diagram("Serverless", direction="LR"):
    cf = CloudFront("CDN")
    auth = Cognito("Auth")
    gw = APIGateway("API")
    
    with Cluster("Functions"):
        fns = [Lambda("Create"), Lambda("Read"), Lambda("Update"), Lambda("Delete")]
    
    db = DynamoDB("Data")
    s3 = S3("Assets")
    
    cf >> gw >> fns >> db
    cf >> s3
    auth >> gw
```

#### Kubernetes Deployment
```python
from diagrams import Diagram, Cluster
from diagrams.k8s.network import Ingress, Service
from diagrams.k8s.compute import Deployment, Pod
from diagrams.k8s.storage import PersistentVolume

with Diagram("K8s Deployment", direction="LR"):
    ingress = Ingress("ingress")
    
    with Cluster("production"):
        svc = Service("api-service")
        with Cluster("Deployment"):
            pods = [Pod("pod-1"), Pod("pod-2"), Pod("pod-3")]
        pv = PersistentVolume("data")
    
    ingress >> svc >> pods
    pods[0] - pv
```

### Cross-Provider Equivalence

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

---

## Best Practices

1. **Use containers/clusters** to group related services (VPCs, subnets, namespaces)
2. **Direction matters**: `LR` for data flow, `TB` for hierarchies
3. **Limit complexity**: Max 20-25 nodes per diagram. Split into multiple views.
4. **Label edges** for protocols and data types (REST, gRPC, SQL, etc.)
5. **Use discovery tools** to find correct node/shape names
6. **Consistent naming**: Match your actual service names
7. **Multiple views**: Separate diagrams for network, compute, data, and security layers
8. **For RFPs**: Always use draw.io servers for the final version — they produce editable, professional output
