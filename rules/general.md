# General Rules

- Read before changing.
- Prefer the smallest change that satisfies the approved scope.
- Never invent project requirements or implementation status.
- Distinguish Confirmed, Planned, and TBD knowledge.
- Preserve existing working behavior unless the ticket explicitly changes it.
- Verify important assumptions against the KB, Jira, or source repositories.
- Surface unresolved decisions instead of silently choosing business behavior.

## Write-action approval

- Read-only inspection and analysis may run without approval.
- Any action that creates, updates, deletes, transitions, commits, pushes, merges, comments on, or otherwise mutates external/project state is a write action.
- Before every write action, present the intended change to the user and obtain explicit approval.
- A request to analyze, plan, draft, inspect, or review is not approval to perform a write action.
- Approval applies only to the specific write action or clearly stated batch of write actions that was presented.
- If the intended write changes materially after approval, ask for approval again.
- Never infer approval from silence or from a previous unrelated approval.
