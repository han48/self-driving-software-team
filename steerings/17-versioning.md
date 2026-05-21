---
inclusion: fileMatch
fileMatchPattern: '**/requirements.md,**/design.md,**/bugfix.md'
---

# Steering: Spec Versioning Strategy

## Khi nào áp dụng
Khi tạo hoặc cập nhật spec files (requirements.md, design.md, bugfix.md).

## Quy tắc Version

| Loại thay đổi | Version | Cần re-approve? | Ví dụ |
|---------------|---------|-----------------|-------|
| **Major** (scope/logic) | v1.0 → v2.0 | ✅ Bắt buộc | Thêm requirement, đổi architecture |
| **Minor** (bổ sung) | v1.0 → v1.1 | ✅ Bắt buộc | Thêm edge case, clarify AC |
| **Patch** (typo) | v1.0 → v1.0.1 | ❌ Không cần | Fix typo, format |

## Header bắt buộc trong mỗi spec file

```markdown
**Version:** X.Y  
**Last updated:** YYYY-MM-DD  
**Updated by:** [AI Agent / Human name]  
**Approved by:** [Role] – YYYY-MM-DD  
```

## Change Log bắt buộc

```markdown
## Change Log

| Version | Ngày | Thay đổi | Approved by | Trigger |
|---------|------|----------|-------------|---------|
| 1.2 | 2025-03-20 | Thêm rate limiting AC | Tech Lead | CR-005 |
| 1.1 | 2025-03-15 | Clarify lock duration | Tech Lead | QA-003 |
| 1.0 | 2025-03-10 | Initial version | Tech Lead | — |
```

## Quy tắc

1. **Mỗi spec file có version** ở header metadata
2. **Change log bắt buộc** cho mỗi thay đổi
3. **Git tag cho milestone:** `spec/{feature}-v{X.Y}` khi approve major
4. **Không sửa spec đã approve** mà không re-approve (trừ patch)
5. **Q&A → minor version:** Khi conclusion dẫn đến spec change
6. **CR → major version:** CR lớn tạo major version mới
7. **Traceability:** Mỗi change reference trigger (QA-XXX, CR-XXX, RISK-XXX)
8. **Không xóa history:** Append change log, không xóa entries cũ
