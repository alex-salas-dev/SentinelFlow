# SentinelFlow Architecture

## 1. Overview

SentinelFlow is a monitoring and observability platform designed to receive, store, process and visualize technical events.

Initial architecture:

- React: frontend and dashboards
- Spring Boot: core API and business logic
- Python: analytics and event processing
- PostgreSQL: persistent storage
- Redis: cache and temporary high-speed data
- Docker Compose: local development environment

## 2. Components

### Spring Boot

Responsibilities:

- Receive monitoring events
- Validate incoming payloads
- Expose REST APIs
- Persist events in PostgreSQL
- Query event information
- Coordinate application business logic

Initial API example:

POST /api/v1/events

Future examples:

GET /api/v1/events
GET /api/v1/events/{id}
GET /api/v1/health

### PostgreSQL

Responsibilities:

- Persistent event storage
- Application metadata
- Event history
- Queryable observability data

PostgreSQL is the system of record.

### Redis

Responsibilities:

- Temporary cache
- Fast counters
- Rate-limiting support
- Temporary event state
- Future asynchronous processing support

Redis must not be the primary source of persistent event data.

### React

Responsibilities:

- Monitoring dashboard
- Event visualization
- Filters and search
- Metrics visualization
- Alert visualization

React will communicate with Spring Boot using REST APIs.

### Python

Responsibilities:

- Event analytics
- Statistical processing
- Anomaly detection
- Future machine learning capabilities

Python will remain independent from the core transactional API.

## 3. Initial Data Flow

Client / Agent
      |
      | HTTP / JSON
      v
Spring Boot API
      |
      +------> PostgreSQL
      |
      +------> Redis
      |
      v
Python Analytics
      |
      v
Processed insights
      |
      v
Spring Boot API
      |
      v
React Dashboard

## 4. Architectural Principles

- API-first design
- Clear separation of responsibilities
- Stateless backend where possible
- PostgreSQL as persistent source of truth
- Redis only for temporary or performance-oriented data
- Independent analytics component
- Containerized local infrastructure
- Configuration through environment variables
- No secrets committed to Git

## 5. Sprint 1 Scope

Sprint 1 will focus only on:

1. Spring Boot application
2. PostgreSQL
3. Event entity
4. Event persistence
5. POST /api/v1/events
6. Basic validation
7. Basic automated tests

React, Redis analytics logic and Python processing are outside Sprint 1 implementation scope.

## 6. Event Flow

### Event ingestion

External systems or monitoring agents send events to SentinelFlow using HTTP.

Example request:

POST /api/v1/events

Content-Type: application/json

Example payload:

{
  "source": "payment-service",
  "type": "ERROR",
  "severity": "HIGH",
  "message": "Database connection timeout",
  "timestamp": "2026-09-30T10:30:00Z"
}

### Processing flow

1. Client sends an event to Spring Boot.
2. Spring Boot validates the incoming payload.
3. Invalid events return a 400 response.
4. Valid events are converted into the internal domain model.
5. The event is persisted in PostgreSQL.
6. The API returns the created event identifier.
7. Redis may later store counters or temporary derived data.
8. Python analytics may consume event data in future Sprints.
9. React retrieves event information through the Spring Boot API.

## 7. Communication Contracts

### External clients -> Spring Boot

Protocol:

HTTP / HTTPS

Data format:

JSON

Initial endpoint:

POST /api/v1/events

### React -> Spring Boot

Protocol:

HTTP / HTTPS

Data format:

JSON

The frontend must never access PostgreSQL or Redis directly.

### Spring Boot -> PostgreSQL

Communication:

JPA / Hibernate

Purpose:

Persistent application data.

### Spring Boot -> Redis

Communication:

Redis client

Purpose:

Caching, counters and temporary state.

Redis is not part of the critical persistence path during Sprint 1.

### Spring Boot <-> Python

The exact communication mechanism will be introduced in a future Sprint.

Candidate approaches:

- REST API
- asynchronous messaging
- event queue

The implementation will not be coupled during Sprint 1.

## 8. Sprint 1 Event Contract

Initial event fields:

- id
- source
- type
- severity
- message
- occurredAt
- receivedAt

The client provides:

- source
- type
- severity
- message
- occurredAt

SentinelFlow generates:

- id
- receivedAt

Initial severity values:

- INFO
- LOW
- MEDIUM
- HIGH
- CRITICAL

## 9. Initial Request Lifecycle

Client
  |
  | POST /api/v1/events
  v
EventController
  |
  v
EventService
  |
  v
EventRepository
  |
  v
PostgreSQL
  |
  v
Event persisted
  |
  v
HTTP 201 Created

Error flow:

Invalid request
  |
  v
Validation
  |
  v
HTTP 400 Bad Request
