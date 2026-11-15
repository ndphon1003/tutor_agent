---
name: implement-ticket
description: Implement an existing Tutor Marketplace Jira ticket from its confirmed requirements and reviewed implementation plan. Use when the user asks to implement, code, execute, or continue implementation of an identified Jira issue.
---

# Implement Ticket

## Goal

Implement an existing Jira ticket in the relevant Tutor Marketplace repositories, following the ticket's approved requirements, the reviewed technical plan, verified current source, and applicable KB conventions. Return an evidence-based implementation report.

`create-ticket` owns WHAT and acceptance criteria. `plan-ticket` owns proposed HOW. `implement-ticket` makes the authorized changes and reports what ACTUALLY changed. Do not mistake a proposed design for implemented behavior.

## Invocation

Use for `$implement-ticket TUTOR-123` or a clear request to implement or continue an identified Jira ticket. Additional text supplies constraints; it does not silently supersede confirmed project decisions.

An actual Jira issue is required. Resolve and read it; a draft or missing ticket does not qualify. If the issue or a material approval/decision is unavailable, report the blocker instead of inventing requirements or a replacement issue.

## Core principles

- Implement the smallest coherent change satisfying the approved ticket; preserve existing workflows and user changes.
- Distinguish **Confirmed**, **Planned**, **TBD**, **Proposed**, and **Verified implemented**. Neither a KB label nor a Jira plan alone proves runtime implementation.
- Follow `AGENTS.md`, `rules/general.md`, `rules/git.md`, `rules/jira.md`, `rules/knowledge-base.md`, and the relevant specialist agent instructions.
- A reviewed plan is guidance, not permission to add requirements. If plan and ticket conflict, flag the conflict; do not quietly rewrite the ticket or widen scope.
- No new business rule, security policy, API contract, schema, migration, or infrastructure dependency without grounding and any needed approval.
- Prefer existing code patterns and local workspace inspection to speculative paths. Record exact paths and the inspected branch/ref.
- Do not overwrite unrelated work or expose secrets. Never claim a test passed without its actual result.

## Context gathering

1. Read the Jira issue (description, acceptance criteria, status, relevant comments) through Atlassian MCP.
2. Identify the latest **reviewed/approved** implementation plan, normally in Jira comments. A plan posted to Jira is not automatically approved; confirm the decision when not evident. Separate unresolved proposals and blockers.
3. Read relevant `tutor_kb` pages: architecture, domain, API, database, product, decisions, and applicable standards. Inspect the owning documents rather than relying only on the KB README.
4. Inspect actual `tutor_backend` and/or `tutor_frontend` files and working tree/branch before editing. For database work, inspect source-controlled Flyway migrations and relevant database state through Supabase MCP when necessary.
5. Inspect dependencies, existing tests, and related tickets only when they affect this implementation.
6. Reconcile stale/conflicting KB pages with Jira and verified source. Flag discrepancies; do not silently treat an outdated statement as current.

Use relevant specialist guidance: `agents/backend.md`, `agents/frontend.md`, `agents/database.md`; route cross-layer work through `agents/orchestrator.md`. Where local execution is unavailable, disclose that limitation and do not imply remote GitHub inspection is a local test run.

## Pre-implementation scope and approval gate

Before changing any file or external state:

1. Summarize the issue and acceptance criteria, identified plan, affected repositories/layers, proposed change batch, and validation approach.
2. Identify any material discrepancy, missing decision, data migration risk, or unapproved plan change.
3. Request **explicit approval for the described implementation batch** in accordance with `AGENTS.md` and `rules/general.md`. Invoking this skill or approving an earlier plan is not, by itself, approval to edit files.
4. Proceed only within that approval. If scope changes materially, stop and request renewed approval.

Approval for code edits does not automatically authorize committing, pushing, opening PRs, Jira comments/transitions, KB writes, or live database changes. State each requested external action and obtain approval for that action or clearly specified batch. Respect any existing user edits.

## Implementation workflow

