# 09 — Processing Engine Specification

## 1. Ownership

Processing owns normalization refinements, duplicate detection, filtering, AI transformation, channel-specific formatting and review preparation. It does not fetch sources or send Eitaa messages.

## 2. Pipeline

1. accept versioned normalized item;
2. validate schema and provenance;
3. normalize language/text/time/media metadata;
4. compute deterministic fingerprints;
5. identify exact and probable duplicates within approved scope;
6. apply tenant/channel filters;
7. choose processing profile;
8. invoke AI provider adapter if configured;
9. validate output and enforce policy;
10. create immutable content version;
11. route to review or publishing handoff;
12. meter usage and record processing run.

## 3. Duplicate detection

Use multiple layers:
- exact external identity;
- canonical URL;
- normalized text hash;
- title/body similarity candidate scoring.

Never merge across tenants. Define matching scope and retention. Similarity detection must not automatically discard a story without explainable thresholds and auditability.

## 4. AI adapter

- provider-neutral interface;
- configurable endpoint/model/credentials;
- connection/read timeouts;
- bounded retries and rate limits;
- token/usage tracking where available;
- request/response schema validation;
- prompt template versioning;
- output length and language validation;
- safe fallback/manual review on provider outage;
- no provider secret in tenant-visible logs;
- prompt injection defenses.

## 5. Channel profiles

A channel profile may define:
- instructions and tone;
- summary length or title-only mode;
- allowed/blocked categories and keywords;
- signature and hashtag rules;
- formatting template;
- image policy;
- language;
- review requirement;
- publication eligibility.

The Processing engine must not own publication timing or Eitaa delivery.

## 6. Content versioning

Keep original normalized content, each AI output version, manual edits and final approved payload separately. Record actor, model, prompt version, timestamps and reason for changes.

## 7. Acceptance tests

- exact duplicate;
- same story across different sources;
- similar but distinct story;
- tenant isolation;
- provider timeout/429/5xx/malformed output;
- prompt injection content;
- title-only mode;
- manual review path;
- output length and formatting constraints;
- usage metering.
