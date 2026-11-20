---
name: review-implementation
description: Independently review implementation of an existing Tutor Marketplace Jira ticket against its acceptance criteria, reviewed plan, current source diff, tests, and confirmed project knowledge. Use when asked to review, validate, or assess ticket implementation.
---

# Review Implementation

## Goal

Conduct an evidence-based, independent review of a completed or partial Jira-ticket implementation. Check correctness, scope, security, data integrity, regression risk, and validation coverage. Produce actionable findings and a clear verdict **without silently fixing the code**.

`create-ticket` defines WHAT, `plan-ticket` proposes HOW, `implement-ticket` records ACTUAL changes, and this skill independently compares all three against current source and verified tests.

## Invocation

Use for `$review-implementation TUTOR-123` or a request to review an identified Jira ticket's implementation; a PR/branch can be provided as additional review target.

Resolve and read the existing Jira issue. Identify the exact implementation under review: PR, commit range, branch, working-tree diff, or explicitly supplied patch. If there is no inspectable implementation, report `Blocked / Unable to Verify` rather than pretending a review occurred. Never compare arbitrary `main` to assumptions about local uncommitted work.

## Core review principles

- Be independent of the implementer's summary; use it to locate evidence, not as proof of correctness.
- Prioritize behavior and regressions over naming/style. Report findings grounded in specific code paths, observable behavior, tests, or requirements.
- Review against Jira acceptance criteria, the **reviewed/approved** plan, relevant confirmed KB rules, current source, and explicit user decisions. Approval of a plan is not proof implementation is correct.
- Distinguish `Confirmed`, `Planned`, `TBD`, and actual implementation. If sources conflict or KB is stale, describe the discrepancy and which claims can be verified.
- Do not invent a bug, risk, API contract, schema rule, or test result merely to populate a section.
- Default to read-only review; do not modify source, KB, DB, GitHub, or Jira as part of reviewing.

## Context gathering

1. Read the Jira issue, acceptance criteria, status, and relevant comments through Atlassian MCP.
2. Identify the reviewed implementation plan and any approved deviations; locate the implementation report if available.
3. Identify the exact review target and base/head revisions. Inspect the complete applicable diff and enough surrounding source to understand behavior; include related call sites, migrations, and tests where material.
4. Read relevant current `tutor_kb` architecture/domain/API/database/standards/decision pages. Do not rely solely on the README; cross-check time-sensitive or stale statements against code and Jira.
5. Verify actual backend/frontend patterns in the appropriate repositories; inspect Supabase only if live state matters and distinguish it from migration files.
6. Apply `AGENTS.md`, `rules/general.md`, `rules/git.md`, `rules/jira.md`, `rules/knowledge-base.md`, and `agents/reviewer.md`, plus relevant specialist guidance.

If a required source cannot be accessed, name the gap and avoid an unsupported approval. An inaccessible local workspace is not equivalent to clean git status.

## Review workflow

1. Establish exact issue scope, reviewed plan, diff/PR/branch target, and evidence available.
2. Inspect each affected layer for behavior, API and database contracts, authentication/authorization boundaries, failures, state transitions, concurrency, compatibility, and unintended scope expansion where applicable.
3. Compare **every** Jira acceptance criterion with the code and verification evidence; distinguish implemented behavior from successfully tested behavior.
4. Check tests: meaningful coverage of changed behavior, negative/boundary cases, regressions, migrations, and CI/local results when accessible. Consult `tutor_kb/standards/testing.md` and verify current CI configuration. If changed executable Java coverage is required above 85%, report actual measured status; never infer it from the presence of JaCoCo.
5. Identify concrete findings; for each provide importance, issue, exact evidence (path and lines or diff hunk when possible), impact, and smallest recommended fix.
6. Rate every finding by importance using exactly three levels, ordered `High`, `Medium`, `Low`:
   - `High`: serious security exposure, data loss/corruption, failure of a core or required flow, or a material regression in existing features with no reasonable workaround.
   - `Medium`: a functional defect, partial requirement failure, or regression with limited scope or a reasonable workaround; also a code-quality defect with a demonstrated maintenance or reliability cost.
   - `Low`: a narrowly scoped, non-critical defect or maintainability issue with small impact; it may be a nonblocking follow-up.
   Explain the rating using observed impact, affected users/features, conditions or likelihood, and available workarounds where supported. Do not call speculative style preferences defects or treat missing evidence alone as a proven high-importance bug.
