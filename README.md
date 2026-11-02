# Tutor Marketplace Agent

Agent orchestration repository for the Tutor Marketplace project.

## Responsibilities

This repository defines:

- **Agents** — who is responsible for a type of work.
- **Rules** — constraints that agents must follow.
- **Skills** — reusable workflows for carrying out work.

Project/domain knowledge belongs in `tutor_kb`. Application code belongs in `tutor_backend` and `tutor_frontend`.

## Initial workflow

User request → Orchestrator → project rules + KB → skill → specialist agent → Reviewer.

The agent system must not turn Planned or TBD knowledge into Confirmed requirements without an explicit decision.
