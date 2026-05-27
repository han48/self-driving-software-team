---
inclusion: manual
---

# Skill: Pre-Approve Check – Requirements (Spec)

## Mục đích
Chạy automated checklist verification cho requirements.md trước khi đưa Human approve. Giảm thời gian review cho Human bằng cách AI tự phát hiện vấn đề trước.

## Input
File `requirements.md` cần kiểm tra.

## Checklist tự động

Đọc file requirements.md và kiểm tra từng item sau. Output kết quả PASS/FAIL cho mỗi item:

### Format & Structure
- [ ] Heading là `# Requirements Document`
- [ ] Có section `## Introduction` (không rỗng)
- [ ] Có section `## Glossary` (ít nhất 1 term)
- [ ] Có separator `---` trước `## Requirements`
- [ ] Có ít nhất 1 `### Requirement N:` section
- [ ] Mỗi requirement có `**User Story:**` với format "As a... I want... so that..."
- [ ] Mỗi requirement có `#### Acceptance Criteria` với ít nhất 1 item

### EARS Notation
- [ ] Tất cả AC dùng pattern: THE/WHEN/IF + SHALL
- [ ] Component names viết HOA (THE API, THE System, THE Admin_Panel...)
- [ ] Mỗi AC được đánh số (1, 2, 3...)
- [ ] Không có AC dùng ngôn ngữ mơ hồ ("nên", "có thể", "tốt hơn")

### Logic & Completeness
- [ ] Không có 2 AC mâu thuẫn nhau (cùng condition → khác behavior)
- [ ] Có error cases / edge cases (không chỉ happy path)
- [ ] Glossary cover tất cả thuật ngữ chuyên ngành xuất hiện trong AC
- [ ] Không có assumption ẩn (mọi giả định ghi rõ trong AC hoặc Introduction)

### Testability
- [ ] Mỗi AC có thể chuyển thành test case cụ thể (có input + expected output rõ ràng)
- [ ] Không có AC quá chung chung ("hệ thống phải nhanh", "UI phải đẹp")

### Traceability
- [ ] Nếu có Risk liên quan → đã reference RISK-XXX
- [ ] Nếu có Q&A liên quan → đã reference QA-XXX hoặc Q&A đã Answered

## Output format

```markdown
# Pre-Approve Check: Requirements

**File:** [path]
**Ngày check:** YYYY-MM-DD
**Kết quả:** X/Y items PASS

## Results

| # | Item | Status | Ghi chú |
|---|------|--------|---------|
| 1 | Heading đúng format | ✅ PASS | |
| 2 | Introduction không rỗng | ✅ PASS | |
| 3 | EARS notation đúng | ❌ FAIL | AC 3.2 thiếu SHALL |
| ... | ... | ... | ... |

## Issues cần sửa trước khi approve

1. **AC 3.2:** Thiếu keyword SHALL – hiện tại viết "hệ thống trả về lỗi" → sửa thành "THE System SHALL trả về lỗi"
2. **Glossary:** Thuật ngữ "DashboardMessage" xuất hiện trong AC nhưng chưa có trong Glossary

## Recommendation

- [ ] Sửa 2 issues trên → chạy lại pre-approve check
- [ ] Sau khi PASS hết → đưa Human approve
```
