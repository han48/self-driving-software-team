# Prompt Templates – Mẫu prompt cho AI ở từng Phase

> Dùng các template dưới đây khi cần AI sinh output đúng format quy trình.
> Thay `{...}` bằng nội dung thực tế của bạn.

---

## 1. Sinh Requirements (Spec)

### Prompt cơ bản

```
Hãy sinh requirements.md cho feature sau:

{Mô tả yêu cầu thô – có thể là 1-2 câu hoặc meeting notes}

Yêu cầu:
- Dùng EARS notation (THE/WHEN/IF + SHALL)
- Bao gồm User Story cho mỗi requirement
- Cover cả happy path và error cases
- Định nghĩa Glossary cho thuật ngữ chuyên ngành
- Đánh số acceptance criteria (1, 2, 3...)
- Nếu phát hiện ambiguity → tạo QA-XXX
- Nếu phát hiện risk → tạo RISK-XXX

Output format: theo cấu trúc trong Phụ lục A.2
File location: openspec/specs/{feature-name}/requirements.md
```

### Prompt nâng cao (có context)

```
Context:
- Project: {tên project}
- Tech stack: {Node.js + PostgreSQL + React...}
- Existing specs: {reference đến specs liên quan nếu có}
- Constraints: {deadline, budget, technical limitations}

Yêu cầu thô từ khách hàng:
"{Copy paste yêu cầu gốc}"

Hãy sinh requirements.md. Lưu ý:
- Phải compatible với architecture hiện tại
- Kiểm tra conflict với specs đã có
- Flag nếu yêu cầu vượt scope hợp đồng
```

---

## 2. Sinh Design

### Prompt cơ bản

```
Dựa trên requirements.md đã approve (file: openspec/specs/{feature}/requirements.md),
hãy sinh design.md bao gồm:

1. Overview (tech stack, approach)
2. Site Map (Mermaid graph)
3. GUI Design (sinh HTML prototype vào folder gui/)
4. Architecture (sequence diagrams)
5. Database Design + ERD
6. Infrastructure Design
7. Components & Interfaces
8. Correctness Properties (cho property-based testing)
9. Error Handling strategy
10. Testing Strategy

Yêu cầu:
- Mỗi component phải reference requirement tương ứng
- GUI prototype phải mở được trực tiếp trên browser
- Database design phải có index strategy
- Correctness Properties dạng "For any..."

Output format: theo cấu trúc trong Phụ lục A.3
```

### Prompt cho feature nhỏ (skip sections)

```
Feature size: Small (< 1 ngày)
Chỉ cần sinh design.md với sections:
- Overview
- Components & Interfaces
- Testing Strategy

Skip: Site Map, GUI, Architecture, Database, Infrastructure
(Feature quá nhỏ, không cần full design)
```

---

## 3. Sinh Tasks

### Prompt cơ bản

```
Dựa trên design.md đã approve (file: openspec/specs/{feature}/design.md),
hãy sinh tasks.md:

Yêu cầu:
- Mỗi task < 4 giờ effort
- Nested checkbox format (- [ ] 1. / - [ ] 1.1)
- Mỗi sub-task có _Requirements: X.X_ reference
- Task Dependency Graph (JSON waves)
- Bao gồm tasks cho: code, unit test, integration test, E2E test
- Thứ tự logic: setup → core logic → tests → integration

Output format: theo cấu trúc trong Phụ lục A.4
```

---

## 4. Sinh RADIO Report

### Prompt cơ bản

```
Hãy sinh/cập nhật RADIO report cho feature {tên}:

Data sources:
- tasks.md: {X}/{Y} tasks done
- CI results: {pass/fail, coverage %}
- Blockers: {list nếu có}
- Recent changes: {commits gần đây}

Format:
- R: Trạng thái + metrics cụ thể
- A: Task đang làm + next steps
- D: Blockers/risks (hoặc "None")
- I: Context ảnh hưởng tiến độ (hoặc "—")
- O: Kết quả đạt được + số liệu

Nếu có D items nghiêm trọng → kiểm tra đã có RISK-XXX/ISSUE-XXX chưa, tạo nếu chưa.
```

---

## 5. Sinh Bugfix Spec

### Prompt cơ bản

```
Bug report: "{mô tả bug}"
Steps to reproduce: {steps}
Expected: {expected behavior}
Actual: {actual behavior}

Hãy sinh bugfix.md với:
1. Current Behavior (Defect) – EARS format
2. Expected Behavior (Correct) – EARS format
3. Unchanged Behavior (Regression Prevention) – EARS format

Sau đó sinh design.md với:
- 5WHY root cause analysis (đào đến root cause thực sự)
- Fix Implementation (surgical, thay đổi ít nhất)
- Files Changed (bảng rõ ràng)
- Prevention (làm gì để bug loại này không tái phát)
- Correctness Properties

Output: openspec/bugs/{bug-name}/bugfix.md + design.md + tasks.md
```

---

## 6. Đánh giá CR (Change Request)

### Prompt cơ bản

```
CR mới nhận:
- Từ: {nguồn}
- Mô tả: "{nội dung CR}"
- Chi tiết: {bullet list}

Hãy:
1. Đánh giá impact đến hệ thống hiện tại
2. Estimate effort (giờ/ngày)
3. Đánh giá risk nếu implement
4. Đề xuất priority (High/Medium/Low) với lý do
5. Kiểm tra conflict với specs/design hiện tại
6. Ghi vào backlog.md section "Pending" đúng format
```

---

## 7. Detect Risk & Issue

### Prompt cho Risk Detection

```
Hãy review {file: spec/design/code} và phát hiện risks:

Kiểm tra:
- Security risks (injection, auth bypass, data leak)
- Performance risks (N+1, memory leak, timeout)
- Scalability risks (single point of failure, bottleneck)
- Dependency risks (deprecated lib, breaking changes)
- Business risks (scope creep, unclear requirement)

Với mỗi risk phát hiện:
- Đánh giá Xác suất (H/M/L) + Ảnh hưởng (H/M/L)
- Viết Prevention (làm gì để tránh)
- Viết Mitigation (nếu xảy ra thì làm gì)
- Xác định Trigger (dấu hiệu nhận biết)

Output: thêm vào openspec/risk-issue/risks.md đúng format RISK-XXX
```

---

## 8. Sinh Q&A

### Prompt khi phát hiện ambiguity

```
Khi đọc spec/design, tôi phát hiện điểm mơ hồ:
"{mô tả điểm mơ hồ}"

Hãy:
1. Tạo QA-XXX với câu hỏi rõ ràng, có context
2. Liệt kê các options có thể (nếu biết)
3. Đề xuất default answer (nếu có thể suy luận)
4. Xác định ai cần trả lời (KH / Architect / Lead)
5. Đánh giá priority (High nếu block implementation)

Output: openspec/qa/QA-XXX/thread.md + cập nhật index.md
```

---

## Tips sử dụng Prompt

1. **Càng nhiều context càng tốt:** Đính kèm file liên quan, không chỉ mô tả
2. **Reference existing specs:** Nếu có specs khác liên quan, mention chúng
3. **Specify constraints:** Deadline, tech stack, team size ảnh hưởng output
4. **Iterative:** Nếu output chưa đúng → feedback cụ thể, không prompt lại từ đầu
5. **Verify format:** Sau khi AI sinh → kiểm tra đúng format theo Phụ lục A/B
