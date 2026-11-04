# 04 — Tenancy, Identity, Authorization and Security

## 1. Threat model

Assume hostile users, malicious HTML, compromised credentials, prompt injection in source content, replayed webhooks, queue duplication, stale sessions, malicious extensions and accidental operator error.

Primary security objectives:
- prevent cross-tenant disclosure and mutation;
- protect Eitaa and AI credentials;
- prevent SSRF and unsafe file access;
- prevent unauthorized publishing;
- preserve auditability;
- limit blast radius of compromised workers/extensions.

## 2. Tenant context

Resolve tenant from authenticated user membership or an explicitly authenticated service identity. Never trust a raw `tenant_id` supplied by a browser as proof of membership.

For every request/job:
1. authenticate actor/service;
2. resolve tenant scope;
3. authorize action against resource and tenant;
4. establish tenant context for DB transaction;
5. execute operation;
6. emit audit/security events as required.

Background job envelopes must contain tenant ID, actor/service identity, correlation ID, idempotency key, schema version and payload reference. Workers must revalidate that the tenant exists and is active.

## 3. PostgreSQL RLS

- Enable RLS on tenant-owned tables.
- Use `FORCE ROW LEVEL SECURITY` where appropriate.
- Application roles must not own protected tables and must not have `BYPASSRLS`.
- Set tenant context transaction-locally using `SET LOCAL` or a safe parameterized mechanism.
- Never use session-global tenant variables with pooled connections.
- Ensure every repository operation runs in a transaction with tenant context.
- Test pooled-connection reuse to prove context does not leak between tenants.
- Cross-tenant platform operations require a separate narrowly privileged path, explicit authorization, audit and tests. Do not silently bypass RLS.
- Migrations run under a separate migration role, not the runtime role.

## 4. Authorization

- Deny by default.
- Check permission on backend application service boundaries.
- Scope permissions to tenant/resource/action.
- Separate platform and tenant roles.
- Require explicit elevated permission for secret viewing/rotation, destination changes, publishing, tenant suspension, billing changes and extension installation.
- Support revocation and session invalidation.
- Audit privileged actions.

## 5. Secrets and encryption

- Never store secrets in source control, logs, URLs or client-side code.
- Use envelope encryption or authenticated encryption (AES-256-GCM is an acceptable initial option) with key version metadata.
- Master encryption keys come from deployment secret injection, not the database.
- Define key rotation and re-encryption procedure.
- Keep secret references separate from secret values in normal business tables.
- Redact credentials, cookies, authorization headers, message contents where configured, and personal data from logs.
- Use TLS for external and internal network links where supported.

## 6. Web collection security

- Allow only HTTP/HTTPS.
- Resolve DNS and reject loopback, private, link-local, multicast and reserved IP ranges.
- Revalidate redirects and every resolved destination.
- Defend against DNS rebinding.
- Enforce response size, timeout, redirect count and content-type limits.
- Do not fetch arbitrary local files or cloud metadata endpoints.
- Use safe HTML parsing; never execute scripts.
- Sanitize extracted HTML before display.
- Respect publisher terms, access restrictions and rate limits.

## 7. AI security

- Treat source text and model output as untrusted data.
- Prompt injection in a news article must not change system instructions, tool permissions or publishing policy.
- AI cannot directly execute tools or alter configuration.
- Validate output schema, length, language, forbidden content and destination constraints.
- Keep secrets out of prompts.
- Store prompt versions and provider metadata without logging sensitive input unnecessarily.

## 8. Eitaa account security

- Store session/credential material encrypted.
- Bind session use to authorized tenant and purpose.
- Do not expose raw session material to UI, logs or extension APIs.
- Support revocation, health status and re-authentication.
- Separate session storage from business adapter logic.

## 9. Incident response

Provide procedures for:
- revoke compromised account/session;
- rotate encryption and provider keys;
- suspend tenant or extension;
- stop publishing globally or per tenant;
- quarantine failed/unsafe jobs;
- preserve audit evidence;
- restore service after compromise.
