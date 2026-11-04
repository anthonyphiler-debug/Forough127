# 10 — Publishing Engine Specification

## 1. Ownership

Publishing owns destination configuration, Eitaa delivery adapters, publication policies, schedules, rate limits, retries, receipts and publication status. It does not collect source news or generate AI summaries.

## 2. Destination management

- bind destination to tenant;
- verify account access/permission before activation;
- store session/credential reference, not raw secrets;
- support health state, disconnect/re-authentication and audit;
- prevent destination IDs from being reassigned across tenants without explicit secure workflow.

## 3. Publication workflow

- accept only approved/eligible content;
- validate destination and payload;
- calculate next eligible time using tenant/channel timezone;
- enforce activity window, interval, daily cap and rate limits;
- create durable publication job;
- attempt send through adapter;
- record attempt and remote acknowledgement;
- reconcile ambiguous outcomes before retry;
- store remote message ID/receipt;
- expose success/failure/pending/review status.

## 4. Duplicate prevention

- stable publication idempotency key;
- record request fingerprint;
- before retry after timeout, reconcile remote state where supported;
- never blindly resend an ambiguous request if it may already have succeeded;
- enforce unique constraints on intended publication identity.

## 5. Retry and failure classes

Transient: network timeout, temporary server error, rate limit with retry-after.
Permanent: invalid destination, permission denied, unsupported payload, revoked session.
Ambiguous: request timed out after possible remote acceptance; requires reconciliation policy.
Retries are bounded and observable.

## 6. Manual controls

- pause/resume destination or channel;
- cancel pending publication;
- retry eligible failed attempt;
- force publish only with elevated permission and audit;
- global emergency stop available to authorized platform operators.

## 7. Acceptance tests

- unauthorized destination;
- expired/revoked session;
- activity-window boundary;
- interval/cap enforcement;
- duplicate job delivery;
- ambiguous timeout reconciliation;
- retry-after handling;
- shutdown while sending;
- tenant isolation;
- emergency stop.
