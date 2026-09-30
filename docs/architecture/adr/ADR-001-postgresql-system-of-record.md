# ADR-001: PostgreSQL as System of Record

## Status

Accepted

## Context

SentinelFlow needs reliable persistent storage for received monitoring events and application metadata.

## Decision

PostgreSQL will be the primary persistent database and system of record.

Spring Boot will access PostgreSQL using JPA/Hibernate.

## Consequences

Positive:

- Durable event storage
- Transaction support
- Strong consistency
- Powerful querying capabilities
- Mature Spring ecosystem integration

Trade-offs:

- Database schema changes must be managed carefully
- Future scaling strategies may require partitioning or additional infrastructure
