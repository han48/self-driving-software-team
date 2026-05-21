---
inclusion: fileMatch
fileMatchPattern: '**/bugfix.md'
---

# Steering: Tạo Bugfix Spec

## Khi nào áp dụng
Khi AI xử lý bug phức tạp cần root cause analysis và regression prevention.

## Quy tắc

1. **bugfix.md – Format bắt buộc:**
   - Heading: `# Bugfix Requirements Document`
   - Sections: `## Introduction` → `## Bug Analysis`
   - Bug Analysis có 3 sub-sections:
     - `### Current Behavior (Defect)` – WHEN...THEN the system [sai]
     - `### Expected Behavior (Correct)` – WHEN...THE SYSTEM SHALL [đúng]
     - `### Unchanged Behavior (Regression Prevention)` – WHEN...THE SYSTEM SHALL CONTINUE TO [giữ]
   - Đánh số: 1.x (Defect), 2.x (Correct), 3.x (Unchanged)

2. **design.md – Format bắt buộc:**
   - Sections: Overview → Glossary → Bug Details → Expected Behavior → Hypothesized Root Cause (5WHY) → Fix Implementation → Correctness Properties → Testing Strategy → Files Changed
   - `## Hypothesized Root Cause (5WHY)` phải dùng phương pháp 5WHY:
     - 5 levels "Why" liên tiếp
     - Kết luận Root cause
     - Prevention (tránh tái phát)
   - `## Files Changed` là bảng liệt kê files thay đổi (New/Modified)
   - Correctness Properties gồm: valid passes, invalid fails, no side effects

3. **tasks.md – Format bắt buộc:**
   - Reference dùng `_Bugfix: X.X_` (không phải `_Requirements:_`)
   - Luồng: Fix code → Apply → Unit tests → Feature tests
   - Task Dependency Graph bắt buộc

4. **Nguyên tắc surgical fix:**
   - Thay đổi ít nhất có thể
   - Unchanged Behavior PHẢI liệt kê rõ ràng
   - Files Changed PHẢI liệt kê rõ files được phép sửa

5. **Output location:** `openspec/bugs/{bug-name}/`
