---
inclusion: auto
---

# Steering: Quy trình AI-First, Spec-Driven (Tổng quan)

## Nguyên tắc cốt lõi

> Con người phê duyệt và chịu trách nhiệm. AI định nghĩa, thực thi và báo cáo.

## Workflow chính

```
Human cung cấp yêu cầu thô
    ↓
AI sinh Spec (requirements.md) → 🔄 AI tự kiểm tra → Human approve
    ↓
AI sinh Design (design.md + gui/) → 🔄 AI tự kiểm tra → Human approve
    ↓
AI sinh Tasks (tasks.md) → Human approve
    ↓
AI thực thi (code + test L1–L4, L6)
    ↓
AI validate (spec compliance + test results)
    ↓
AI sinh RADIO report
    ↓
Human approve (PR + RADIO)
```

## Folder structure

```
📁 openspec/
├── 📁 specs/{feature}/        ← Feature specs + gui/
├── 📁 bugs/{bug}/             ← Bugfix specs
├── 📁 change-request/         ← CR backlog + specs
├── 📁 risk-issue/             ← risks.md + issues.md
├── 📁 qa/                     ← Q&A index + threads
└── deliverables.md            ← Deliverables + Limitations
```

## AI tự động tạo trong quá trình làm

- **Risk:** Khi phát hiện rủi ro tiềm ẩn → `openspec/risk-issue/risks.md`
- **Issue:** Khi phát hiện vấn đề đang xảy ra → `openspec/risk-issue/issues.md`
- **Q&A:** Khi phát hiện spec mơ hồ → `openspec/qa/QA-XXX/thread.md`
- **RADIO:** Sau mỗi task/milestone → trong spec folder hoặc inline

## Quy tắc vàng

1. Không bỏ qua phase: Requirements → Design → Tasks → Execute
2. Không thực thi khi chưa approve
3. Mọi output phải traceable (reference Requirements/Bugfix)
4. AI tự detect risk/issue/Q&A – không đợi Human
5. Human chỉ approve, không tự làm (trừ manual test L5)
6. AI retry tối đa 3 lần, sau đó escalate Human
7. RADIO cập nhật ít nhất 1 lần/ngày khi đang active
8. Mọi spec thay đổi phải có version + change log
9. Escalate theo SLA: Critical 1h, High 4h, Medium 24h
10. Async-first communication, sync chỉ khi Critical

> 📖 Thuật ngữ viết tắt: xem Glossary trong `self-driving-software-team.md`