1. Recheck the issue and implementation plan against current source before modifying it; identify correct modules/files and existing test setup.
2. Implement the approved change in a focused sequence. Respect backend layering, Angular feature/core/shared conventions and SSR safety, validated API contracts, and database migration history when applicable.
3. Handle relevant success, failure, authorization, validation, concurrency, and state-lifecycle behavior required by the ticket. Do not expand scope for generic refactors.
4. Add or adjust focused tests for changed behavior when feasible. Run relevant project checks in the available environment, observing currently documented standards and actual repository configuration. The backend KB testing page documents a changed-Java-line coverage target greater than 85%; verify the applicable current setup and report measured evidence rather than asserting compliance.
5. Check diffs, migration/compatibility impact, accidental secrets, and unrelated changes. Distinguish changes made by this skill from pre-existing work.
6. Map **every** applicable Jira acceptance criterion to implementation evidence and verification evidence. `Implemented` does not mean `Verified`.
7. Where work establishes durable accepted knowledge, identify the KB pages/ADR to update. Only write those updates with their own approval; do not retroactively promote an unapproved decision to Confirmed.
8. Prepare the implementation report. If blocked or partially done, state exactly what is incomplete and how to continue.
9. Perform GitHub/Jira/DB write actions only when explicitly authorized; verify any resulting commit/PR/comment/state by read-back when possible. Do not mark Jira Done before required review and validation.

## Required output structure

Use the concise **Implementation Report** below. Omit irrelevant categories, not failed/unverified work. The report is a result, not a speculative implementation plan.

### 1. Implementation Summary
- Jira key and title; repository/branch or inspected ref.
- What was actually completed and current state.

### 2. Changes Made
- Group by affected Backend, Frontend, Database, Configuration, or Knowledge Base only when applicable.
- State meaningful behavior and important actual file paths. Identify any approved commit/PR links if created.

### 3. Acceptance Criteria
Use a table: `Criterion | Status | Evidence`.
- `Pass`: implemented and verified with evidence.
- `Fail`: tested and did not meet requirement.
- `Not Verified`: changed but not tested, not observable, or evidence insufficient.
- `Not Implemented`: requirement not delivered.
Do not claim Pass based only on code being present.

### 4. Validation & Testing
- Exact checks run, outcomes, important failures, and environment.
- Note skipped, unavailable, or unrun checks explicitly; distinguish local from CI results.

### 5. Deviations & Remaining Issues
- Conditional. Include approved departures from the plan, unresolved issues, regressions, blockers, and follow-up work. Separate approved changes from unapproved proposals.

### 6. Final Status
Choose exactly one: `Completed`, `Partially Completed`, or `Blocked`, with a concise rationale.
- `Completed` means in-scope changes are implemented with required verification; review approval is a separate later step.
- Never state that Jira is Done, code is merged, or the DB deployed unless verified.

## Report template

```markdown
## Implementation Report — TUTOR-123: <Title>

### 1. Implementation Summary
<Completed work and current status; repo/branch>

### 2. Changes Made
- **Backend:** <behavior and verified paths>
- **Database:** <migration or schema impact, if relevant>

### 3. Acceptance Criteria
| Criterion | Status | Evidence |
| --- | --- | --- |
| AC-1: <requirement> | Pass / Fail / Not Verified / Not Implemented | <file, test, or observation> |

### 4. Validation & Testing
- <check> — <Pass / Fail / Not Run; actual evidence>

### 5. Deviations & Remaining Issues
- <Only if applicable>

### 6. Final Status
**Completed / Partially Completed / Blocked** — <reason>
```

## Review handoff

Pass the issue, reviewed plan, changed files/branch or diff, tests and their results, acceptance-criteria mapping, and unresolved concerns to `review-implementation`. A self-reported `Completed` status is not a reviewer approval.

## Final quality check

- Actual Jira issue and plan were read; no proposed design was treated as an approved fact.
- Changes stayed within the authorized scope and preserved unrelated work.
- Relevant KB and source were checked; stale/conflicting documentation was surfaced.
- Every criterion has an honest status and concrete evidence.
- Tests and coverage claims are based on actual checks, with gaps disclosed.
- Migrations and external writes were not run merely because tools were available.
- No secret or unapproved business rule was introduced.
- Final status and any commit/Jira claims can be verified.
