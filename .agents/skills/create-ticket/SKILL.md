---
name: create-ticket
description: Draft, refine, or prepare an update to a Tutor Marketplace Jira Story or Task, including scope, confirmed constraints, and acceptance criteria. Use for ticket requirements; use plan-ticket for solution design of an existing issue.
---

# Create Ticket

## Goal

Turn a user request into a clear Jira work item that describes the problem, desired outcome, confirmed constraints, and success conditions without silently choosing an implementation solution.

The ticket must be understandable by a human reader who has project context but has not participated in the conversation that created the ticket.

The user's request provides the ticket-specific requirement. This skill provides the standard structure, wording rules, context gathering, validation, and approval workflow.

## Invocation

Use this skill when the user explicitly invokes `$create-ticket` or when the request clearly asks to create, draft, prepare, propose, write, or refine a Jira ticket/work item for Tutor Marketplace.

Examples:

`$create-ticket Database Design & Initial Setup`

`$create-ticket Add tutor authentication`

The text after `$create-ticket` is the ticket-specific request. Any following text is additional requirement/context and must be interpreted under the same rules in this skill.

The user does not need to restate the ticket template or workflow. This skill owns the standard ticket structure and creation workflow.

Explicit invocation of `$create-ticket` selects this workflow, but it is not approval to perform the Jira write. The approval gate below still applies.

The operation may be drafting a new ticket, refining a session draft, or updating an existing Jira issue. Establish which operation is intended; a session draft is not a persisted issue. For updates, resolve and read the target issue before proposing its replacement content. If the target cannot be found, continue drafting and explain the missing target; do not silently create a replacement.

## Core writing principle

A ticket describes **what is needed and what success looks like**, not **how to implement it**.

Do not put implementation solutions into the ticket unless the user explicitly states that a particular constraint is itself a requirement.

Avoid solution details such as:
- class, method, package, or file names to create
- database table or column designs that have not already been confirmed requirements
- SQL statements
- algorithms
- code snippets
- endpoint implementation details
- framework-specific implementation steps
- step-by-step coding instructions

If implementation ideas are discovered while researching the ticket, do not silently include them in the Jira ticket. They belong in later planning/implementation work unless the user explicitly asks for them to be part of the requirement.

## Scope and decision handling

- Establish the delivery boundary from the user's request, including relevant environments and affected layers. A confirmed KB capability is not automatically in scope for this ticket.
- Distinguish explicit user decisions, verified current behavior, proposals requiring review, and material unknowns. User decisions may change project direction; identify the change without presenting it as existing implementation or a KB update.
- Treat follow-up messages as revisions to the same draft. Preserve unchanged requirements, apply explicit scope changes, and remove superseded questions. Do not infer approval from the user supplying a decision.
- For technical or cross-layer tickets, inspect relevant integration boundaries and check material gaps in contracts, state lifecycle, error recovery, concurrency, environment restrictions, and validation. Skip concerns that do not apply.
- Ask about related material decisions together, with recommendations when defensible. Continue drafting known requirements rather than turning every implementation choice into a blocker.
- During clarification, summarize the changes instead of reproducing the entire ticket and requesting approval on every turn. Present a consolidated version when it is ready for review or the user requests it.
- Keep ticket-specific values and domain decisions in the ticket or KB, not in this reusable skill.

## Language style

Write for humans, not for agents.

Use:
- plain, natural English
- short and direct sentences
- concrete wording
- product/domain terminology already used by the project
- observable outcomes

Avoid:
- unnecessary technical jargon
- vague phrases such as "handle properly", "support as needed", or "make it robust"
- agent/process language such as "the agent should inspect..."
- implementation instructions
- speculative requirements
- excessive background information

The ticket should be concise enough to scan quickly but complete enough to understand without the original chat.

## Jira work type

Choose the work type from the nature of the requested outcome:

- **Story** — a user-facing behavior, capability, or product outcome.
- **Task** — technical, internal, infrastructure, setup, maintenance, research, or engineering work that is not directly a user-facing capability.

Do not invent another issue type if the Jira project does not provide it.

## Ticket structure

Use the compact format below for ordinary requirement tickets. For technical or cross-layer tickets, use the extended format when confirmed constraints and delivery boundaries warrant it. Follow an explicit user-supplied format in either case.

