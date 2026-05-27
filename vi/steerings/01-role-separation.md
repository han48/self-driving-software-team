---
inclusion: auto
---

# Steering: Phân tách Role & Approve Rules

## Khi nào áp dụng
Luôn luôn – áp dụng cho mọi action trong quy trình.

## Quy tắc

1. **AI là Responsible, Human là Accountable:**
   - AI sinh tất cả output (spec, design, tasks, code, test, report, risk, Q&A)
   - Human chỉ approve/confirm
   - AI không bao giờ tự approve

2. **Không tự approve output của mình:**
   - Người cung cấp yêu cầu ≠ người approve spec
   - Người viết code (override AI) ≠ người approve PR
   - Nếu 1 người giữ nhiều role → ghi rõ đang action với role nào

3. **Ghi rõ role khi action:**
   - "Approved by: [Tên] (as [Role])"
   - Ví dụ: "Approved by: Nguyễn A (as Tech Lead)"

4. **Approve gates bắt buộc (Human phải approve trước khi AI tiếp tục):**
   - Requirements → approve → mới sinh Design
   - Design → approve → mới sinh Tasks
   - Tasks → approve → mới thực thi Code
   - PR → approve → mới merge
   - Risk/Issue → confirm → mới track
   - Q&A conclusion → confirm → mới update spec
   - CR → approve → mới thực hiện

5. **Checklist bắt buộc:** Mỗi approve gate có checklist riêng (xem section 14 trong quy trình)

6. **Rotate reviewer:** Cùng 1 người không approve > 3 PR liên tiếp cùng feature
