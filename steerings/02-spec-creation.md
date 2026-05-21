---
inclusion: fileMatch
fileMatchPattern: '**/requirements.md'
---

# Steering: Tạo Spec (Requirements)

## Khi nào áp dụng
Khi AI sinh requirements.md cho feature mới hoặc CR.

## Quy tắc

1. **Format bắt buộc:**
   - Heading: `# Requirements Document`
   - Sections: `## Introduction` → `## Glossary` → `---` → `## Requirements`
   - Mỗi requirement: `### Requirement N: [Tên]` + `**User Story:**` + `#### Acceptance Criteria`

2. **EARS Notation bắt buộc cho Acceptance Criteria:**
   - `THE [Component] SHALL [behavior]` – hành vi luôn đúng
   - `WHEN [condition] THE [Component] SHALL [behavior]` – có trigger
   - `IF [condition] THEN THE [Component] SHALL [behavior]` – có điều kiện
   - Component name viết HOA: THE API, THE System, THE Admin_Panel...

3. **Vòng lặp kiểm tra trước khi đưa Human approve:**
   - Spec có rõ ràng, đầy đủ không?
   - Có mâu thuẫn logic giữa các AC?
   - Có khó khăn technical nào?
   - Phát hiện risk → tạo RISK-XXX trong `openspec/risk-issue/risks.md`
   - Phát hiện ambiguity → tạo QA-XXX trong `openspec/qa/`
   - Lặp lại cho đến khi không còn vấn đề

4. **Chất lượng:**
   - Mỗi AC phải testable (chuyển được thành test case)
   - Bao gồm cả happy path và error cases
   - Không có assumption ẩn – mọi giả định ghi rõ
   - Glossary định nghĩa tất cả thuật ngữ chuyên ngành

5. **Output location:** `openspec/specs/{feature-name}/requirements.md`
