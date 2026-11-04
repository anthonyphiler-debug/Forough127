# Binding Decision Register

This file records decisions that DeepSeek must not change unilaterally.

| ID | Decision | Status |
|---|---|---|
| D-001 | Product name is **Forough 127**. | Fixed |
| D-002 | Product is a multi-tenant SaaS platform. | Fixed |
| D-003 | Backend language is Go. | Fixed |
| D-004 | Backend architecture is a Modular Monolith, not microservices. | Fixed |
| D-005 | Primary database is PostgreSQL. | Fixed |
| D-006 | Redis is used for cache, coordination and job-queue support where justified. | Fixed |
| D-007 | Frontend uses HTML, CSS and JavaScript; TypeScript is not permitted. | Fixed |
| D-008 | Initial deployment target is Linux VPS; local development must also be supported. | Fixed |
| D-009 | Persian-first interface, Jalali calendar display, and Asia/Tehran timezone handling are required. Store timestamps in UTC. | Fixed |
| D-010 | Customer/tenant data must be isolated across every application boundary, including DB, cache, jobs, files, logs and integrations. | Fixed |
| D-011 | News collection, processing and publishing are separate business modules with explicit contracts. | Fixed |
| D-012 | Core Foundation owns shared infrastructure; business engines own their business workflows. | Fixed |
| D-013 | Extension architecture is required, custom-built for Forough 127; extension runtime and install/upgrade lifecycle must be security-designed before implementation. | Fixed |
| D-014 | Android is a later delivery using Capacitor; it is not part of the initial backend milestone. | Fixed |
| D-015 | Initial media storage is local filesystem behind a replaceable storage interface. | Fixed |
| D-016 | PostgreSQL access uses pgx v5; schema changes use Goose versioned SQL migrations. | Fixed |
| D-017 | Reliable asynchronous work uses Redis + Asynq for execution, with PostgreSQL Transactional Outbox for reliable DB-to-queue handoff. | Fixed |
| D-021 | Core baseline database is PostgreSQL 16; use a pinned patch image in CI/development and document the exact production patch. | Fixed |
| D-022 | Core baseline Redis major version is 7; pin the exact image tag/digest in deployment files. | Fixed |
| D-023 | Go toolchain baseline is Go 1.27.3 as specified by the product owner; pin it in go.mod/toolchain and CI. Do not silently downgrade. | Fixed |
| D-024 | Use Go standard library net/http for the initial HTTP server unless an ADR approved by the owner justifies a router dependency. | Fixed |
| D-025 | Use Go standard library slog for structured application logging initially; expose interfaces so exporters can be added. | Fixed |
| D-026 | Initial storage adapter is local filesystem behind a replaceable interface. | Fixed |
| D-027 | Initial authenticated encryption uses AES-256-GCM with key version metadata; master keys are injected outside the database. | Fixed |
| D-018 | Shared Eitaa session infrastructure belongs to Core; collection and publishing adapters belong to their respective engines. | Fixed |
| D-019 | AI provider integration is isolated behind an adapter. Provider credentials are tenant-scoped or platform-scoped according to explicit ownership and never exposed to clients. | Fixed |
| D-020 | Do not reuse the previous Core ZIP as implementation baseline. This is a clean implementation restart. | Fixed |

## Decisions intentionally not fixed yet

These require evidence, a written proposal and explicit product-owner approval before implementation:
- Exact PostgreSQL and Redis patch versions/image digests used by production.
- Exact validation library (standard-library validation is acceptable if consistently implemented).
- Exact Eitaa client/library/protocol and legal/operational compatibility.
- Exact AI provider endpoint/model defaults and provider fallback policy.
- Production object storage provider and retention/legal policy.
- Billing provider, tax rules and payment integration.
- Exact extension sandbox/runtime model and signing/distribution trust model.
- Production observability vendor.
- Domain, SMTP provider, SMS/OTP provider and hosting topology.

## Decision protocol

For every unresolved choice DeepSeek must submit:
1. The problem and constraints.
2. At least two viable options where meaningful.
3. Security, reliability, operational and maintenance trade-offs.
4. Recommended option and rationale.
5. Migration/rollback implications.
6. Tests needed.
7. Explicit question requiring owner approval.

Do not convert a recommendation into a binding decision without approval.