7. State documentation inconsistencies, missing tests, inaccessible verification, or unresolved requirements separately as appropriate. Avoid duplicate findings for the same root cause.
8. Produce the review report and verdict. Return actionable fixes to `implement-ticket` for another approved implementation pass.

## Approval and write boundaries

Review and report drafting are read-only. Do not automatically:
- fix or reformat code;
- commit, push, create PRs, merge, or post GitHub reviews;
- change Jira status, comment, or issue description;
- update KB, execute migrations, or mutate live database state.

If the user requests any such write, present the exact target and proposed content/action, obtain explicit approval for that scoped write in accordance with `AGENTS.md`, and verify the result. A review request alone never grants write permission. Reviewer `Approved` is an assessment, not a GitHub PR approval action or Jira Done transition.

## Required output structure

### 1. Review Summary
- Jira key/title, exact source target and revisions reviewed, overall assessment and limits.

### 2. Requirement Compliance
Use a table: `Criterion | Result | Evidence`.
- `Pass`: evidence supports required behavior and applicable verification.
- `Fail`: implementation demonstrably violates the criterion.
- `Unverified`: evidence or testing insufficient.
- `Not Implemented`: criterion absent from the reviewed implementation.

### 3. Findings
Group in importance order (`High`, `Medium`, `Low`) when findings exist. Each finding:
- **Importance:** exactly one of `Low`, `Medium`, `High`, with a brief justification based on impact and scope.
- **Issue:** precise defect or mismatch, not a generic warning.
- **Evidence:** file and line/diff hunk, behavior, or failed check.
- **Impact:** affected requirement, user or system behavior.
- **Recommended Fix:** minimal actionable correction without writing code.
If none, state `No actionable findings identified within reviewed scope`; do not fabricate issues.

### 4. Test & Validation Review
State tests/CI reviewed, their actual results, meaningful uncovered scenarios, and environment constraints. Separate `not run` from `failed` and source inspection from execution.

### 5. Final Verdict
Choose exactly one:
- **Approved** — no material findings, applicable criteria satisfied with adequate evidence, and no blocking verification gaps.
- **Changes Requested** — one or more actionable defects or acceptance-criteria failures require changes.
- **Blocked / Unable to Verify** — essential code, requirement, plan decision, or verification evidence is missing, making an honest approval impossible.

A `Low` finding need not block approval if it has no material correctness impact; explicitly state any nonblocking follow-up. Do not mark Jira Done or merge as a side effect.

## Review report template

```markdown
## Implementation Review — TUTOR-123: <Title>

### 1. Review Summary
<Exact repo, base/head/PR and overall assessment>

### 2. Requirement Compliance
| Criterion | Result | Evidence |
| --- | --- | --- |
| AC-1: <requirement> | Pass / Fail / Unverified / Not Implemented | <source, check, or limitation> |

### 3. Findings
#### Medium — <Short title>
- **Importance:** Medium — <reason based on impact and scope>
- **Issue:** <specific problem>
- **Evidence:** <path:line or diff; observable fact>
- **Impact:** <consequence>
- **Recommended Fix:** <minimal correction>

### 4. Test & Validation Review
- <Executed checks, results, uncovered cases and constraints>

### 5. Final Verdict
**Approved / Changes Requested / Blocked / Unable to Verify** — <rationale>
```

Omit the example finding when none exists. In the verdict, use the exact combined label `Blocked / Unable to Verify`, not two separate verdict options.

## Final quality check

- The Jira issue and exact implementation target were resolved, not guessed.
- Changes and surrounding code were independently inspected; assertions have evidence.
- Every acceptance criterion is mapped, including gaps.
- Security, integrity, failure handling, backward compatibility and scope were assessed when relevant.
- Test evidence is accurately attributed; no test or threshold is claimed without measurement.
- Findings are specific, deduplicated, actionable, and ordered High, Medium, Low; each importance rating has an evidence-based justification.
- Conflicting KB/Jira/source claims and unknowns are disclosed.
- Review was read-only; no approval was silently turned into a repository/Jira action.
