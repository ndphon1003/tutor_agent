---
name: create-ticket
description: Create a Jira work item for Tutor Marketplace from an approved user request and confirmed project knowledge.
---

# Create Ticket

## Goal

Turn an approved requirement or technical work item into a clear Jira issue without inventing requirements.

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
8. Present unresolved decisions explicitly.
9. Create the Jira issue only when issue creation is requested/approved.

## Output quality

A ticket should explain what outcome is required, why it is needed when known, what is in scope, and how completion can be verified without fabricating product behavior.
