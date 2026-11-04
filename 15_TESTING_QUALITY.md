# 15 — Testing and Quality Strategy

## 1. Test layers

- unit tests: pure logic and validation;
- repository tests: PostgreSQL behavior and constraints;
- integration tests: PostgreSQL, Redis, storage and queue;
- contract tests: module boundaries and API schemas;
- security tests: tenant isolation, authorization, SSRF, secret redaction;
- end-to-end tests: source → processing → review → publication using controlled test adapters;
- migration tests: clean install and upgrade path;
- load/soak tests for bounded concurrency and queue stability.

## 2. Mandatory quality gates

- `gofmt` clean;
- `go vet ./...`;
- `go test ./...`;
- race detector for concurrency-sensitive packages where feasible;
- dependency vulnerability scan;
- static security checks;
- migration validation;
- OpenAPI validation;
- frontend lint/format checks if tooling is selected;
- secret scanning.

Exact commands must be recorded in the milestone report.

## 3. Test integrity

- Never skip failing tests without documented reason and owner approval.
- Never replace integration tests with mocks when the behavior depends on PostgreSQL RLS, locking or Redis semantics.
- Use deterministic fixtures and isolated test tenants.
- Tests must clean up safely.
- Test logs must show actual command, exit code and environment versions.

## 4. Definition of release candidate

All critical/high issues fixed or explicitly accepted with mitigation; all acceptance tests pass; backup/restore procedure validated; deployment smoke test passes.
