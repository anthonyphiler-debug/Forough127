# 07 — Jobs, Transactional Outbox and Scheduler

## 1. Reliability model

A database transaction must not commit business state while silently losing the event that triggers downstream work. Business state change and outbox event are written in the same PostgreSQL transaction. A dispatcher later transfers the event to Redis/Asynq. Asynq is the initial execution queue; PostgreSQL Outbox remains the durability bridge.

## 2. Outbox event fields

At minimum:
- event_id;
- tenant_id (nullable only for explicitly platform-scoped events);
- aggregate_type and aggregate_id;
- event_type;
- schema_version;
- payload or payload_reference;
- occurred_at;
- available_at;
- status;
- attempt_count;
- lease_owner and lease_expires_at;
- last_error_code (sanitized);
- created_at and processed_at.

## 3. Claim and dispatch

- Claim events atomically using row locking/skip-locked or another proven concurrency-safe mechanism.
- A claimed event has an expiring lease.
- Multiple dispatcher replicas must not dispatch the same lease concurrently.
- Enqueue operation must be idempotent or reconcile ambiguous outcomes.
- Mark dispatched only after queue acknowledgement.
- If queue acknowledgement is ambiguous, safe duplicate delivery is preferable to lost work; handlers must be idempotent.
- Reclaim expired leases.
- Bound batch size and concurrency.
- Apply backoff and jitter.
- Dead-letter after configured policy; preserve diagnostic context without secrets.
- Provide operator retry/requeue controls with audit.

## 4. Job envelope

Required fields:
- job_id;
- tenant_id or explicit platform scope;
- job_type;
- schema_version;
- idempotency_key;
- correlation_id;
- causation_id;
- actor/service identity;
- created_at;
- deadline/expiry if relevant;
- payload reference or validated payload.

## 5. Worker contract

- register handler by stable job type and version;
- validate envelope and tenant state;
- establish tenant context;
- enforce concurrency and timeout;
- acknowledge only after durable outcome;
- retry transient failures;
- classify permanent failures;
- make side effects idempotent;
- emit structured logs and metrics;
- support graceful shutdown.

## 6. Scheduler

- schedule definitions are durable;
- timezone and DST behavior are explicit;
- replicas coordinate to avoid duplicate runs;
- missed-run policy: skip, run-once or catch-up must be configurable per schedule class;
- schedule execution creates normal durable jobs;
- scheduler does not contain collection or publishing business logic.

## 7. Mandatory tests

- crash before enqueue;
- crash after enqueue before outbox mark;
- duplicate queue delivery;
- lease expiry and recovery;
- two dispatchers racing;
- worker crash mid-handler;
- retry exhaustion/dead-letter;
- tenant context propagation;
- disabled/suspended tenant;
- graceful shutdown.
