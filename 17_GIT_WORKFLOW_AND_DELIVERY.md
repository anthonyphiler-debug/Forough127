# 17 — Git Workflow and Delivery Protocol

## 1. Branching

- `main` contains reviewed, owner-approved work only.
- Feature work uses short-lived branches such as `feature/core-db` or `fix/outbox-lease`.
- No direct pushes to main for implementation milestones.
- Use pull requests or patch bundles for review.
- Never force-push shared branches without explicit approval.

## 2. Commit conventions

Use clear imperative messages, optionally conventional prefixes:
- `feat(core): add tenant-aware transaction helper`
- `fix(outbox): recover expired dispatcher leases`
- `test(security): cover pooled connection tenant isolation`
- `docs(core): define worker lifecycle`

## 3. Milestone delivery package

Every milestone must include:
- source code;
- migrations;
- tests and fixtures;
- configuration example without secrets;
- README/runbook updates;
- change summary;
- exact commands executed;
- environment/tool versions;
- actual build/test outputs;
- known limitations and unresolved decisions;
- SHA-256 checksums for release ZIP if ZIP is provided.

## 4. Review rules

- One logical change per PR where practical.
- Explain schema changes and rollback.
- Do not mix formatting-only changes with large logic changes.
- Include tests for bug fixes.
- Owner approval required for architecture changes and scope reductions.
