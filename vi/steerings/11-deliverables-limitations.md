---
inclusion: fileMatch
fileMatchPattern: '**/deliverables/**'
---

# Steering: Quản lý Deliverables & Limitations

## Khi nào áp dụng
Khi kết thúc sprint/phase/release hoặc khi cần tổng hợp trạng thái dự án.

## Quy tắc

1. **AI tự tổng hợp:** Scan toàn bộ tasks.md, test results, RADIO để sinh deliverables

2. **Cấu trúc folder:**
   ```
   openspec/deliverables/
   ├── index.md                ← Bảng tổng hợp tất cả đợt deliver
   ├── sprint-XX/deliverables.md   ← Theo sprint
   ├── phase-X/deliverables.md     ← Theo phase (gộp sprints)
   └── YYYY-MM-DD/deliverables.md  ← Theo ngày (hotfix, urgent)
   ```

3. **index.md format:**
   ```markdown
   # Deliverables Index
   
   | Đợt | Ngày | Features delivered | Limitations | Link |
   |-----|------|-------------------|-------------|------|
   | Sprint 3 | 2025-03-28 | Login, Register | 2 LIM | `sprint-03/deliverables.md` |
   ```

4. **Deliverable entry:**
   ```
   ### Feature: [Tên]
   - **Spec:** `openspec/specs/{feature-name}/`
   - **Status:** ✅ Complete
   - **Test coverage:** XX%
   - **Spec compliance:** XX%
   - **Deployed:** [environment]
   - **Mô tả:** [Tóm tắt]
   ```

5. **Limitation entry:**
   ```
   ### LIM-XXX: [Tên]
   - **Loại:** Not Implemented / Partial / Technical Constraint / Known Issue
   - **Liên quan:** [Spec/CR reference]
   - **Mô tả:** [Chi tiết]
   - **Lý do:** [Tại sao]
   - **Ảnh hưởng:** [Impact]
   - **Kế hoạch:** Phase 2 / Backlog / Won't Fix
   - **Workaround:** [Nếu có]
   ```

6. **Phân loại Limitations:**
   - Not Implemented: requirement có nhưng chưa làm
   - Partial: đã làm nhưng chưa đầy đủ
   - Technical Constraint: giới hạn kỹ thuật
   - Known Issue: bug đã biết chưa fix

7. **Tách theo đợt deliver:**
   - Sprint: mỗi sprint 1 file
   - Phase: tổng hợp nhiều sprints
   - Hotfix/urgent: theo ngày (YYYY-MM-DD)

8. **Cập nhật index.md** mỗi khi có đợt deliver mới

9. **Human confirm:** Mọi limitation phải được Human xác nhận trước khi gửi KH

10. **Summary bắt buộc:** Bảng metric tổng hợp ở cuối mỗi file deliverables.md
