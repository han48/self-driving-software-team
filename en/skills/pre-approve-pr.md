---
inclusion: manual
---

# Skill: Pre-Approve Check – Pull Request (Code)

## Purpose
Run automated checklist verification for code changes before presenting to Human for PR approval.

## Input
Code changes (diff) + `requirements.md` + `design.md` + `tasks.md` related.

## Automated Checklist

### Spec Compliance
- [ ] Code implements correctly per acceptance criteria in spec
- [ ] No requirement missed in implementation
- [ ] API endpoints match design (route, method, input/output)
- [ ] Data models match database design (columns, types, relationships)

### Test Results
- [ ] All unit tests PASS
- [ ] All integration tests PASS
- [ ] E2E tests PASS (if applicable)
- [ ] Test coverage ≥ 80%
- [ ] Property-based tests PASS (if applicable)

### Code Quality
- [ ] No hardcoded secrets (API keys, passwords, tokens)
- [ ] No unresolved TODO/FIXME/HACK comments
- [ ] No dead code (unused imports, unreachable code)
- [ ] No console.log/print debug statements
- [ ] Error handling: no swallowed exceptions (empty catch blocks)
- [ ] Logging: sufficient for debugging, no sensitive data logged

### Security (basic)
- [ ] Input validation on all user inputs
- [ ] Parameterized queries (no string concatenation for SQL)
- [ ] No XSS vulnerabilities (output encoding)
- [ ] Auth checks on protected endpoints
- [ ] Sensitive data not in response/logs

### Design Compliance
- [ ] Code follows architecture pattern in design.md
- [ ] Component responsibilities correct (no god class/function)
- [ ] Naming conventions consistent

## Output format

```markdown
# Pre-Approve Check: Pull Request

**PR:** [title/branch]
**Check date:** YYYY-MM-DD
**Result:** X/Y items PASS

## Test Summary

| Type | Total | Pass | Fail | Coverage |
|------|-------|------|------|----------|
| Unit | 24 | 24 | 0 | 87% |
| Integration | 8 | 8 | 0 | — |
| E2E | 3 | 3 | 0 | — |
| PBT | 5 | 5 | 0 | — |

## Spec Compliance

| Requirement | Implemented | Tested | Status |
|-------------|------------|--------|--------|
| Req 1.1 | ✅ | ✅ | ✅ |
| Req 1.2 | ✅ | ✅ | ✅ |
| Req 1.3 | ❌ | — | ❌ |

## Security Scan

- Hardcoded secrets: 0 found ✅
- SQL injection risk: 0 found ✅
- XSS risk: 0 found ✅

## Issues to fix before approval

1. **Requirement 1.3** not implemented
2. **src/utils.ts:42** – empty catch block

## Recommendation

- [ ] Fix issues → re-run pre-approve check
- [ ] After all PASS → present to Human for PR approval
```
