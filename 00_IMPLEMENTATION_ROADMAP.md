# Implementation Roadmap for DeepSeek

## Phase 0 — Specification confirmation (no coding)

Read every file. Return:
- concise understanding of product;
- list of all modules and ownership;
- dependency graph;
- proposed Go package layout;
- database ownership and RLS approach;
- job/outbox/scheduler flow;
- threat model;
- unresolved decisions with options;
- milestone plan and estimated deliverables;
- contradictions, ambiguities or missing requirements.

Wait for explicit product-owner approval. Do not write implementation code in Phase 0.

## Phase 1 — Architecture and decision closure

Submit proposed exact versions and libraries for Go, PostgreSQL, Redis, migrations, HTTP, queue, logging, metrics, tracing and frontend tooling. Explain trade-offs. Update `DECISIONS.md` only after owner approval. Produce ADRs for material choices.

## Phase 2 — Core Foundation

Implement in this order:
1. project bootstrap, config validation and lifecycle;
2. database pool, migrations and transaction helpers;
3. tenant context, authorization primitives and RLS;
4. Redis client and key conventions;
5. structured logging, errors, metrics and health;
6. secret encryption/key versioning;
7. local storage adapter;
8. session storage boundary;
9. outbox repository and dispatcher;
10. worker registry/runtime and retry/dead-letter;
11. durable scheduler coordination;
12. integration tests and Docker dev environment.

Each item must be runnable and tested before proceeding.

## Phase 3 — Identity and Tenant Management

Authentication, sessions, memberships, role/permission enforcement, tenant lifecycle, audit and tests.

## Phase 4 — Collection Engine

Implement `docs/08_ENGINE_COLLECTION.md`, using only approved Core contracts.

## Phase 5 — Processing Engine

Implement `docs/09_ENGINE_PROCESSING.md`, using only approved Core contracts.

## Phase 6 — Publishing Engine

Implement `docs/10_ENGINE_PUBLISHING.md`, using only approved Core contracts.

## Phase 7 — Integration

End-to-end flow with controlled adapters; test failure/retry/restart/duplicate scenarios and tenant isolation.

## Phase 8 — Platform, developer and customer UI

Implement API contracts and polished Persian-first UI. UI must not precede stable backend authorization contracts.

## Phase 9 — Extensions, billing and production hardening

Implement approved extension runtime/security model, billing provider integration, backup/restore drills, load/soak tests and operational runbooks.

## Phase 10 — Release candidate

Security review, migration review, restore drill, deployment smoke test, release notes and owner acceptance.

## Non-negotiable

Do not implement all phases in one giant output. Use incremental commits and milestones. Do not claim a phase complete when acceptance criteria fail.
