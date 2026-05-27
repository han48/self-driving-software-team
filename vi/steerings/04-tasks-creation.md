---
inclusion: fileMatch
fileMatchPattern: '**/tasks.md'
---

# Steering: Tạo Tasks (Implementation Plan)

## Khi nào áp dụng
Khi AI sinh tasks.md từ design đã được approve.

## Quy tắc

1. **Format bắt buộc:**
   - Heading: `# Implementation Plan: [Tên feature]`
   - Sections: `## Overview` → `## Tasks` → `## Task Dependency Graph` → `## Notes`
   - Tasks dùng nested checkbox: `- [ ] 1.` → `- [ ] 1.1`
   - Mỗi sub-task kết thúc bằng: `_Requirements: X.X, Y.Y_`

2. **Task granularity:**
   - Mỗi task < 4h effort
   - Mỗi task có expected outcome kiểm chứng được
   - Bao gồm tasks cho test (unit, integration, E2E)

3. **Dependency Graph dùng JSON waves:**
   ```json
   {
     "waves": [
       {"tasks": ["1"]},
       {"tasks": ["2", "3"]},
       {"tasks": ["4"]}
     ]
   }
   ```
   - Tasks cùng wave chạy song song
   - Wave sau đợi wave trước hoàn thành

4. **Property test tasks:**
   - Format: `- [ ] N.M Viết property test — Property P`
   - Bên trong: `**Property P: [Tên]**` + `**Validates: Requirements X.X**`

5. **Status tracking:**
   - `- [ ]` = Not started
   - `- [x]` = Complete
   - AI tự cập nhật status khi hoàn thành task

6. **Output location:** `openspec/specs/{feature-name}/tasks.md`
