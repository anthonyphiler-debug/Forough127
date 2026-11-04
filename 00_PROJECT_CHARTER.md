# 00 — Project Charter

## 1. Vision

Forough 127 is a professional, extensible, multi-tenant news automation SaaS. It helps channel operators configure sources, collect news, validate and normalize items, process content according to channel-specific rules, review or approve outputs when required, and publish them to Eitaa destinations.

The product must be reliable enough for unattended operation while keeping customer data, credentials, processing rules and publishing destinations isolated.

## 2. Product principles

- Tenant isolation is a foundational security property, not a UI feature.
- Reliability and traceability take precedence over clever automation.
- Every asynchronous operation is durable, observable, retryable and idempotent.
- Business modules communicate through versioned contracts, not private implementation imports.
- Human review is supported as a configurable workflow where required.
- AI output is untrusted until validated.
- Extensions are capability-limited and auditable.
- The UI should be polished, fast, accessible and Persian-first.
- Configuration changes must be validated and auditable.
- The system must fail safely: never publish uncertain, unauthorized or cross-tenant content.

## 3. Target users and roles

- Platform Owner: owns product-wide configuration and operational control.
- Platform Administrator/Operator: manages tenants, support, platform operations and policy-bound interventions.
- Tenant Owner: owns a customer workspace and subscription.
- Tenant Administrator: manages users, sources, channels, settings and workflows within assigned permissions.
- Tenant Editor/Operator: reviews content and operational queues.
- Tenant Viewer/Analyst: read-only access to authorized reports.
- Developer/Extension Publisher: builds and submits extensions under platform policy; does not gain tenant data access by default.

Roles are templates. Effective permissions must be explicit and scoped; role names alone are not authorization.

## 4. Product boundaries

Included:
- Multi-tenant identity/workspaces and access control.
- Source management and news collection from supported websites and Eitaa sources.
- Normalization, validation, duplicate detection and processing.
- AI-assisted rewrite, summary and formatting according to channel configuration.
- Eitaa publishing and publication controls.
- Operational dashboard, audit, reports and usage metering.
- Platform/developer administration.
- Extensibility framework and extension governance.
- Billing/subscription foundations (payment provider remains undecided).
- Deployment, backup, recovery and observability.

Not included in the first delivery:
- Native Android application (later via Capacitor).
- Microservices decomposition.
- Arbitrary execution of untrusted third-party extension code without a reviewed sandbox.
- Guaranteed compatibility with undocumented or unstable third-party protocols.
- Autonomous publication when policy requires manual review.

## 5. Success criteria

- One tenant cannot read, infer, modify or trigger another tenant's data.
- A collected item can be traced from source through processing to publication.
- Restart/retry does not silently lose jobs or create duplicate publication.
- A failed source does not stop unrelated sources.
- AI provider failure does not corrupt source records or publish malformed content.
- Tenant operators can understand system state and resolve actionable failures.
- Platform operators can audit sensitive administrative actions.
- The platform can be deployed reproducibly on a clean Linux host.
