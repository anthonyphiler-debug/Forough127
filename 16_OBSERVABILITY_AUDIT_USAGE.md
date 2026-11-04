# 16 — Observability, Audit and Usage Metering

## 1. Logs

Structured logs include timestamp, severity, service/process, request ID, correlation ID, tenant ID when safe, actor/service ID, module, operation and stable error code.

Do not log passwords, tokens, session strings, encryption keys, full cookies, secret headers or unnecessary personal/content data.

## 2. Metrics

Minimum metrics:
- HTTP request rate/latency/error rate;
- DB pool usage and query latency;
- Redis availability/latency;
- outbox pending count and oldest age;
- queue depth, retries and dead letters;
- worker concurrency and job duration;
- collection success/failure by source;
- processing latency/provider errors;
- publication success/failure/ambiguous outcomes;
- storage usage;
- tenant usage/entitlement consumption.

Avoid high-cardinality labels such as raw URLs, user IDs or unbounded tenant identifiers in metrics.

## 3. Audit

Audit sensitive events:
- login/security changes;
- membership/role/permission changes;
- source/destination changes;
- secret/session connect, revoke and rotate;
- publication override/retry/cancel;
- tenant suspend/reactivate;
- billing/entitlement changes;
- extension install/update/permission grant;
- platform support access.

Audit records should include actor, scope, action, target, timestamp, request/correlation ID, outcome and safe before/after summary. Never store secret values in audit payloads.

## 4. Usage metering

Use a ledger-like model for billable events. Events must be idempotent and reconcilable. Distinguish attempted, successful and billable usage according to explicit plan policy. UI display must not be the enforcement layer.
