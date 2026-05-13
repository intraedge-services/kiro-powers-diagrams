# Draw.io XML Generation Guide

This guide covers how to generate professional-quality draw.io XML diagrams using the official `drawio` MCP server. This is the PRIMARY engine for all client-facing, RFP, and presentation-quality diagrams.

## Core Workflow

1. **Search for shapes** — Use `search_shapes` to find correct style strings for icons (AWS, Azure, GCP, Cisco, K8s, UML, etc.)
2. **Generate XML** — Build proper mxGraphModel XML with correct styles, positioning, and connections
3. **Open in draw.io** — Use `open_drawio_xml` to render the diagram in the editor

## XML Structure

Every draw.io diagram follows this structure:

```xml
<mxGraphModel>
  <root>
    <mxCell id="0" />
    <mxCell id="1" parent="0" />
    <!-- All diagram elements go here with parent="1" -->
  </root>
</mxGraphModel>
```

- Cell `id="0"` is the root
- Cell `id="1"` is the default layer (parent of all visible elements)
- All vertices and edges reference `parent="1"` (or a container cell)

## Vertices (Shapes/Nodes)

```xml
<mxCell id="2" value="Web Server" style="shape=mxgraph.aws4.ec2;..." 
        vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="60" height="60" as="geometry" />
</mxCell>
```

Key attributes:
- `id` — Unique identifier (use incrementing numbers or descriptive IDs)
- `value` — Display label (supports HTML: `<b>bold</b>`, `<br>` for line breaks)
- `style` — Semicolon-separated style properties
- `vertex="1"` — Marks this as a shape (not an edge)
- `parent` — Parent cell ID (use "1" for top-level, or container ID for grouped elements)

## Edges (Connections)

```xml
<mxCell id="10" value="" style="edgeStyle=orthogonalEdgeStyle;rounded=1;..." 
        edge="1" source="2" target="3" parent="1">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

Key attributes:
- `edge="1"` — Marks this as a connection
- `source` — ID of the source vertex
- `target` — ID of the target vertex
- Labels on edges: set `value="label text"`

## Containers (Groups/Clusters)

Use containers for VPCs, subnets, logical groupings:

```xml
<!-- Container -->
<mxCell id="20" value="VPC" style="rounded=1;whiteSpace=wrap;html=1;arcSize=5;
        verticalAlign=top;fillColor=#E8F5E9;strokeColor=#43A047;fontStyle=1;
        fontSize=14;container=1;collapsible=0;" 
        vertex="1" parent="1">
  <mxGeometry x="50" y="50" width="500" height="300" as="geometry" />
</mxCell>

<!-- Child element inside container -->
<mxCell id="21" value="EC2" style="shape=mxgraph.aws4.ec2;..." 
        vertex="1" parent="20">
  <mxGeometry x="50" y="50" width="60" height="60" as="geometry" />
</mxCell>
```

Important container style properties:
- `container=1` — Enables child containment
- `collapsible=0` — Prevents collapse (recommended for architecture diagrams)
- `verticalAlign=top` — Places label at top of container
- `childLayout=...` — Optional auto-layout for children

## Common Style Properties

### Shape Styles
| Property | Values | Purpose |
|----------|--------|---------|
| `rounded` | 0, 1 | Rounded corners |
| `whiteSpace` | wrap | Enable text wrapping |
| `html` | 1 | Enable HTML in labels |
| `fillColor` | #hex | Background color |
| `strokeColor` | #hex | Border color |
| `fontColor` | #hex | Text color |
| `fontSize` | number | Font size in pt |
| `fontStyle` | 0,1,2,3 | 0=normal, 1=bold, 2=italic, 3=bold+italic |
| `shadow` | 0, 1 | Drop shadow |
| `opacity` | 0-100 | Transparency |
| `align` | left, center, right | Horizontal text alignment |
| `verticalAlign` | top, middle, bottom | Vertical text alignment |

### Edge Styles
| Property | Values | Purpose |
|----------|--------|---------|
| `edgeStyle` | orthogonalEdgeStyle, elbowEdgeStyle, entityRelationEdgeStyle | Routing algorithm |
| `curved` | 0, 1 | Curved edges |
| `rounded` | 0, 1 | Rounded corners on orthogonal edges |
| `endArrow` | classic, block, open, oval, diamond, none | Arrow head style |
| `startArrow` | (same as endArrow) | Arrow tail style |
| `strokeWidth` | number | Line thickness |
| `dashed` | 0, 1 | Dashed line |
| `dashPattern` | "8 8", "4 4" | Custom dash pattern |
| `exitX`, `exitY` | 0-1 | Connection point on source (0=left/top, 1=right/bottom) |
| `entryX`, `entryY` | 0-1 | Connection point on target |

### Icon/Shape References
Use `search_shapes` to find these. Common patterns:
- AWS: `shape=mxgraph.aws4.ec2`, `shape=mxgraph.aws4.lambda`, `shape=mxgraph.aws4.s3`
- Azure: `shape=mxgraph.azure.virtual_machine`, `shape=mxgraph.azure.sql_database`
- GCP: `shape=mxgraph.gcp2.compute_engine`, `shape=mxgraph.gcp2.cloud_storage`
- Cisco: `shape=mxgraph.cisco19.*`
- Kubernetes: `shape=mxgraph.kubernetes.*`
- Network: `shape=mxgraph.network.*`

## Professional Layout Guidelines

### Spacing and Alignment
- **Minimum spacing**: 40px between elements, 80px between groups
- **Consistent sizing**: Use uniform sizes for same-type elements (e.g., all services = 60x60)
- **Grid alignment**: Align elements to a 20px grid for clean appearance
- **Container padding**: 40px padding inside containers

### Color Schemes for Professional Diagrams

**AWS-style (light background):**
- Background: `#FFFFFF`
- Containers: `fillColor=#E8F5E9;strokeColor=#43A047` (green for VPC)
- Subnets: `fillColor=#E3F2FD;strokeColor=#1976D2` (blue for public), `fillColor=#FFF3E0;strokeColor=#F57C00` (orange for private)
- Connections: `strokeColor=#333333`

