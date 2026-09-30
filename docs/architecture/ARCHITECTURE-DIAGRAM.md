# SentinelFlow - Architecture Diagram

## High-Level Architecture

```mermaid
flowchart LR

    A[External Agents / Clients] -->|HTTP JSON| B[Spring Boot API]

    B --> C[(PostgreSQL)]
    B --> D[(Redis)]
    B -->|Future integration| E[Python Analytics]

    E -->|Insights / Results| B

    F[React Frontend] -->|REST API| B

    C -->|Persistent Events| B
    D -->|Cache / Counters| B
```

## Main Responsibilities

### External Clients

Send monitoring events to SentinelFlow.

### Spring Boot API

Acts as the central application layer.

Responsibilities:

- event ingestion
- validation
- business logic
- persistence orchestration
- REST API exposure

### PostgreSQL

Stores persistent application and event data.

### Redis

Stores temporary or performance-oriented data.

### Python Analytics

Processes event information for future analytics and anomaly detection.

### React

Provides dashboards and visualization for users.
