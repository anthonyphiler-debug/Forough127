# 08 — Collection Engine Specification

## 1. Ownership

Collection owns source configuration, discovery, retrieval, parsing, checkpointing and normalized handoff. It does not own cross-source duplicate decisions, AI transformation or publishing.

## 2. Website sources

Per source configure:
- display name and enabled state;
- one or more category/listing URLs;
- article URL matching rule (pattern/regex or structured extraction rule);
- allowed host/domain policy;
- scan interval or inherited global interval;
- maximum complete articles per scan;
- maximum article age;
- request timeout, response size and concurrency limits;
- extraction strategy and selectors/metadata rules;
- language and timezone hints;
- source-specific rate limits.

Requirements:
- no hardcoded publisher domain;
- discover only links matching configured rule and allowed host policy;
- normalize canonical URL;
- fetch article page and extract title/body/time/author/image;
- image must originate from the article itself or configured source metadata; do not search unrelated sites or synthesize replacement images;
- preserve original URL and provenance;
- tolerate missing optional fields;
- isolate failures per URL/source;
- protect against SSRF, unsafe redirects and hostile content;
- do not execute page JavaScript.

## 3. Eitaa sources

- use shared Core session manager;
- only collect from sources the authorized account can access;
- store stable external message identifiers;
- checkpoint per tenant + source;
- handle edited/deleted messages according to explicit policy;
- collect text, caption and supported attachments;
- do not expose session credentials to Collection business code beyond adapter interface.

## 4. Idempotency and handoff

- source-level idempotency key = tenant + source + external item identity/canonical source identity;
- repeated scans must not create duplicate source records;
- handoff to Processing is durable and retryable;
- Processing unavailable must not lose collected items;
- cross-source semantic duplicate detection belongs to Processing.

## 5. Operational behavior

- one bad source cannot stop others;
- bounded per-host concurrency;
- retry transient HTTP/network failures only;
- classify 4xx/auth/blocked/parse failures;
- expose run stats and source health;
- support pause/resume and manual scan with authorization.

## 6. Acceptance tests

- multiple domains and URL patterns;
- relative and absolute article links;
- redirect to disallowed/private IP;
- malformed/oversized HTML;
- missing image/date/body;
- duplicate scan;
- tenant isolation;
- checkpoint resume;
- partial source failure;
- Processing handoff outage and recovery.
