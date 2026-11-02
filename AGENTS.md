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

## Safety against invented requirements

Never invent project requirements, database schema, API contracts, business rules, or implementation status.

Treat KB status literally:
- Confirmed = may be relied upon.
- Planned = intended but not implemented/confirmed as current state.
- TBD = unresolved.

If required information is missing, identify it as TBD or request a decision rather than silently choosing a business rule.
