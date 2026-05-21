---
inclusion: fileMatch
fileMatchPattern: '**/radio.md'
---

# Steering: Sinh RADIO Report

## Khi nào áp dụng
Khi AI cần báo cáo tiến độ sau mỗi task/sprint hoặc khi có thay đổi đáng kể.

## Tần suất cập nhật (bắt buộc)

| Trigger | Khi nào | Bắt buộc |
|---------|---------|----------|
| Task complete | Mỗi khi task chuyển `[x]` | ✅ |
| Test fail | Khi CI report failure | ✅ |
| Blocker mới | Khi phát hiện blocker/risk | ✅ |
| End of day | Cuối ngày (nếu có progress) | ✅ |
| Sprint end | Tổng hợp cuối sprint | ✅ |
| Milestone | Hoàn thành wave trong dependency | Khuyến nghị |
| **Minimum** | **Ít nhất 1 lần/ngày nếu đang active** | ✅ |

## Quy tắc

1. **File location:** `openspec/specs/{feature-name}/radio.md`

2. **Cấu trúc file radio.md:**
   ```markdown
   # RADIO Report: [Tên feature]

   **Ngày cập nhật:** YYYY-MM-DD
   **Sprint/Phase:** [Sprint X]
   **Sinh bởi:** AI Agent
   **Reviewed bởi:** [Human – nếu đã review]

   ---

   ## Latest Report

   **R (Review):** [Trạng thái hiện tại, metric cụ thể]
   **A (Action):** [Đang làm gì, next steps]
   **D (Difficulty):** [Blockers, risks, issues – hoặc "None"]
   **I (Information):** [Context ảnh hưởng tiến độ – hoặc "—"]
   **O (Outcome):** [Kết quả đạt được, số liệu]

   ---

   ## History

   ### [YYYY-MM-DD]
   - R: [...]
   - A: [...]
   - D: [...]
   - I: [...]
   - O: [...]

   ### [YYYY-MM-DD]
   - R: [...]
   - A: [...]
   - D: [...]
   - I: [...]
   - O: [...]
   ```

3. **RADIO content rules:**
   - R: từ test results, spec compliance %, tasks.md status (X/Y done)
   - A: từ tasks.md (task đang in-progress)
   - D: từ risk-issue register, test failures, dependency issues
   - I: từ monitoring, external factors, stakeholder updates
   - O: từ CI/CD results, coverage %, tasks completed count

4. **"D" items quan trọng:**
   - Mỗi D item nghiêm trọng → kiểm tra đã có RISK-XXX hoặc ISSUE-XXX chưa
   - Nếu chưa → tạo ngay
   - Critical D items phải escalate trong RADIO

5. **History:**
   - Mỗi lần AI sinh RADIO mới → Latest Report cập nhật, report cũ chuyển xuống History
   - History sắp xếp mới nhất ở trên
   - Giữ tối đa 10 entries gần nhất (cũ hơn có thể archive)

6. **Không chủ quan:** RADIO phải dựa trên data, không phải cảm tính

7. **Đầu mục bắt buộc:**

   | Đầu mục | Bắt buộc |
   |---------|----------|
   | `# RADIO Report: [Tên]` | ✅ |
   | Header metadata (Ngày, Sprint, AI, Human) | ✅ |
   | `## Latest Report` | ✅ |
   | R, A, D, I, O (đủ 5 items) | ✅ |
   | `## History` | ✅ |
