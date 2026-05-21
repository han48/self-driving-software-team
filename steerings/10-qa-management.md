---
inclusion: fileMatch
fileMatchPattern: '**/qa/**'
---

# Steering: Quản lý Q&A

## Khi nào áp dụng
Khi AI phát hiện ambiguity trong spec/design hoặc cần clarify với stakeholder.

## Quy tắc

1. **AI tự tạo Q&A:** Khi phát hiện spec mơ hồ → tự tạo QA-XXX, không đợi Human.

2. **Cấu trúc:**
   - Index: `openspec/qa/index.md` (bảng tổng hợp)
   - Thread: `openspec/qa/QA-XXX/thread.md` (conversation chi tiết)

3. **thread.md format bắt buộc:**
   ```markdown
   # QA-XXX: [Chủ đề]
   
   **Status:** Open / Answered / Cancelled
   **Ngày tạo:** YYYY-MM-DD
   **Hỏi bởi:** AI
   **Đối tượng:** [KH / Architect / Lead]
   **Ưu tiên:** High / Medium / Low
   **Liên quan:** [spec/CR/Risk reference]
   
   ---
   ## Câu hỏi gốc
   [Nội dung]
   
   ---
   ## Thread
   ### [YYYY-MM-DD] [Người] →
   [Reply]
   
   ---
   ## Kết luận
   **Quyết định:** [Tóm tắt]
   **Action items:**
   - [ ] [Hành động]
   ```

4. **Khi có kết luận:**
   - AI tổng hợp kết luận → Human confirm
   - AI tự update spec/design dựa trên kết luận → Human approve changes
   - Chuyển status sang "Answered" trong index.md

5. **Mỗi Q&A 1 folder:** Không gộp nhiều topic vào 1 thread

6. **Deadline:** QA High priority phải có trả lời trong 48h
