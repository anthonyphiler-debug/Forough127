# 11 — API Contracts

## 1. General

- Base path `/api/v1`.
- JSON over HTTPS.
- Authentication via secure server-managed session or approved token mechanism.
- CSRF protection for cookie-authenticated state-changing browser requests.
- Request ID, correlation ID and idempotency key where appropriate.
- Stable error object: `code`, `message`, `request_id`, optional safe `details`.
- Do not expose stack traces, SQL errors, internal hostnames or provider credentials.

## 2. API groups

- `/auth/*`: login, logout, session, recovery, MFA lifecycle.
- `/tenants/*`: workspace selection and tenant settings.
- `/members/*`, `/roles/*`, `/permissions/*`.
- `/sources/*`, `/collection-runs/*`.
- `/news/*`, `/processing-profiles/*`, `/review-tasks/*`.
- `/destinations/*`, `/publication-policies/*`, `/publications/*`.
- `/usage/*`, `/subscriptions/*`.
- `/audit/*`, `/reports/*`.
- `/developer/extensions/*`.
- `/platform/*`: platform-only operations with separate authorization.

## 3. Contract requirements

Before implementing each endpoint document:
- method/path;
- purpose;
- authentication;
- required permission;
- tenant scope;
- request schema and validation;
- response schema;
- error codes;
- idempotency/retry behavior;
- pagination/filter/sort;
- audit requirement;
- rate limit.

## 4. OpenAPI

Maintain an OpenAPI 3.x document. CI must validate it. API changes must include contract tests and compatibility notes.
