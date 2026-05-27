---
inclusion: manual
---

# Skill: Pre-Approve Check – RADIO Report

## Purpose
Verify RADIO report before presenting to Human for review. Ensure report is based on actual data, not subjective.

## Input
RADIO report content + tasks.md status + test results.

## Automated Checklist

### Format
- [ ] Has all 5 items: R, A, D, I, O
- [ ] Each item is not empty

### Accuracy (cross-check with data)
- [ ] R (Review): status matches tasks.md status (X/Y tasks done)
- [ ] A (Action): in-progress task matches tasks.md
- [ ] O (Outcome): metrics match CI/test results (coverage %, tests pass)
- [ ] No claims without evidence (e.g., "100% pass" but CI hasn't run)

### Completeness
- [ ] D (Difficulty): if tests fail → must mention
- [ ] D (Difficulty): if RISK/ISSUE Open → must mention
- [ ] D (Difficulty): if QA Open > 3 days → must mention
- [ ] I (Information): context affecting progress is noted
- [ ] O (Outcome): has at least 1 specific metric (number, %)

### Risk/Issue Integration
- [ ] Each serious D item → has corresponding RISK-XXX or ISSUE-XXX
- [ ] If new D item without risk/issue → flag needs creation

## Output format

```markdown
# Pre-Approve Check: RADIO Report

**Check date:** YYYY-MM-DD
**Result:** X/Y items PASS

## Data Verification

| RADIO item | Claim | Evidence | Match? |
|-----------|-------|----------|--------|
| R: 8/10 tasks done | tasks.md | 8 [x] / 10 total | ✅ |
| O: coverage 87% | CI report | 87.2% | ✅ |
| D: "no blockers" | risk-issue/ | ISSUE-002 Open | ❌ MISMATCH |

## Issues

1. **D item incomplete:** ISSUE-002 is Open but RADIO says "no blockers"

## Recommendation

- [ ] Fix D item to reflect ISSUE-002
- [ ] After PASS → present to Human for review
```
