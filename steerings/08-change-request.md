---
inclusion: fileMatch
fileMatchPattern: '**/change-request/**'
---

# Steering: Quản lý Change Request (CR)

## Khi nào áp dụng
Khi có CR mới hoặc cần xử lý CR trong backlog.

## Quy tắc

1. **Mọi CR phải vào backlog:** `openspec/change-request/backlog.md`

2. **Format CR entry bắt buộc:**
   ```
   ### CR-XXX: [Tiêu đề]
   - **Ngày nhận:** YYYY-MM-DD
   - **Từ:** [Nguồn]
   - **Ưu tiên:** High / Medium / Low
   - **Mô tả:** [Mô tả ngắn gọn]
   ```

3. **Lifecycle:** Pending → Approved → In Progress → Review → Done (hoặc Rejected)

4. **Khi chuyển sang In Progress:**
   - Tạo folder `openspec/change-request/CR-XXX/`
   - AI sinh requirements.md → Human approve
   - AI sinh design.md → Human approve
   - AI sinh tasks.md → Human approve
   - AI thực thi tasks

5. **Fields bắt buộc theo status:**
   - Approved: thêm `**Approved by:**` + `**Scheduled:**`
   - In Progress: thêm `**Started:**` + `**Spec:**`
   - Done: thêm `**Completed:**`
   - Rejected: thêm `**Rejected by:**` + `**Lý do:**`

6. **CR nhỏ (< 1 ngày):** Có thể skip design, chỉ cần requirements + tasks
7. **CR lớn (> 1 ngày):** Phải có đầy đủ spec (requirements + design + tasks)

8. **Không approve = không làm:** AI không được thực hiện CR chưa approve