**Corporate/RFP-style:**
- Primary: `fillColor=#1A237E;fontColor=#FFFFFF` (dark blue)
- Secondary: `fillColor=#0277BD;fontColor=#FFFFFF` (medium blue)
- Accent: `fillColor=#00838F;fontColor=#FFFFFF` (teal)
- Background containers: `fillColor=#F5F5F5;strokeColor=#BDBDBD`
- Connections: `strokeColor=#424242;strokeWidth=2`

**Neutral/Clean:**
- Shapes: `fillColor=#FFFFFF;strokeColor=#333333;strokeWidth=2`
- Containers: `fillColor=#F8F9FA;strokeColor=#DEE2E6`
- Labels: `fontColor=#212529;fontSize=12`
- Connections: `strokeColor=#6C757D`

### Layout Patterns

**Left-to-Right (Data Flow):**
- Users/clients on the left
- Processing in the middle
- Data stores on the right
- Use orthogonal edges with `edgeStyle=orthogonalEdgeStyle`

**Top-to-Bottom (Hierarchy):**
- Entry points at top (CDN, Load Balancer)
- Application tier in middle
- Data tier at bottom

**Layered Architecture:**
- Presentation layer (top)
- Business logic layer (middle)
- Data access layer (bottom)
- Each layer in its own container

## Example: Professional AWS Architecture

