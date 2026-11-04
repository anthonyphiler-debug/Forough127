# 05 — Core Foundation Specification

## 1. Mission

Core Foundation provides reusable infrastructure to all modules. It does not implement news collection, AI business processing or Eitaa publication workflows.

## 2. Required capabilities

### Application lifecycle
- deterministic startup ordering;
- dependency initialization and health validation;
- graceful shutdown with deadline;
- cancellation propagation;
- cleanup on partial startup failure;
- signal handling for Linux;
- no leaked goroutines/connections.

### Configuration
- typed configuration structs;
- environment variable and file support as approved;
- validation at startup;
- safe defaults only for non-sensitive settings;
- redaction when displaying config;
- no secret values in logs.

### Database
- pgx v5 pool;
- connection limits/timeouts;
- transaction helper;
- tenant-aware transaction helper;
- health check;
- migration runner separated from runtime process;
- readiness check distinct from liveness.

### Redis
- connection pool/timeouts;
- health check;
- namespacing and tenant-aware key conventions;
- TTL policy;
- no use as sole durable source of truth.

### Tenant context and authorization primitives
- request/job context type;
- tenant context propagation;
- actor/service identity;
- transaction-local PostgreSQL tenant setting;
- context validation and tests.

### Job and Outbox
- versioned job envelope;
- transactional outbox repository;
- dispatcher with atomic claim/lease;
- bounded concurrency;
- retry policy with exponential backoff and jitter;
- dead-letter state and operator visibility;
- idempotency;
- stale lease recovery;
- graceful shutdown;
- metrics and structured logs.

### Scheduler
- durable schedule definitions or durable trigger mechanism;
- timezone-aware trigger calculation;
- no duplicate schedule execution across replicas;
- missed-run policy;
- pause/resume;
- per-tenant limits;
- business schedule policy remains in owning module.

### Storage
- interface for put/get/delete/head/list as required;
- local filesystem initial adapter;
- tenant-scoped paths and authorization before access;
- path traversal prevention;
- atomic writes;
- size limits and content metadata;
- cleanup and retention hooks;
- future object storage replacement without business module changes.

### Session infrastructure
- encrypted session repository;
- tenant-scoped access;
- versioned credentials;
- status and revocation;
- no Eitaa-specific business calls in Core.

### Observability
- structured logs;
- request/correlation/job IDs;
- metrics;
- readiness/liveness;
- audit event interface;
- secret and PII redaction.

## 3. Explicit exclusions

Core must not implement:
- website scraping;
- Eitaa source message collection;
- cross-source news duplicate logic;
- AI prompts or summarization;
- channel content formatting;
- Eitaa message sending;
- publication time windows and per-channel posting intervals;
- billing provider integration;
- arbitrary extension code execution.

## 4. Required Core deliverables

- runnable application entry point;
- real PostgreSQL and Redis integration;
- reproducible migrations;
- functional outbox/dispatcher/worker path demonstrated by a test handler;
- scheduler infrastructure demonstrated by a test task;
- tenant isolation tests;
- storage tests;
- encrypted session store tests;
- health/readiness endpoints;
- Docker-based local development;
- CI workflow;
- operations documentation.

## 5. Definition of done

Core is not complete if it only defines interfaces. Each infrastructure capability must have a real implementation or a deliberately deferred, explicitly approved decision with no false completion claim.
