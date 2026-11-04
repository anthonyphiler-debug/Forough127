# 14 — Operations, Deployment, Backup and Recovery

## 1. Environments

- local developer environment;
- automated CI/test environment;
- staging;
- production Linux VPS.

Configuration must be environment-specific and secrets injected securely. Never commit `.env` with real values.

## 2. Containers and services

Provide Docker Compose for local PostgreSQL and Redis and optionally the app. Production may use Docker or systemd; document one supported reference deployment first.

## 3. Health endpoints

- liveness: process is alive and event loop is responsive;
- readiness: required dependencies and migrations are ready;
- dependency health: PostgreSQL, Redis, storage and worker state;
- health endpoint must not leak credentials or infrastructure internals.

## 4. Backup

Define:
- PostgreSQL full and incremental/PITR strategy where supported;
- media filesystem backup;
- encryption and access control;
- retention and off-host copy;
- backup integrity verification;
- restore drill schedule;
- RPO/RTO targets approved by owner.

A backup is not considered valid until a restore test succeeds.

## 5. Deployment

- build reproducibly from a tagged commit;
- run migrations as a controlled deployment step;
- deploy app and worker processes;
- verify readiness;
- run smoke tests;
- monitor errors and queue depth;
- rollback application and database changes according to documented compatibility strategy.

## 6. Security operations

- TLS;
- firewall and least-privilege service accounts;
- SSH key-based access;
- patch cadence;
- secret rotation;
- log retention;
- incident response;
- no public exposure of PostgreSQL/Redis management ports.
