# ADR-002: Redis for Temporary and Performance-Oriented Data

## Status

Accepted

## Context

SentinelFlow will require fast access to temporary data, counters and cached information.

## Decision

Redis will be used only for temporary or performance-oriented workloads.

Initial expected use cases:

- cache
- counters
- rate limiting
- temporary state
- future short-lived processing metadata

Redis will not replace PostgreSQL as the persistent source of truth.

## Consequences

Positive:

- Very fast reads and writes
- Suitable for counters and temporary state
- Reduces pressure on PostgreSQL

Trade-offs:

- Redis data must not be assumed to be permanently durable
- Application logic must tolerate cache loss
