---
inclusion: manual
---

# Skill: Pre-Approve Check – Bugfix

## Purpose
Run automated checklist verification for bugfix spec before presenting to Human for approval.

## Input
Files in bugfix folder: `bugfix.md` + `design.md` + `tasks.md`

## Automated Checklist

### bugfix.md
- [ ] Heading is `# Bugfix Requirements Document`
- [ ] Has `## Introduction` (not empty)
- [ ] Has `## Bug Analysis` with 3 sub-sections
- [ ] `### Current Behavior (Defect)` has at least 1 statement (1.x)
- [ ] `### Expected Behavior (Correct)` has at least 1 statement (2.x)
- [ ] `### Unchanged Behavior (Regression Prevention)` has at least 1 statement (3.x)
- [ ] Defect statements use "THEN the system" (lowercase)
- [ ] Correct statements use "THE SYSTEM SHALL" (uppercase)
- [ ] Unchanged statements use "THE SYSTEM SHALL CONTINUE TO"
- [ ] Numbering correct: 1.x, 2.x, 3.x

### design.md
- [ ] Has `## Glossary`
- [ ] Has `## Bug Details` with affected components
- [ ] Has `## Hypothesized Root Cause` (explains WHY, not just describes)
- [ ] Has `## Fix Implementation` with specific code
- [ ] Has `## Correctness Properties` (at least 2: valid passes + invalid fails)
- [ ] Has `## Testing Strategy`
- [ ] Has `## Files Changed` (table listing)
- [ ] Files Changed only contains necessary files (surgical fix)

### tasks.md
- [ ] References use `_Bugfix: X.X_` (not `_Requirements:_`)
- [ ] Has task to reproduce bug (test FAILS before fix)
- [ ] Has task to implement fix
- [ ] Has task to validate fix (test PASSES after fix)
- [ ] Has task for regression test
- [ ] Has `## Task Dependency Graph`

### Cross-check
- [ ] Unchanged Behavior in bugfix.md → has regression test in tasks.md
- [ ] Files Changed in design.md → tasks only modify those files
- [ ] Correctness Properties → has corresponding property test tasks

## Output format

```markdown
# Pre-Approve Check: Bugfix

**Bug:** [name]
**Check date:** YYYY-MM-DD
**Result:** X/Y items PASS

## Surgical Fix Validation

| Check | Status |
|-------|--------|
| Files Changed ≤ 5 files | ✅ |
| Unchanged Behavior listed | ✅ (3 items) |
| Regression tests cover Unchanged | ✅ |
| Fix is minimal | ✅ |

## Issues to fix before approval

1. **Unchanged Behavior 3.2** has no corresponding regression test in tasks.md

## Recommendation

- [ ] Fix issues → re-run pre-approve check
- [ ] After all PASS → present to Human for approval
```