Title and issue type are Jira metadata. Show them above the draft for review; do not repeat them inside the saved description unless the user requests that format.

### 1. Title

A short statement of the outcome or work to be completed.

Rules:
- Prefer one clear phrase or sentence.
- Do not include implementation instructions.
- Do not prefix the title with `Story:`, `Task:`, repository names, or ticket numbers unless required by the project.

Example style:

`Set Up Initial Database Structure`

### 2. Issue Type

State the selected Jira issue type:

`Story` or `Task`

This is Jira metadata and does not need to be duplicated inside the Jira description when the issue is created.

### 3. Description

Explain the requested outcome and relevant context in natural language.

The description should answer, when the information is known:
- What is needed?
- Why is it needed?
- What area of the product or project does it affect?

Keep it focused on the requirement. Do not describe the implementation approach.

A good description is normally one to three short paragraphs.

### 4. Acceptance Criteria

Define the observable conditions that must be true for the ticket to be considered complete.

Format each criterion as a checkbox:

`- [ ] ...`

Rules:
- Each criterion must be independently verifiable.
- State an observable outcome with its relevant trigger or condition. Split bundled assertions when they can pass or fail independently.
- Describe outcomes, not implementation steps.
- Include only criteria supported by the user's request or confirmed project knowledge.
- Do not turn assumptions, Planned items, or TBD decisions into acceptance criteria.
- Do not add generic criteria such as "code is clean", "best practices are followed", or "works correctly" unless the project explicitly requires them.

Prefer a small set of meaningful criteria over a long checklist.

For larger tickets, group criteria by behavior or responsibility. Include meaningful failure, boundary, security, or concurrency scenarios only when supported by the scope. Put required testing outcomes in a Testing group when useful; do not duplicate every behavior as a separate test checklist. No fixed criterion count or mandatory group list applies.

### 5. Out of Scope

Include this section only when boundaries are known and stating them helps prevent scope confusion.

Use it to clarify related work that this ticket intentionally does not include.

Do not invent exclusions merely to populate the section.

### 6. Open Questions

Include this section only when unresolved information materially affects the requirement or acceptance criteria.

Use it for genuine TBD decisions that must not be silently assumed.

Rules:
- Phrase each item as a clear question.
- Do not answer the question by guessing.
- Do not block drafting the rest of the ticket when the known requirement can still be described accurately.

If there are no relevant open questions, omit the section.

### Extended format sections

Use these conditional sections to make a technical ticket reviewable without turning it into a solution-design document:

- **Scope:** capabilities, affected layers, and environment boundaries included in delivery. State exclusions under Out of Scope rather than duplicating them here.
- **Technical Decisions:** user-specified or already confirmed constraints needed to implement the requirement. Distinguish verified existing behavior from new decisions. Clearly label any requested design proposals as requiring review; do not turn them into confirmed acceptance criteria before approval.
- **Dependencies:** actual prerequisite tickets, existing foundations, external systems, or configuration. Distinguish completed foundations from unresolved blockers. Do not invent dependencies to fill the section.

Keep Description focused on outcome and purpose, Technical Decisions on constraints, and Acceptance Criteria on observable completion. Avoid copying the same paragraph into all three.

## Sections that should NOT be added by default

Do not add these sections unless the user explicitly asks for solution-design content or the project later establishes them as required:

- Technical Notes
- Implementation Plan
- Proposed Solution
- Database Design
- API Design
- Files to Change
- Code Changes
- Testing Implementation

These belong to planning or implementation workflows rather than requirement definition.

## Context gathering

Before drafting the ticket:

1. Read the user's request, including any additional requirements they provide.
2. Read relevant confirmed knowledge from `tutor_kb` through GitHub MCP.
3. Inspect Jira through Atlassian MCP for relevant existing work when duplication, dependency, or current workflow state matters.
4. Inspect `tutor_backend` or `tutor_frontend` through GitHub MCP only when current implementation state materially affects how the requirement should be described.
5. Inspect Supabase only when the current database state materially affects the requirement.
6. Use OpenAI Developer Docs only when the ticket concerns OpenAI/Codex functionality and current official behavior matters.

Do not gather unrelated context merely because a connection is available.

For an existing-issue update, read its current description, criteria, and relevant comments before editing. Preserve scope unless the user explicitly changes it. For cross-layer behavior, inspect the relevant frontend and backend source rather than relying only on README summaries. If an authoritative source is inaccessible, disclose the gap; label any local fallback and do not claim remote freshness.

