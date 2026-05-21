---
inclusion: fileMatch
fileMatchPattern: '**/risk-issue/**'
---

# Steering: Quản lý Risk & Issue

## Khi nào áp dụng
Khi AI phát hiện risk hoặc issue trong quá trình làm việc.

## Quy tắc

1. **AI tự tạo Risk/Issue:** Không đợi Human – AI detect và tạo ngay.

2. **Risk format (trong `openspec/risk-issue/risks.md`):**
   ```
   ### RISK-XXX: [Tên]
   - **Ngày phát hiện:** YYYY-MM-DD
   - **Phát hiện bởi:** AI
   - **Xác suất:** High / Medium / Low
   - **Ảnh hưởng:** High / Medium / Low
   - **Mức độ:** Critical / High / Medium / Low
   - **Mô tả:** [Chi tiết]
   - **Hành động phòng ngừa (Prevention):** [Tránh risk xảy ra]
   - **Hành động giảm thiểu (Mitigation):** [Nếu xảy ra → hậu quả nhỏ nhất]
   - **Owner:** [Ai theo dõi]
   - **Trigger:** [Dấu hiệu nhận biết]
   - **Status:** Open
   ```

3. **Issue format (trong `openspec/risk-issue/issues.md`):**
   ```
   ### ISSUE-XXX: [Tên]
   - **Ngày phát sinh:** YYYY-MM-DD
   - **Phát hiện bởi:** AI
   - **Severity:** Critical / High / Medium / Low
   - **Ảnh hưởng:** [Impact]
   - **Mô tả:** [Chi tiết]
   - **5WHY Analysis:**
     - Why 1: [Tại sao?] → [Trả lời]
     - Why 2: [Tại sao?] → [Trả lời]
     - Why 3: [Tại sao?] → [Trả lời]
     - Why 4: [Tại sao?] → [Trả lời]
     - Why 5: [Tại sao?] → [Root cause]
   - **Root cause:** [Kết luận từ 5WHY]
   - **Resolution plan:** [Xử lý root cause]
   - **Prevention:** [Tránh tái phát]
   - **Owner:** [Ai xử lý]
   - **Deadline:** YYYY-MM-DD
   - **Status:** Open
   ```

4. **Phân biệt:**
   - Risk = chưa xảy ra (tiềm ẩn) → Prevention + Mitigation
   - Issue = đã xảy ra → Resolution

5. **Điều kiện Closed cho Risk:**
   - Cả 2 hành động phải hoàn thành:
     - ✅ Prevention đã áp dụng (giảm xác suất)
     - ✅ Mitigation đã chuẩn bị sẵn (giảm hậu quả)
   - Chỉ 1/2 → status vẫn là Mitigated, chưa được Closed

6. **5WHY bắt buộc cho Issue:**
   - AI tự thực hiện 5WHY analysis khi tạo issue
   - Mỗi "Why" phải có evidence, không đoán
   - Root cause phải là thứ có thể fix được
   - Resolution plan xử lý root cause, không chỉ triệu chứng
   - Prevention ngăn issue tái phát

7. **Tích hợp RADIO:** Flag risk/issue trong RADIO "D" (Difficulty) items

8. **Human chỉ confirm:** AI tạo, Human confirm severity + approve plan
