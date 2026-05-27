---
inclusion: manual
---

# Skill: Pre-Approve Check – Risk

## Purpose
Verify risk entry before presenting to Human for confirmation.

## Input
Risk entry in `openspec/risk-issue/risks.md`.

## Automated Checklist

### Format
- [ ] Has heading `### RISK-XXX: [Name]`
- [ ] Has `**Date detected:**` (format YYYY-MM-DD)
- [ ] Has `**Probability:**` (High/Medium/Low)
- [ ] Has `**Impact:**` (High/Medium/Low)
- [ ] Has `**Severity:**` (Critical/High/Medium/Low)
- [ ] Has `**Description:**` (not empty)
- [ ] Has `**Prevention action:**` (specific, actionable)
- [ ] Has `**Mitigation action:**` (specific, actionable)
- [ ] Has `**Owner:**`
- [ ] Has `**Trigger:**`
- [ ] Has `**Status:**`

### Quality
- [ ] Prevention ≠ Mitigation (2 different actions)
- [ ] Prevention reduces probability of occurrence (not consequences)
- [ ] Mitigation reduces consequences if it occurs (not probability)
- [ ] Trigger is an observable sign (not "when it happens")
- [ ] Severity = Probability × Impact (logic correct)
- [ ] Not duplicate of existing RISK

### Traceability
- [ ] If related to spec → has reference
- [ ] If related to CR → has reference

## Output format

```markdown
# Pre-Approve Check: Risk

**Risk:** RISK-XXX
**Result:** X/Y items PASS

## Validation

| Check | Status | Note |
|-------|--------|------|
| Prevention actionable | ✅ | "Add circuit breaker" – specific |
| Mitigation actionable | ✅ | "Fallback to cache" – specific |
| Prevention ≠ Mitigation | ✅ | Different |
| Severity logic | ❌ | High × Low = Medium, but marked Critical |

## Issues

1. **Severity:** High × Low should be Medium, not Critical

## Recommendation

- [ ] Fix severity → present to Human for confirmation
```
