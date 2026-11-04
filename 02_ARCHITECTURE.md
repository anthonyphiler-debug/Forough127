# 02 — System Architecture

## 1. Architecture style

Use a **Modular Monolith**: one deployable backend application with strict internal module boundaries. Do not split into microservices during initial implementation.

Modules:
- `platform`: configuration, database, Redis, security, tenancy, logging, metrics, queue, scheduler, storage, sessions, lifecycle.
- `identity`: users, authentication, memberships, roles and permissions.
- `tenants`: tenant lifecycle and workspace configuration.
- `collection`: website and Eitaa source collection.
- `processing`: validation, normalization extensions, duplicate detection, AI workflows and formatting.
- `publishing`: review handoff, scheduling and Eitaa delivery.
- `billing`: plans, entitlements, usage and subscription state.
- `extensions`: extension registry, permissions, lifecycle and extension points.
- `audit`: immutable or tamper-evident audit events.
- `reporting`: tenant and platform reports.

## 2. Approved technology baseline

- Go 1.27.3 toolchain.
- PostgreSQL 16, accessed through pgx v5.
- Goose for versioned SQL migrations.
- Redis 7.
- Asynq for background job execution, coordinated with the PostgreSQL transactional outbox.
- Go standard-library `net/http` for the initial HTTP server.
- Go standard-library `slog` for structured logging.
- Local filesystem storage behind a replaceable interface.
- AES-256-GCM for initial authenticated encryption with external master-key injection and key versioning.
- HTML/CSS/vanilla JavaScript; no TypeScript.

Pin exact container patch tags/digests and dependency versions in the implementation. Do not silently substitute major technologies.

## 3. Dependency rules

- Platform packages must not import business modules.
- Business modules may depend on platform interfaces and shared contracts.
- Business modules must not import another business module's private packages.
- Cross-module communication uses application interfaces, domain events or versioned contracts.
- No cyclic imports.
- Shared packages must contain stable primitives, not miscellaneous business logic.
- Each module owns its tables and migrations.
- Cross-module reads use explicit query interfaces or documented read models; do not reach into another module's private repository.

## 4. Suggested repository layout

```text
cmd/forough/
internal/
  app/
  platform/
    config/ database/ redis/ tenant/ authz/ crypto/
    logging/ metrics/ tracing/ errors/ jobs/ outbox/
    scheduler/ storage/ sessions/ httpserver/ lifecycle/
  shared/
    ids/ clock/ validation/ contracts/ pagination/
  modules/
    identity/
    tenants/
    collection/
    processing/
    publishing/
    billing/
    extensions/
    audit/
    reporting/
migrations/
  identity/
  tenants/
  collection/
  processing/
  publishing/
  billing/
  extensions/
  audit/
web/
  templates/
  static/
tests/
  integration/
  e2e/
deploy/
  docker/
  systemd/
  scripts/
docs/
```

This is a target structure, not a command to create empty folders. Only create packages when they contain working code, tests or a documented implementation boundary.

## 5. Runtime topology

Initial deployment may use:
- one HTTP/API process;
- one or more worker processes using the same codebase and configuration;
- PostgreSQL;
- Redis;
- local media directory mounted on persistent storage;
- reverse proxy/TLS termination.

The app must support separate API and worker process roles from the same binary/config where practical. A single-process development mode is acceptable if lifecycle and concurrency semantics remain correct.

## 6. Request flow

HTTP request → authentication → tenant resolution → authorization → validation → application service → domain/repository → transaction → outbox event → response.

Tenant identity must come from authenticated membership/context, never from an untrusted request field alone.

## 7. Background flow

Business transaction writes state and outbox event atomically → dispatcher claims event → durable queue receives envelope → worker validates envelope and tenant context → handler executes idempotently → state/outcome recorded → retry/dead-letter policy applies.

If a queue library is selected, it must not replace the transactional guarantee between DB commit and enqueue.

## 8. API conventions

- Version APIs under `/api/v1`.
- JSON request/response schemas.
- Stable machine-readable error codes.
- Request ID and correlation ID propagation.
- Cursor pagination for large collections.
- Explicit tenant context and authorization.
- Idempotency key for mutating operations that may be retried.
- OpenAPI specification generated/maintained alongside handlers.
- Never expose internal stack traces, secrets or raw provider errors to clients.

## 9. Frontend direction

- HTML, CSS and vanilla JavaScript; no TypeScript.
- Server-rendered pages or progressive enhancement are acceptable.
- RTL and Persian-first.
- Customer panel visual direction: polished violet/blue glassmorphism.
- Developer/platform panel visual direction: green/turquoise glassmorphism.
- Responsive desktop-first, usable on mobile.
- Keep UI state and authorization decisions on backend; frontend hiding is not security.
- Use accessible semantic controls and keyboard navigation.

## 10. Architecture validation

Before coding, DeepSeek must return:
- package dependency graph;
- module ownership table;
- runtime process diagram;
- database ownership map;
- event/job flow;
- tenant isolation strategy;
- threat model summary;
- proposed technology versions and unresolved choices.
