# Tutor Marketplace Agent Instructions

## Purpose

This repository coordinates agent-assisted development for Tutor Marketplace.

## Sources of truth

- `tutor_kb`: confirmed architecture, domain, API, database, product, and engineering knowledge.
- Jira: work items and their current workflow state.
- `tutor_backend`: backend implementation.
- `tutor_frontend`: frontend implementation.
- This repository: agent responsibilities, rules, and reusable skills.

## Core model

- Agent = WHO performs or owns a type of work.
- Rule = MUST / MUST NOT constraints.
- Skill = HOW a repeatable workflow is performed.

## Routing

Start with `agents/orchestrator.md` unless the user explicitly requests a specialist.

Specialists:
- Backend work → `agents/backend.md`
- Frontend work → `agents/frontend.md`
- Database work → `agents/database.md`
- Review work → `agents/reviewer.md`

Before acting, read the relevant files under `rules/`.

Use a skill under `.agents/skills/` when the requested workflow matches its description.

## Write-action approval gate

Read-only inspection, analysis, planning, and drafting may proceed without approval.

Before any write action, the agent MUST:
1. Explain what it intends to change.
2. Show or summarize the proposed change at an appropriate level of detail.
3. Ask the user for explicit approval.
4. Perform the write only after that approval.

Write actions include creating, updating, deleting, transitioning, committing, pushing, merging, commenting on, or otherwise mutating Jira, GitHub repositories, the KB, application code, database state, configuration, or other project/external state.

Approval is scoped to the specific proposed action or clearly stated batch. If scope materially changes, request approval again.

## Safety against invented requirements

Never invent project requirements, database schema, API contracts, business rules, or implementation status.

Treat KB status literally:
- Confirmed = may be relied upon.
- Planned = intended but not implemented/confirmed as current state.
- TBD = unresolved.

If required information is missing, identify it as TBD or request a decision rather than silently choosing a business rule.