```xml
<mxGraphModel>
  <root>
    <mxCell id="0" />
    <mxCell id="1" parent="0" />
    
    <!-- AWS Cloud Container -->
    <mxCell id="2" value="AWS Cloud" style="rounded=1;whiteSpace=wrap;html=1;arcSize=3;verticalAlign=top;fillColor=#FF9900;fillOpacity=10;strokeColor=#FF9900;fontStyle=1;fontSize=16;fontColor=#FF9900;container=1;collapsible=0;" vertex="1" parent="1">
      <mxGeometry x="100" y="50" width="700" height="500" as="geometry" />
    </mxCell>
    
    <!-- VPC Container -->
    <mxCell id="3" value="VPC (10.0.0.0/16)" style="rounded=1;whiteSpace=wrap;html=1;arcSize=3;verticalAlign=top;fillColor=#E8F5E9;strokeColor=#43A047;fontStyle=1;fontSize=13;container=1;collapsible=0;" vertex="1" parent="2">
      <mxGeometry x="40" y="60" width="620" height="400" as="geometry" />
    </mxCell>
    
    <!-- Public Subnet -->
    <mxCell id="4" value="Public Subnet" style="rounded=1;whiteSpace=wrap;html=1;arcSize=3;verticalAlign=top;fillColor=#E3F2FD;strokeColor=#1976D2;fontSize=11;container=1;collapsible=0;" vertex="1" parent="3">
      <mxGeometry x="30" y="50" width="250" height="150" as="geometry" />
    </mxCell>
    
    <!-- ALB in Public Subnet -->
    <mxCell id="5" value="ALB" style="shape=mxgraph.aws4.application_load_balancer;verticalLabelPosition=bottom;verticalAlign=top;fontSize=10;" vertex="1" parent="4">
      <mxGeometry x="95" y="45" width="60" height="60" as="geometry" />
    </mxCell>
    
    <!-- Private Subnet -->
    <mxCell id="6" value="Private Subnet" style="rounded=1;whiteSpace=wrap;html=1;arcSize=3;verticalAlign=top;fillColor=#FFF3E0;strokeColor=#F57C00;fontSize=11;container=1;collapsible=0;" vertex="1" parent="3">
      <mxGeometry x="30" y="230" width="560" height="140" as="geometry" />
    </mxCell>
    
    <!-- ECS Service -->
    <mxCell id="7" value="ECS Service" style="shape=mxgraph.aws4.ecs_service;verticalLabelPosition=bottom;verticalAlign=top;fontSize=10;" vertex="1" parent="6">
      <mxGeometry x="60" y="40" width="60" height="60" as="geometry" />
    </mxCell>
    
    <!-- RDS -->
    <mxCell id="8" value="Aurora PostgreSQL" style="shape=mxgraph.aws4.rds;verticalLabelPosition=bottom;verticalAlign=top;fontSize=10;" vertex="1" parent="6">
      <mxGeometry x="250" y="40" width="60" height="60" as="geometry" />
    </mxCell>
    
    <!-- ElastiCache -->
    <mxCell id="9" value="ElastiCache" style="shape=mxgraph.aws4.elasticache;verticalLabelPosition=bottom;verticalAlign=top;fontSize=10;" vertex="1" parent="6">
      <mxGeometry x="430" y="40" width="60" height="60" as="geometry" />
    </mxCell>
    
    <!-- Connections -->
    <mxCell id="10" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#333333;strokeWidth=2;" edge="1" source="5" target="7" parent="3">
      <mxGeometry relative="1" as="geometry" />
    </mxCell>
    <mxCell id="11" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#333333;strokeWidth=1;dashed=1;" edge="1" source="7" target="8" parent="6">
      <mxGeometry relative="1" as="geometry" />
    </mxCell>
    <mxCell id="12" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#333333;strokeWidth=1;dashed=1;" edge="1" source="7" target="9" parent="6">
      <mxGeometry relative="1" as="geometry" />
    </mxCell>
  </root>
</mxGraphModel>
```

## CSV Import Format

For org charts, hierarchies, and tabular data, use `open_drawio_csv`:

```csv
## label: %name%<br><i>%role%</i>
## style: shape=mxgraph.basic.rect;rounded=1;fillColor=#FFFFFF;strokeColor=#333333;fontSize=12;whiteSpace=wrap;html=1;
## parentstyle: edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#666666;
## identity: id
## parent: manager
## width: 160
## height: 60
## padding: 30
##
id,name,role,manager
1,Sarah Chen,CEO,
2,James Park,CTO,1
3,Maria Lopez,CFO,1
4,Alex Kim,VP Engineering,2
5,Priya Patel,VP Product,2
6,David Wu,Lead Architect,4
7,Lisa Zhang,Senior Engineer,4
```

## Mermaid to Draw.io Conversion

Use `open_drawio_mermaid` to convert Mermaid syntax into editable draw.io diagrams:

```
flowchart LR
    A[Client] --> B[Load Balancer]
    B --> C[Service A]
    B --> D[Service B]
    C --> E[(Database)]
    D --> E
```

This produces an editable draw.io diagram from Mermaid syntax — useful for quick drafts that need polish.

## Best Practices for RFP-Quality Output

1. **Always search shapes first** — Use `search_shapes("aws ec2")` to get exact style strings rather than guessing
2. **Use containers for grouping** — VPCs, subnets, availability zones, logical tiers
3. **Consistent icon sizing** — 60x60 for service icons, 48x48 for smaller elements
4. **Label placement** — Use `verticalLabelPosition=bottom;verticalAlign=top` for icons with labels below
5. **Professional colors** — Use the color schemes above, avoid garish colors
6. **Orthogonal routing** — Use `edgeStyle=orthogonalEdgeStyle;rounded=1` for clean connections
7. **Proper spacing** — Minimum 80px between elements, 40px padding in containers
8. **Title and legend** — Add a title cell and optional legend for complex diagrams
9. **Consistent font** — Use `fontSize=11` or `fontSize=12` throughout, `fontStyle=1` (bold) for titles only
10. **Shadow for depth** — Add `shadow=1` to primary elements for visual hierarchy
