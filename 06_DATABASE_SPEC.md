# 06 — Database Specification

## 1. General rules

- PostgreSQL is the system of record.
- Use pgx v5.
- Use Goose versioned SQL migrations. Migration files are owned by their respective modules.
- Every module owns its tables and migration namespace.
- Use `timestamptz` UTC.
- Add created_at/updated_at where lifecycle requires.
- Add tenant_id to tenant-owned tables.
- Define foreign keys, unique constraints and indexes explicitly.
- Avoid JSONB as a substitute for relational integrity; use it for bounded flexible metadata only.
- Never use SQLite as a substitute for PostgreSQL integration tests.

## 2. Minimum table families

Identity: users, credentials, sessions, tenants, memberships, roles, permissions, role_permissions.

Platform: outbox_events, job_executions, audit_events, system_settings, media_objects, encrypted_secret_records.

Collection: sources, source_configs, source_checkpoints, collection_runs, collected_items, source_attachments.

Processing: news_items, news_item_versions, fingerprints, duplicate_clusters, processing_profiles, processing_runs, prompt_versions, review_tasks.

Publishing: destinations, publication_policies, publication_schedules, publication_jobs, publication_attempts, publication_receipts.

Billing: plans, subscriptions, entitlements, usage_ledger.

Extensions: extensions, extension_versions, extension_installations, extension_permissions, extension_migrations.

## 3. Required indexes and constraints

- tenant-scoped lookup indexes begin with tenant_id where appropriate.
- unique source item identity scoped by tenant + source + source external ID.
- unique idempotency keys within their intended tenant/operation scope.
- unique remote publication receipt where provider guarantees stable remote IDs.
- foreign keys must not permit cross-tenant parent-child relationships.
- status/time indexes for queue and operational scans.
- partial indexes for active/pending work where beneficial.

## 4. RLS policy requirements

- Define policy for every tenant-owned table.
- Define runtime, migration and narrowly scoped platform-operation DB roles.
- Runtime role cannot own tables or bypass RLS.
- Use transaction-local tenant context.
- Test no context, invalid context, tenant A, tenant B, pooled connection reuse and privileged platform workflows.
- Ensure views/functions do not accidentally bypass RLS; use security invoker unless a reviewed security-definer function is required.
- Security-definer functions must pin search_path and have narrowly granted execute permissions.

## 5. Migration rules

- migrations are immutable once applied in shared environments;
- forward and rollback strategy documented;
- destructive migration requires backup and owner approval;
- migration execution is separate from app startup in production;
- migration tests run against clean DB and upgrade from previous supported schema;
- each module declares ownership and dependencies.

## 6. Required schema deliverable

Before implementation, provide ER diagram, table dictionary, ownership matrix, constraints/index list, RLS matrix, migration ordering and sample tenant-isolation tests.