## Knowledge handling

Treat project knowledge literally:

- **Confirmed** — may be used as ticket context or acceptance criteria when relevant.
- **Planned** — may provide context, but must not be rewritten as already implemented or as a confirmed requirement unless the ticket/user request establishes it.
- **TBD** — unresolved; surface it as an Open Question when it materially affects the ticket.

Never invent product behavior, business rules, database schema, API contracts, dependencies, or implementation status to make a ticket look more complete.

## Workflow

1. Identify the intended operation: draft, refine, create, or update; resolve the target issue for updates.
2. Gather only the relevant project context.
3. Check for an obviously duplicate Jira issue when appropriate.
4. Determine whether the work is a Story or Task.
5. Establish scope and separate confirmed requirements, proposals, and material unknowns.
6. Draft or revise using the appropriate compact or extended structure.
7. Review readability and remove unsupported details and duplicated requirements.
8. Verify every acceptance criterion is supported and observable.
9. Present the consolidated ticket and any remaining material questions for review.
10. For a requested write, identify the exact action and target, then obtain explicit approval if it has not already been given for that content and action. Draft-only work ends without a Jira write.
11. Perform only the approved create or update through Atlassian MCP. Approval to update does not authorize creating a missing issue.
12. Read the saved issue and verify its content against the approved draft, allowing harmless formatting normalization. Report any substantive discrepancy; do not claim it was verified if the read-back fails.
13. Return the created or updated issue key, title, and link. Treat it as persisted only after Jira confirms the write.

If the proposed ticket changes materially after approval, show the revised ticket and obtain approval again before writing.

## Approval rule

Reading, researching, checking for duplicates, and drafting are read-only actions and do not require approval.

Creating, editing, transitioning, commenting on, or deleting a Jira issue is a write action and requires explicit user approval before the action occurs.

A request such as `draft a ticket`, `prepare a ticket`, `use create-ticket`, or an explicit `$create-ticket` invocation is not by itself approval to create the Jira issue.

When the draft is ready, ask a direct question such as:

`Do you approve creating this Jira ticket?`

For updates, name the issue: `Do you approve replacing the description of TUTOR-123 with this revision?` Do not request the same approval again once the exact content, action, and target are authorized. Creation approval also covers the routine read-back verification, not unrelated Jira changes.

Only an explicit approval permits the Jira write.

## Draft output format

Compact format:

```markdown
## <Title>

**Issue Type:** <Story | Task>

### Description
<Human-readable description>

### Acceptance Criteria
- [ ] <Observable completion condition>
- [ ] <Observable completion condition>

### Out of Scope
- <Known boundary>

### Open Questions
- <Unresolved question?>
```

`Out of Scope` and `Open Questions` are conditional sections and must be omitted when they add no useful information.

Extended format, with the same title and issue-type metadata above it:

```markdown
### Description
<Outcome, purpose, and relevant context>

### Scope
<Included capabilities, layers, and environments>

### Technical Decisions
<Confirmed constraints; identify requested proposals separately>

### Acceptance Criteria
#### <Relevant behavior or responsibility>
- [ ] <Observable completion condition>

#### Testing
- [ ] <Required verification outcome, when applicable>

### Out of Scope
<Known delivery exclusions>

### Dependencies
<Actual prerequisites and their status>

### Open Questions
<Material unresolved decisions>
```

Omit conditional sections and groups that add no useful information. Do not add placeholder headings merely to complete a template.

## Final quality check

Before presenting a draft, verify:

- A human can understand the ticket without reading the original conversation.
- The title describes the work or outcome clearly.
- The description explains what is needed and why when known.
- Acceptance criteria define verifiable outcomes.
- Technical details are explicit requirements, verified facts, or clearly marked proposals; no solution has silently become a requirement.
- No unsupported requirement has been invented.
- Planned/TBD knowledge has not been presented as Confirmed.
- The current scope and environment boundaries reflect all user revisions.
- Settled questions and superseded requirements have been removed.
- The action is clearly identified as drafting, creating, or updating an actual issue.
- Final approved content uses final wording rather than stale review instructions; no unapproved proposal is relabeled as confirmed.
- Optional sections are included only when useful.
- The ticket is concise and readable.
