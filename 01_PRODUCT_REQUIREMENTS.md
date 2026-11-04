# 01 — Product Requirements

## 1. Main user journey

1. Tenant is created and configured.
2. Tenant owner invites users and assigns scoped permissions.
3. Tenant configures source accounts and source definitions.
4. Tenant configures one or more output channels and connects authorized Eitaa destinations.
5. Collection workers discover and retrieve new source items.
6. The system validates, normalizes, fingerprints and stores collected items.
7. Processing applies tenant/channel-specific filtering, duplicate detection, AI transformation and formatting.
8. Policy determines automatic publication, scheduled publication, or manual review.
9. Publishing sends approved content and records the remote result.
10. Dashboards, reports, usage and audit records expose the lifecycle.

## 2. Functional capabilities

### 2.1 Tenant and user management
- Create, suspend, reactivate and archive tenants according to platform policy.
- Invite users, revoke access, reset sessions and manage memberships.
- Support multiple users per tenant and users belonging to multiple tenants if approved by the identity design.
- Enforce tenant-scoped authorization on every request and background task.
- Support MFA-ready identity design; exact provider/method may be decided later.

### 2.2 Sources
- Website source configuration: name, category/listing URL(s), extraction strategy, article URL matching rule, request policy, scan interval, maximum items per scan, age limits, enabled state and source-specific metadata.
- Support multiple publishers and domains without hardcoded hostnames.
- Eitaa source configuration: authorized account/session reference, source channel identifiers, access status, checkpoint and collection settings.
- Record source health, last successful scan, last error and collection statistics.
- Support pause/resume and per-source rate/concurrency limits.
- Website fetching must defend against SSRF, unsafe redirects, private IP ranges, DNS rebinding, oversized responses and hostile HTML.
- Respect applicable legal restrictions, robots/publisher terms and rate limits; do not bypass authentication or access controls.

### 2.3 News item lifecycle
- Preserve source identity, original URL/message ID, source timestamps, retrieval timestamp and raw-source provenance.
- Normalize title, body, author, publication time, canonical URL, language and media references.
- Keep raw and normalized forms with retention controls.
- Track state transitions with timestamps and reason codes.
- Distinguish source-level idempotency from cross-source duplicate detection.
- Never silently overwrite original source data with AI output.

### 2.4 Processing
- Configurable rules per tenant and channel.
- Filtering by keywords, categories, source, age, language and tenant-defined policy.
- Duplicate detection across configured scopes.
- AI transformation behind provider adapter: summarize, rewrite, classify, extract structured fields and format.
- Channel instructions, style rules, length limits, signature, hashtags and formatting.
- Validate model output against schema and safety/publishing constraints.
- Record model/provider, prompt version, token/usage estimates where available, latency, outcome and error.
- AI output must not be trusted as a command or allowed to alter system configuration.
- Support no-AI/manual processing paths where configured.

### 2.5 Review and approval
- Configurable per-channel policy: auto-publish, review-required, or hybrid.
- Review queue with approve, reject, edit and return-for-reprocessing actions subject to permission.
- Record reviewer, decision, reason and timestamp.
- Prevent publication of rejected, expired, unauthorized or superseded content.

### 2.6 Publishing
- Publish text and supported media to configured Eitaa destinations.
- Validate destination ownership/access and content constraints before sending.
- Schedule publication with tenant/channel timezone policy; store canonical timestamps in UTC.
- Enforce channel activity windows, delay/interval, rate limits and daily caps where configured.
- Use idempotency keys and remote-result reconciliation to avoid duplicate posts.
- Retry transient errors with bounded exponential backoff and jitter; do not retry permanent authorization/content errors indefinitely.
- Record remote message identifiers and publication receipts.
- Provide manual retry and cancellation with authorization and audit.
- Do not claim success until remote acknowledgement is confirmed.

### 2.7 Platform operations
- Platform-level tenant status and service health.
- Audit trail for sensitive changes.
- Operational queues for failed jobs, dead-letter items and integration issues.
- Usage metering by tenant and feature.
- Support interventions must be permissioned, time-bounded where possible and audited.

### 2.8 Developer and extension system
- Extension catalog, metadata, version compatibility, permissions, plan availability and lifecycle state.
- Extension points may add tenant/developer menu entries, pages, backend API routes, event hooks, background jobs, settings screens and optional migrations/tables.
- Extensions must declare capabilities and request only necessary permissions.
- Extension data is tenant-scoped unless explicitly platform-scoped.
- Installation, enablement, update, disablement and removal must be transactional or recoverable.
- Extension code must not receive unrestricted access to process memory, filesystem, secrets or database.
- Runtime/sandbox and signing model must be approved before executable third-party extensions are enabled.

### 2.9 Billing and usage
- Subscription plans, entitlements, usage counters and plan restrictions.
- Enforce entitlement checks in backend services, not only UI.
- Meter source scans, processed items, AI usage, publication and storage as applicable.
- Billing provider and local legal/tax implementation are separate decisions.

## 3. Non-functional requirements
- Security: least privilege, defense in depth, tenant isolation, secure secret handling.
- Reliability: durable jobs, retries, idempotency, recovery and graceful shutdown.
- Performance: bounded concurrency, pagination, indexes and backpressure.
- Scalability: horizontal worker scaling without duplicate execution.
- Maintainability: modular monolith, clear ownership and dependency rules.
- Observability: structured logs, metrics, traces/correlation IDs and audit events.
- Accessibility: keyboard support, contrast, responsive layouts and semantic markup.
- Localization: Persian-first, RTL, Jalali display; internal timestamps UTC.
- Privacy: data minimization, retention, export and deletion policies.
- Portability: local development and Linux VPS deployment.

## 4. Acceptance principle

Every requirement must map to an implementation component, an automated test or a documented manual acceptance procedure. Unmapped requirements are not considered delivered.
