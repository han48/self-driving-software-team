---
inclusion: manual
---

# Skill: Pre-Approve Check – RADIO Report

## Mục đích
Verify RADIO report trước khi đưa Human review. Đảm bảo report dựa trên data thực, không chủ quan.

## Input
RADIO report content + tasks.md status + test results.

## Checklist tự động

### Format
- [ ] Có đủ 5 items: R, A, D, I, O
- [ ] Mỗi item không rỗng

### Accuracy (cross-check với data)
- [ ] R (Review): trạng thái khớp với tasks.md status (X/Y tasks done)
- [ ] A (Action): task đang in-progress khớp với tasks.md
- [ ] O (Outcome): metrics khớp với CI/test results (coverage %, tests pass)
- [ ] Không có claim không có evidence (ví dụ: "100% pass" nhưng CI chưa chạy)

### Completeness
- [ ] D (Difficulty): nếu có test fail → phải mention
- [ ] D (Difficulty): nếu có RISK/ISSUE Open → phải mention
- [ ] D (Difficulty): nếu có QA Open > 3 ngày → phải mention
- [ ] I (Information): context ảnh hưởng tiến độ được ghi nhận
- [ ] O (Outcome): có ít nhất 1 metric cụ thể (số, %)

### Risk/Issue Integration
- [ ] Mỗi D item nghiêm trọng → có RISK-XXX hoặc ISSUE-XXX tương ứng
- [ ] Nếu có D item mới chưa có risk/issue → flag cần tạo

## Output format

```markdown
# Pre-Approve Check: RADIO Report

**Ngày check:** YYYY-MM-DD
**Kết quả:** X/Y items PASS

## Data Verification

| RADIO item | Claim | Evidence | Match? |
|-----------|-------|----------|--------|
| R: 8/10 tasks done | tasks.md | 8 [x] / 10 total | ✅ |
| O: coverage 87% | CI report | 87.2% | ✅ |
| D: "no blockers" | risk-issue/ | ISSUE-002 Open | ❌ MISMATCH |

## Issues

1. **D item incomplete:** ISSUE-002 đang Open nhưng RADIO ghi "no blockers"

## Recommendation

- [ ] Sửa D item để reflect ISSUE-002
- [ ] Sau khi PASS → đưa Human review
```
