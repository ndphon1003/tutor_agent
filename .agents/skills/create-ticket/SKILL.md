---
name: create-ticket
description: Draft and, after explicit user approval, create a clear Jira work item for Tutor Marketplace using a consistent human-readable format and confirmed project knowledge.
---

# Create Ticket

## Goal

Turn a user request into a clear Jira work item that describes the problem or desired outcome without prescribing the implementation solution.

The ticket must be understandable by a human reader who has project context but has not participated in the conversation that created the ticket.

The user's request provides the ticket-specific requirement. This skill provides the standard structure, wording rules, context gathering, validation, and approval workflow.

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

## Required ticket structure

Every generated ticket must contain these fields/sections in this order.

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
- Describe outcomes, not implementation steps.
- Include only criteria supported by the user's request or confirmed project knowledge.
- Do not turn assumptions, Planned items, or TBD decisions into acceptance criteria.
- Do not add generic criteria such as "code is clean", "best practices are followed", or "works correctly" unless the project explicitly requires them.

Prefer a small set of meaningful criteria over a long checklist.

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

## Sections that should NOT be added by default

Do not add these sections to a normal ticket unless the user explicitly asks for them or the project later establishes them as required:

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

## Knowledge handling

Treat project knowledge literally:

- **Confirmed** — may be used as ticket context or acceptance criteria when relevant.
- **Planned** — may provide context, but must not be rewritten as already implemented or as a confirmed requirement unless the ticket/user request establishes it.
- **TBD** — unresolved; surface it as an Open Question when it materially affects the ticket.

Never invent product behavior, business rules, database schema, API contracts, dependencies, or implementation status to make a ticket look more complete.

## Workflow

1. Receive the user's requested ticket and any additional requirements.
2. Gather only the relevant project context.
3. Check for an obviously duplicate Jira issue when appropriate.
4. Determine whether the work is a Story or Task.
5. Separate confirmed requirements from implementation ideas and unresolved decisions.
6. Draft the ticket using the standard ticket structure in this skill.
7. Review the draft for human readability and remove solution/implementation details.
8. Verify every acceptance criterion is supported and observable.
9. Present the complete proposed ticket to the user.
10. Clearly show any Open Questions that remain.
11. Ask the user for explicit approval to create the Jira issue.
12. Only after explicit approval, create the issue through Atlassian MCP.
13. Return the created Jira issue key and title.

If the proposed ticket changes materially after approval, show the revised ticket and obtain approval again before writing.

## Approval rule

Reading, researching, checking for duplicates, and drafting are read-only actions and do not require approval.

Creating, editing, transitioning, commenting on, or deleting a Jira issue is a write action and requires explicit user approval before the action occurs.

A request such as `draft a ticket`, `prepare a ticket`, or `use create-ticket` is not by itself approval to create the Jira issue.

When the draft is ready, ask a direct question such as:

`Do you approve creating this Jira ticket?`

Only an explicit approval permits the Jira write.

## Draft output format

Present the draft to the user in this shape:

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

## Final quality check

Before presenting a draft, verify:

- A human can understand the ticket without reading the original conversation.
- The title describes the work or outcome clearly.
- The description explains what is needed and why when known.
- Acceptance criteria define verifiable outcomes.
- No implementation solution has leaked into the requirement.
- No unsupported requirement has been invented.
- Planned/TBD knowledge has not been presented as Confirmed.
- Optional sections are included only when useful.
- The ticket is concise and readable.
