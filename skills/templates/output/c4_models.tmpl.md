# C4 Architecture Models

> **Project:** [TODO]
> **Date:** [TODO]

## Level 1: System Context

```mermaid
graph TB
    User["👤 [TODO: User]"] --> System["[TODO: System Name]"]
    System --> Ext1["[TODO: External 1]"]
    System --> Ext2["[TODO: External 2]"]
    style System fill:#1168bd,color:#fff
```

| Element | Type | Description |
|---|---|---|
| [TODO] | Person / External System | [TODO] |

## Level 2: Container Diagram

```mermaid
graph TB
    subgraph Boundary["[TODO: System Boundary]"]
        FE["[TODO: Frontend]"]
        API["[TODO: Backend API]"]
        DB[("[TODO: Database]")]
    end
    FE --> API --> DB
```

| Container | Technology | Description |
|---|---|---|
| [TODO] | [TODO] | [TODO] |

## Level 3: Component Diagram

```mermaid
graph TB
    subgraph API["[TODO: Backend API]"]
        C1["[TODO: Controller Layer]"]
        C2["[TODO: Service Layer]"]
        C3["[TODO: Repository Layer]"]
    end
    C1 --> C2 --> C3
```

| Component | Responsibility | Technology |
|---|---|---|
| [TODO] | [TODO] | [TODO] |

## Level 4: Class Diagram

```mermaid
classDiagram
    class TodoService {
        +method1()
        +method2()
    }
```
