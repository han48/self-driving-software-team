---
inclusion: manual
---

# Skill: Pre-Approve Check – Tasks

## Mục đích
Chạy automated checklist verification cho tasks.md trước khi đưa Human approve.

## Input
File `tasks.md` + `design.md` (đã approve) cần kiểm tra.

## Checklist tự động

### Format & Structure
- [ ] Heading là `# Implementation Plan: [Tên]`
- [ ] Có section `## Overview`
- [ ] Có section `## Tasks` với nested checkboxes
- [ ] Tất cả tasks dùng format `- [ ] N.` hoặc `- [ ] N.M`
- [ ] Có `## Task Dependency Graph` với JSON waves format
- [ ] JSON waves syntax hợp lệ (parseable)

### Granularity & Quality
- [ ] Mỗi sub-task có mô tả đủ rõ để AI thực thi (không quá chung chung)
- [ ] Không có task quá lớn (estimate > 4h nếu có estimate)
- [ ] Mỗi sub-task kết thúc bằng `_Requirements: X.X_`

### Design Coverage
- [ ] Tất cả components trong design.md có task tương ứng
- [ ] Tất cả data models có task migration/model
- [ ] Có tasks cho testing (unit, integration, E2E)
- [ ] Có tasks cho property-based tests (nếu design có Correctness Properties)

### Dependencies
- [ ] Task dependency graph cover tất cả task numbers
- [ ] Không có circular dependency
- [ ] Tasks trong wave sau phụ thuộc đúng vào tasks wave trước
- [ ] Không có task "mồ côi" (không nằm trong wave nào)

### Traceability
- [ ] Mỗi sub-task reference ít nhất 1 requirement number
- [ ] Tất cả requirements được reference bởi ít nhất 1 task
- [ ] Property test tasks có format: `**Property N: [Tên]**` + `**Validates: Requirements X.X**`

## Output format

```markdown
# Pre-Approve Check: Tasks

**File:** [path]
**Ngày check:** YYYY-MM-DD
**Kết quả:** X/Y items PASS

## Requirements Traceability

| Requirement | Tasks covering | Status |
|-------------|---------------|--------|
| Req 1.1 | Task 3.1, 4.2 | ✅ |
| Req 2.3 | ❌ No task | ❌ |

## Dependency Graph Validation

- Waves: 4
- Total tasks: 12
- Orphan tasks: 0
- Circular deps: None ✅

## Issues cần sửa trước khi approve

1. **Requirement 2.3** chưa có task nào cover
2. **Task 5** không nằm trong dependency graph

## Recommendation

- [ ] Sửa issues → chạy lại pre-approve check
- [ ] Sau khi PASS hết → đưa Human approve
```
