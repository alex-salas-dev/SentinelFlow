# SentinelFlow Backend Architecture

## Package Structure

The Spring Boot backend will initially follow a layered architecture.

Planned structure:

```text
com.sentinelflow
+-- controller
+-- service
+-- repository
+-- domain
+-- dto
+-- exception
+-- config
+-- mapper
```

## Responsibilities

### controller

Handles HTTP requests and responses.

Responsibilities:

- receive REST requests
- validate request DTOs
- return HTTP status codes
- delegate business logic to services

Controllers must not contain persistence logic.

### service

Contains application business logic.

Responsibilities:

- coordinate use cases
- apply business rules
- call repositories
- transform domain data when required

### repository

Provides persistence access.

Responsibilities:

- interact with PostgreSQL
- query stored events
- persist entities

Spring Data JPA will be used initially.

### domain

Contains domain entities and core business concepts.

Initial domain object:

Event

### dto

Contains API request and response models.

Initial examples:

CreateEventRequest

EventResponse

### mapper

Transforms between DTOs and domain entities.

Example:

CreateEventRequest -> Event

Event -> EventResponse

### exception

Contains application-specific exceptions and centralized error handling.

Future examples:

ValidationException

EventNotFoundException

### config

Contains Spring configuration.

Examples:

- database configuration
- Redis configuration
- CORS configuration
- security configuration

## Dependency Direction

Expected flow:

```text
Controller
    |
    v
Service
    |
    v
Repository
    |
    v
Database
```

DTO mapping:

```text
DTO
 |
 v
Mapper
 |
 v
Domain
```

## Rules

1. Controllers must not access repositories directly.
2. Repositories must not contain business logic.
3. DTOs must remain separate from persistence entities.
4. Business logic belongs in services or domain objects.
5. Infrastructure details must not leak into API contracts.
6. PostgreSQL entities must not be returned directly from REST controllers.
