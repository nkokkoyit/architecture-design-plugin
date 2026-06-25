# C4 Model Quick Reference

Hướng dẫn sử dụng C4 Model trong thiết kế kiến trúc.

## 4 Levels

| Level | Name | Shows | Audience |
|:---:|---|---|---|
| 1 | System Context | Hệ thống + actors + external systems | Everyone |
| 2 | Container | Deployable units (apps, DBs, queues) | Tech team |
| 3 | Component | Internal modules within 1 container | Dev team |
| 4 | Code | Classes, interfaces, relationships | Developers |

## Mermaid Templates

### Level 1 — System Context
```mermaid
graph TB
    User["👤 User"] --> System["System"]
    System --> External["External System"]
    style System fill:#1168bd,color:#fff
```

### Level 2 — Container
```mermaid
graph TB
    subgraph System["System Boundary"]
        FE["Frontend SPA"]
        API["Backend API"]
        DB[("Database")]
    end
    FE --> API --> DB
```

### Level 3 — Component
```mermaid
graph TB
    subgraph Container["Backend API"]
        Controller["Controllers"]
        Service["Business Logic"]
        Repo["Repository"]
    end
    Controller --> Service --> Repo
```

### Level 4 — Code (Class Diagram)
```mermaid
classDiagram
    class ServiceClass {
        +method1()
        +method2()
    }
```

## Rules
1. Each level zooms into ONE element from the level above
2. Include description tables alongside diagrams
3. Use consistent color coding (blue=new, gray=existing)
4. Mermaid syntax: quote labels with special characters
5. Max ~10-15 elements per diagram for readability
