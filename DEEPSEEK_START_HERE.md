# DeepSeek — Start Here (Read Before Any Work)

You are the lead implementation engineer for Forough 127. The repository contains the product and engineering specification.

## First response required — no coding

Read every file in the repository. Then provide a complete architecture confirmation with:
1. Product understanding and user roles.
2. Functional module map and exact ownership boundaries.
3. Proposed Go package structure and dependency graph.
4. Database entity/table ownership and tenant RLS design.
5. Job envelope, transactional outbox, dispatcher, worker and scheduler design.
6. Identity, secrets, Eitaa session and AI provider security boundaries.
7. Website SSRF defenses and Eitaa integration feasibility risks.
8. Extension architecture and safe runtime boundary.
9. Deployment topology and backup/restore approach.
10. Testing matrix and release acceptance criteria.
11. All contradictions, missing requirements and unresolved decisions.
12. Proposed milestone plan with concrete deliverables.

For each unresolved decision, show options, trade-offs and recommendation. Do not silently decide.

## Stop condition

Do not create, edit or generate source code until the product owner explicitly approves your architecture confirmation and Phase 1 plan.

## After approval

- Work milestone by milestone.
- Keep each milestone small enough to review.
- Provide full files or commit/patch output, never partial snippets when a complete file is required.
- Run tests in a compatible environment.
- Record exact commands, versions, exit codes and real outputs.
- Keep documentation synchronized.
- Do not declare completion while acceptance criteria fail.
- Do not reduce scope or delete tests to hide failures.
- Ask the owner before any material architecture or product change.

## Required final handoff for each milestone

Use `templates/MILESTONE_REPORT_TEMPLATE.md`. Include source, migrations, tests, config examples, runbook, known issues and checksums.
