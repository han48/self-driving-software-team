---
inclusion: manual
---

# Skill: Pre-Approve Check – Bugfix

## Mục đích
Chạy automated checklist verification cho bugfix spec trước khi đưa Human approve.

## Input
Files trong bugfix folder: `bugfix.md` + `design.md` + `tasks.md`

## Checklist tự động

### bugfix.md
- [ ] Heading là `# Bugfix Requirements Document`
- [ ] Có `## Introduction` (không rỗng)
- [ ] Có `## Bug Analysis` với 3 sub-sections
- [ ] `### Current Behavior (Defect)` có ít nhất 1 statement (1.x)
- [ ] `### Expected Behavior (Correct)` có ít nhất 1 statement (2.x)
- [ ] `### Unchanged Behavior (Regression Prevention)` có ít nhất 1 statement (3.x)
- [ ] Defect statements dùng "THEN the system" (chữ thường)
- [ ] Correct statements dùng "THE SYSTEM SHALL" (chữ hoa)
- [ ] Unchanged statements dùng "THE SYSTEM SHALL CONTINUE TO"
- [ ] Đánh số đúng: 1.x, 2.x, 3.x

### design.md
- [ ] Có `## Glossary`
- [ ] Có `## Bug Details` với affected components
- [ ] Có `## Hypothesized Root Cause` (giải thích TẠI SAO, không chỉ mô tả)
- [ ] Có `## Fix Implementation` với code cụ thể
- [ ] Có `## Correctness Properties` (ít nhất 2: valid passes + invalid fails)
- [ ] Có `## Testing Strategy`
- [ ] Có `## Files Changed` (bảng liệt kê)
- [ ] Files Changed chỉ chứa files cần thiết (surgical fix)

### tasks.md
- [ ] Reference dùng `_Bugfix: X.X_` (không phải `_Requirements:_`)
- [ ] Có task reproduce bug (test FAIL trước fix)
- [ ] Có task implement fix
- [ ] Có task validate fix (test PASS sau fix)
- [ ] Có task regression test
- [ ] Có `## Task Dependency Graph`

### Cross-check
- [ ] Unchanged Behavior trong bugfix.md → có regression test trong tasks.md
- [ ] Files Changed trong design.md → tasks chỉ sửa đúng files đó
- [ ] Correctness Properties → có property test tasks tương ứng

## Output format

```markdown
# Pre-Approve Check: Bugfix

**Bug:** [name]
**Ngày check:** YYYY-MM-DD
**Kết quả:** X/Y items PASS

## Surgical Fix Validation

| Check | Status |
|-------|--------|
| Files Changed ≤ 5 files | ✅ |
| Unchanged Behavior listed | ✅ (3 items) |
| Regression tests cover Unchanged | ✅ |
| Fix is minimal | ✅ |

## Issues cần sửa trước khi approve

1. **Unchanged Behavior 3.2** chưa có regression test tương ứng trong tasks.md

## Recommendation

- [ ] Sửa issues → chạy lại pre-approve check
- [ ] Sau khi PASS hết → đưa Human approve
```
