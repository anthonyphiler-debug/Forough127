# Task 01 — Core Foundation

## Objective

Deliver a runnable, tested shared infrastructure layer for all Forough 127 modules.

## In scope

- Go application bootstrap and lifecycle.
- Typed config and startup validation.
- PostgreSQL pgx v5 pool and transaction helpers.
- Tenant context and transaction-local RLS integration.
- Redis client and key policy.
- Structured logging, correlation IDs, error model.
- Health/readiness endpoints.
- Encryption abstraction and key version metadata.
- Local filesystem storage behind interface.
- Shared encrypted session repository boundary.
- Transactional outbox.
- Dispatcher, worker registry/runtime, retries, leases, dead letters.
- Generic durable scheduler coordination.
- Docker Compose for local PostgreSQL/Redis.
- CI and integration tests.
- Documentation and runbooks.

## Out of scope

- Website scraping and Eitaa source collection.
- News duplicate detection and AI transformation.
- Eitaa message publishing adapter.
- Channel publication activity windows and business schedule.
- Full customer/developer UI.
- Untrusted extension code execution.
- Payment gateway integration.

## Required demonstration

A sample internal test handler must receive a durable job created from a transactionally written outbox event, execute once logically despite duplicate delivery, persist outcome, retry transient failures, and recover after worker/dispatcher restart.

A sample scheduler task must create durable jobs without duplicate concurrent schedule execution.

## Acceptance criteria

1. Clean checkout builds with approved Go toolchain.
2. Config validation rejects missing/invalid required values.
3. Migrations apply from empty database.
4. Runtime DB role cannot bypass RLS or own protected tables.
5. Tenant A cannot access Tenant B rows, including pooled connection reuse.
6. Redis health and PostgreSQL health are independently correct.
7. Outbox event and business row commit atomically.
8. Multiple dispatchers do not concurrently own the same lease.
9. Expired leases are recovered.
10. Queue duplicate delivery does not duplicate logical side effects.
11. Retry exhaustion moves work to dead-letter with safe diagnostics.
12. Graceful shutdown stops accepting new work and completes/cancels in-flight work according to deadline.
13. Session records are tenant-scoped and encrypted at rest.
14. Storage blocks path traversal and cross-tenant access.
15. Health endpoints do not disclose secrets.
16. Unit and integration tests pass against real PostgreSQL and Redis.
17. Documentation matches actual behavior.

## Handoff

Publish stable contracts for tenant context, DB transaction, outbox event, job envelope, handler registration, scheduler, storage and session repository. Engine modules must not import Core private implementation packages.
