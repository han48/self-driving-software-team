---
inclusion: manual
---

# Skill: Pre-Approve Check – Pull Request (Code)

## Mục đích
Chạy automated checklist verification cho code changes trước khi đưa Human approve PR.

## Input
Code changes (diff) + `requirements.md` + `design.md` + `tasks.md` liên quan.

## Checklist tự động

### Spec Compliance
- [ ] Code implement đúng theo acceptance criteria trong spec
- [ ] Không có requirement bị bỏ sót trong implementation
- [ ] API endpoints match với design (route, method, input/output)
- [ ] Data models match với database design (columns, types, relationships)

### Test Results
- [ ] Tất cả unit tests PASS
- [ ] Tất cả integration tests PASS
- [ ] E2E tests PASS (nếu có)
- [ ] Test coverage ≥ 80%
- [ ] Property-based tests PASS (nếu có)

### Code Quality
- [ ] Không có hardcoded secrets (API keys, passwords, tokens)
- [ ] Không có TODO/FIXME/HACK comments chưa resolve
- [ ] Không có dead code (unused imports, unreachable code)
- [ ] Không có console.log/print debug statements
- [ ] Error handling: không swallow exceptions (empty catch blocks)
- [ ] Logging: có đủ log cho debugging, không log sensitive data

### Security (basic)
- [ ] Input validation trên tất cả user inputs
- [ ] Parameterized queries (không string concatenation cho SQL)
- [ ] Không có XSS vulnerabilities (output encoding)
- [ ] Auth checks trên protected endpoints
- [ ] Sensitive data không xuất hiện trong response/logs

### Design Compliance
- [ ] Code follow architecture pattern trong design.md
- [ ] Component responsibilities đúng (không có god class/function)
- [ ] Naming conventions nhất quán

## Output format

```markdown
# Pre-Approve Check: Pull Request

**PR:** [title/branch]
**Ngày check:** YYYY-MM-DD
**Kết quả:** X/Y items PASS

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

## Issues cần sửa trước khi approve

1. **Requirement 1.3** chưa implement
2. **src/utils.ts:42** – empty catch block

## Recommendation

- [ ] Sửa issues → chạy lại pre-approve check
- [ ] Sau khi PASS hết → đưa Human approve PR
```
