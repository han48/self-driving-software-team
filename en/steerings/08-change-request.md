---
inclusion: fileMatch
fileMatchPattern: '**/change-request/**'
---

# Steering: Change Request (CR) Management

## When to Apply
When a new CR arrives or when processing CRs in the backlog.

## Rules

1. **All CRs must go into backlog:** `openspec/change-request/backlog.md`

2. **Mandatory CR entry format:**
   ```
   ### CR-XXX: [Title]
   - **Date received:** YYYY-MM-DD
   - **From:** [Source]
   - **Priority:** High / Medium / Low
   - **Description:** [Brief description]
   ```

3. **Lifecycle:** Pending → Approved → In Progress → Review → Done (or Rejected)

4. **When moving to In Progress:**
   - Create folder `openspec/change-request/CR-XXX/`
   - AI generates requirements.md → Human approve
   - AI generates design.md → Human approve
   - AI generates tasks.md → Human approve
   - AI executes tasks

5. **Mandatory fields by status:**
   - Approved: add `**Approved by:**` + `**Scheduled:**`
   - In Progress: add `**Started:**` + `**Spec:**`
   - Done: add `**Completed:**`
   - Rejected: add `**Rejected by:**` + `**Reason:**`

6. **Small CR (< 1 day):** May skip design, only needs requirements + tasks
7. **Large CR (> 1 day):** Must have full spec (requirements + design + tasks)

8. **No approval = no execution:** AI must not execute a CR that has not been approved
