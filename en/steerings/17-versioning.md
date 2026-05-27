---
inclusion: fileMatch
fileMatchPattern: '**/requirements.md,**/design.md,**/bugfix.md'
---

# Steering: Spec Versioning Strategy

## When to Apply
When creating or updating spec files (requirements.md, design.md, bugfix.md).

## Version Rules

| Change type | Version | Re-approve needed? | Example |
|-------------|---------|-------------------|---------|
| **Major** (scope/logic) | v1.0 → v2.0 | ✅ Mandatory | Add requirement, change architecture |
| **Minor** (addition) | v1.0 → v1.1 | ✅ Mandatory | Add edge case, clarify AC |
| **Patch** (typo) | v1.0 → v1.0.1 | ❌ Not needed | Fix typo, formatting |

## Required Header in Every Spec File

```markdown
**Version:** X.Y  
**Last updated:** YYYY-MM-DD  
**Updated by:** [AI Agent / Human name]  
**Approved by:** [Role] – YYYY-MM-DD  
```

## Mandatory Change Log

```markdown
## Change Log

| Version | Date | Change | Approved by | Trigger |
|---------|------|--------|-------------|---------|
| 1.2 | 2025-03-20 | Add rate limiting AC | Tech Lead | CR-005 |
| 1.1 | 2025-03-15 | Clarify lock duration | Tech Lead | QA-003 |
| 1.0 | 2025-03-10 | Initial version | Tech Lead | — |
```

## Rules

1. **Every spec file has a version** in header metadata
2. **Change log is mandatory** for every change
3. **Git tag for milestones:** `spec/{feature}-v{X.Y}` when approving major
4. **Do not modify approved spec** without re-approval (except patch)
5. **Q&A → minor version:** When conclusion leads to spec change
6. **CR → major version:** Large CR creates new major version
7. **Traceability:** Each change references its trigger (QA-XXX, CR-XXX, RISK-XXX)
8. **Do not delete history:** Append to change log, do not remove old entries
