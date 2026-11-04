# Task 02 — Collection Engine

Implement the full requirements in `docs/08_ENGINE_COLLECTION.md`.

Must support configured website category/listing pages and authorized Eitaa sources. Website URL patterns must be configurable and domain-neutral. Per-source maximum article count, extraction rules, age limits, timeouts, response limits and rate limits must be honored.

Use Core tenant context, storage, job and logging contracts. Implement source-scoped idempotency and durable handoff to Processing. Do not implement cross-source semantic deduplication, AI processing or publishing.

Before coding, submit architecture confirmation, data ownership, source adapter contracts, SSRF strategy and acceptance test plan. Wait for owner approval.
