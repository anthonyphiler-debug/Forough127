# 03 — Domain Model and Data Ownership

## 1. Core entities

Identity:
- User
- Credential / ExternalIdentity
- UserSession
- Tenant
- TenantMembership
- Role
- Permission
- RolePermission
- MembershipRole

Tenant configuration:
- TenantSetting
- TenantSecretReference
- Subscription
- Entitlement
- UsageRecord

Collection:
- Source
- WebsiteSourceConfig
- EitaaSourceConfig
- SourceCheckpoint
- CollectionRun
- CollectedItem
- SourceAttachment
- FetchAttempt

Processing:
- NewsItem
- NewsItemVersion
- ContentFingerprint
- DuplicateCandidate / DuplicateCluster
- ProcessingProfile
- ChannelInstruction
- ProcessingRun
- AIProviderConfigReference
- PromptTemplateVersion
- ReviewTask

Publishing:
- Destination
- DestinationCredentialReference
- PublicationPolicy
- PublicationSchedule
- PublicationJob
- PublicationAttempt
- PublicationReceipt

Platform:
- JobRecord / OutboxEvent
- AuditEvent
- SystemSetting
- Extension
- ExtensionVersion
- ExtensionInstallation
- ExtensionPermissionGrant
- ExtensionMigrationRecord
- MediaObject

Names are conceptual. Exact columns and constraints must be specified in `docs/06_DATABASE_SPEC.md` before implementation.

## 2. Tenant ownership

Every tenant-owned record must carry `tenant_id` or be reachable only through a tenant-owned parent with enforced constraints. Prefer explicit `tenant_id` on high-risk/high-volume tables to simplify RLS and indexes.

Global records must be explicitly marked platform-scoped and must not contain tenant secrets or tenant content.

## 3. Data lifecycle

- Raw source payload: immutable or append-only, retention-limited.
- Normalized news: versioned; source provenance preserved.
- AI outputs: versioned; never overwrite raw content.
- Publication attempts: append-only history.
- Audit events: append-only and access-controlled.
- Usage records: append-only or ledger-like to support reconciliation.
- Media: metadata in DB, bytes in storage abstraction.

## 4. IDs and time

- Use opaque, non-sequential public identifiers where exposure is possible.
- Internal primary keys may be UUIDs.
- Store timestamps as UTC `timestamptz`.
- Store original source timestamp and retrieval timestamp separately.
- Display in tenant-configured timezone; default product timezone is Asia/Tehran.
- Jalali conversion is presentation-only; never store Jalali strings as canonical timestamps.

## 5. Deletion and retention

Deletion must consider dependent jobs, files, audit records, legal holds and backup retention. Support soft-delete/archive where appropriate, followed by controlled purge. Define per-data-class retention before production. Do not promise immediate physical erasure from backups unless backup lifecycle supports it.
