---
inclusion: fileMatch
fileMatchPattern: '**/design.md'
---

# Steering: Tạo Design

## Khi nào áp dụng
Khi AI sinh design.md từ requirements đã được approve.

## Quy tắc

1. **Format bắt buộc:**
   - Heading: `# Design Document: [Tên feature]`
   - Sections theo thứ tự: Overview → Site Map → GUI Design → Architecture → Data Models / Database Design + ERD → Infrastructure Design → Components and Interfaces → Correctness Properties → Error Handling → Testing Strategy

2. **GUI Design:**
   - Không viết trực tiếp trong design.md
   - Tạo HTML prototype trong `openspec/specs/{feature-name}/gui/`
   - Trong design.md chỉ reference: `> 📁 Refer: openspec/specs/{feature-name}/gui/`
   - HTML prototype phải có đầy đủ navigation flow, click được, responsive
   - Dùng CSS framework nhẹ (TailwindCSS, Bootstrap)

3. **Architecture:**
   - Dùng Mermaid diagrams (graph TD cho system, sequenceDiagram cho flows)
   - Bao gồm cả system overview và sequence diagrams

4. **Correctness Properties:**
   - Dùng format: `*For any* [condition], [property must hold]`
   - Mỗi property phải có: `**Validates: Requirements X.X**`

5. **Vòng lặp kiểm tra trước khi đưa Human approve:**
   - Design có cover hết requirements?
   - Architecture có scalable, secure?
   - DB design có đúng normalization?
   - Có conflict giữa các component?
   - Có technical risk nào? → tạo RISK-XXX
   - Có câu hỏi cần clarify? → tạo QA-XXX
   - Lặp lại cho đến khi không còn vấn đề

6. **Output location:** `openspec/specs/{feature-name}/design.md`
