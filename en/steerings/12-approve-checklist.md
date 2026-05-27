---
inclusion: manual
---

# Steering: Approve Checklist (Quick Reference)

## When to Apply
Every time Human needs to approve any AI output. AI must remind Human to use the appropriate checklist.

## Mapping: Output → Checklist

| Output to approve | Applicable checklist |
|-------------------|---------------------|
| requirements.md | Checklist Approve Requirements |
| design.md | Checklist Approve Design |
| tasks.md | Checklist Approve Tasks |
| Pull Request (code) | Checklist Approve PR |
| RADIO report | Checklist Approve RADIO |
| CR (accept/reject) | Checklist Approve CR |
| Bugfix (bugfix.md + design.md) | Checklist Approve Bugfix |
| Risk (RISK-XXX) | Checklist Approve Risk |
| Issue resolution | Checklist Approve Issue Resolution |
| Q&A conclusion | Checklist Approve Q&A Conclusion |
| deliverables.md | Checklist Approve Deliverables & Limitations |

## Rules

1. **AI reminds Human:** When presenting output for Human approval, AI must clearly state: "Please review using the [name] Checklist before approving."
2. **Tick all before approve:** Reviewer must pass all items in the checklist
3. **If not passed:** Request changes, AI fixes, resubmit
4. **Record:** Who approved, what date, which role: "Approved by: [Name] (as [Role]) – YYYY-MM-DD"
5. **Rotate:** Same person must not approve > 3 consecutive PRs for the same feature

## Approve Reminder Template

When AI presents output to Human, use this format:

```
✅ [Output type] is ready for review.
📋 Applicable checklist: [Checklist name]
👤 Suggested reviewer: [Appropriate role]
```
