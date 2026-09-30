# ADR-003: SentinelFlow Monorepo

## Status

Accepted

## Context

SentinelFlow contains multiple related components:

- Spring Boot backend
- React frontend
- Python analytics
- Docker infrastructure
- architecture documentation

## Decision

SentinelFlow will initially use a monorepo.

Planned structure:

sentinelflow/
+-- backend/
+-- frontend/
+-- analytics/
+-- infrastructure/
+-- docs/
+-- .github/

## Consequences

Positive:

- Centralized project history
- Easier local development
- Shared documentation
- Simplified portfolio presentation
- Coordinated changes across components

Trade-offs:

- Repository size will grow over time
- CI/CD pipelines must eventually detect which component changed
