---
name: create-ticket
description: Draft and, after explicit user approval, create a Jira work item for Tutor Marketplace from confirmed project knowledge.
---

# Create Ticket

## Goal

Turn a requirement or technical work item into a clear Jira issue without inventing requirements, while requiring explicit user approval before Jira is modified.

## Workflow

1. Read the user's request.
2. Read relevant `tutor_kb` documentation.
3. Inspect relevant source repositories when implementation state affects the ticket.
4. Identify unresolved/TBD decisions.
5. Select the appropriate Jira work type:
   - Story for user-facing behavior/capability.
   - Task for technical/internal work.
6. Draft a concise title and description.
7. Add acceptance criteria only when supported by the request or confirmed knowledge.
8. Present the complete proposed ticket and unresolved decisions to the user.
9. Ask explicitly whether the user approves creating the ticket.
10. Only after explicit approval, create the Jira issue.
11. If the proposed ticket changes materially after approval, present the revised ticket and ask for approval again before writing.

## Approval rule

Drafting and inspecting are read-only and do not require approval. Creating, editing, transitioning, commenting on, or deleting a Jira issue is a write action and requires explicit user approval immediately before the write. Do not treat a request to draft or plan a ticket as approval to create it.

## Output quality

A ticket should explain what outcome is required, why it is needed when known, what is in scope, and how completion can be verified without fabricating product behavior.
