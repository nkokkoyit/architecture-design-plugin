# [TODO: Project Name] — Architecture Document

> **Version:** [TODO: v1.0]
> **Date:** [TODO]
> **Review Score:** [TODO: XX/100]
> **Status:** [TODO: Draft / Reviewed / Approved]

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [C4 Level 1: System Context](#2-c4-level-1-system-context)
3. [C4 Level 2: Container Diagram](#3-c4-level-2-container-diagram)
4. [C4 Level 3: Component Diagram](#4-c4-level-3-component-diagram)
5. [C4 Level 4: Class Diagram](#5-c4-level-4-class-diagram)
6. [State Machine & Business Flow](#6-state-machine--business-flow)
7. [API Design — BFF Endpoints](#7-api-design--bff-endpoints)
8. [Downstream Integration Mapping](#8-downstream-integration-mapping)
9. [Database Design](#9-database-design)
10. [Feature Designs](#10-feature-designs)
11. [Data Flow Matrix](#11-data-flow-matrix)
12. [Data Mapping](#12-data-mapping)
13. [Security & Compliance](#13-security--compliance)
14. [Resilience & Observability](#14-resilience--observability)
15. [Deployment Architecture](#15-deployment-architecture)
16. [Architecture Decision Records](#16-architecture-decision-records)

---

## 1. Executive Summary

[TODO: 3-5 paragraphs describing the project, architecture decisions, and key outcomes]

## 2. C4 Level 1: System Context

[TODO: Merge from c4_models.md — System Context diagram + description table]

## 3. C4 Level 2: Container Diagram

[TODO: Merge from c4_models.md — Container diagram + description table]

## 4. C4 Level 3: Component Diagram

[TODO: Merge from c4_models.md — Component diagram + description table]

## 5. C4 Level 4: Class Diagram

[TODO: Merge from c4_models.md — Class diagram]

## 6. State Machine & Business Flow

### 6.1 State Diagram
[TODO: Merge from state_machine.md]

### 6.2 Sequence Diagram (TO-BE)
[TODO: Merge from integration_architecture.md]

### 6.3 Sequence Diagram (AS-IS)
[TODO: Merge from integration_architecture.md]

## 7. API Design — BFF Endpoints

[TODO: Merge from api_design.md — Endpoint catalog + key endpoint details]

## 8. Downstream Integration Mapping

[TODO: Merge from api_design.md — Downstream mapping table]

## 9. Database Design

[TODO: Merge from database_design.md — ERD, key tables, DDL principles]

## 10. Feature Designs

[TODO: Merge from feature_designs/*.md or note "No additional features"]

## 11. Data Flow Matrix

| Step | User Action | BFF Tables | Downstream API | Data Created |
|:---:|---|---|---|---|
| [TODO] | [TODO] | [TODO] | [TODO] | [TODO] |

## 12. Data Mapping

[TODO: Merge from database_design.md — Data mapping table]

## 13. Security & Compliance

[TODO: Authentication, encryption, PII protection, rate limiting, defense-in-depth]

## 14. Resilience & Observability

[TODO: Circuit breaker, timeout, retry, monitoring, telemetry events]

## 15. Deployment Architecture

[TODO: Container orchestration, HPA, resource limits, health checks]

## 16. Architecture Decision Records

[TODO: Summary table of all ADRs with links]

| ADR | Title | Decision | Status |
|---|---|---|---|
| ADR-001 | [TODO] | [TODO] | Accepted |
