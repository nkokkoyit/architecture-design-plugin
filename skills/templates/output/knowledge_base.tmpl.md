# Knowledge Base

> **Project:** [TODO: Project name]
> **Date:** [TODO]
> **Sources analyzed:** [TODO: list source files]

## 1. UI Flow Analysis

### Step Overview

| Step | Name | Description | Key Fields |
|:---:|---|---|---|
| 1 | [TODO] | [TODO] | [TODO: field1, field2] |
| 2 | [TODO] | [TODO] | [TODO] |

### Data Contract Per Step

| Step | Sends (to server) | Receives (from server) | Dependencies |
|:---:|---|---|---|
| 1 | [TODO: fields] | [TODO: fields] | None |
| 2 | [TODO] | [TODO] | Step 1 data |

## 2. API Schema Extraction

### Endpoint Catalog

| Service | Path | Method | Purpose |
|---|---|---|---|
| [TODO] | [TODO: /api/v1/...] | GET/POST/PATCH | [TODO] |

### Per-Endpoint Schema

#### [TODO: Endpoint Name]
- **Path:** `[TODO]`
- **Method:** `[TODO]`
- **Request Body:**
```json
{
  "[TODO: field]": "[TODO: type]"
}
```
- **Response Body:**
```json
{
  "[TODO: field]": "[TODO: type]"
}
```

## 3. Entity Model

| Entity | Fields | Source | Relationships |
|---|---|---|---|
| [TODO] | [TODO: field1 (type), field2 (type)] | [TODO: Swagger/UI] | [TODO: 1:N with X] |

## 4. Business Rules

| # | Rule | Source | Impact |
|:---:|---|---|---|
| BR-001 | [TODO] | [TODO: PRD §X] | [TODO: affects Step Y] |

## 5. Impact Assessment

### Affected Services
| Service | Impact Type | Description |
|---|---|---|
| [TODO] | New integration / Modified | [TODO] |

### Data Lineage
[TODO: Describe data flow from source to destination]
