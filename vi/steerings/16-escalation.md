---
inclusion: auto
---

# Steering: Escalation Matrix & Communication Protocol

## Khi nào áp dụng
Khi AI cần escalate vấn đề, khi Human không respond, hoặc khi cần giao tiếp giữa các role.

## Escalation Matrix

### Theo Severity

| Severity | Escalate cho ai | SLA phản hồi | SLA xử lý | Kênh |
|----------|----------------|--------------|-----------|------|
| **Critical** | PM + Tech Lead + Stakeholder | 1 giờ | 4 giờ | Slack urgent + Phone |
| **High** | Tech Lead + PM | 4 giờ | 24 giờ | Slack channel |
| **Medium** | Tech Lead | 24 giờ | 3 ngày | Slack / Email |
| **Low** | Ghi nhận, xử lý khi có bandwidth | 48 giờ | Next sprint | Email / Backlog |

### Theo loại vấn đề

| Vấn đề | Escalate path | Trigger |
|--------|--------------|---------|
| Production down | AI → Tech Lead → PM → Stakeholder | Ngay lập tức |
| Risk High xảy ra | AI → Tech Lead → PM | Trong 4 giờ |
| Q&A High quá hạn (> 48h) | AI flag RADIO "D" → PM escalate KH | Sau 48h |
| AI fail > 3 lần | AI → Tech Lead (Human override) | Sau 3 retry |
| Spec conflict | AI → Tech Lead + BA | Ngay khi phát hiện |
| Security vulnerability | AI → Tech Lead → Security Engineer | Ngay lập tức |
| Human không respond approve | AI nhắc (24h) → nhắc (48h) → escalate PM | Sau 48h |

## Communication Protocol

### Kênh giao tiếp

| Loại | Kênh | SLA | Ví dụ |
|------|------|-----|-------|
| Approve request | Slack + In-tool | 4h (High), 24h (Medium) | "PR ready", "Spec cần approve" |
| Clarification | Slack thread + QA file | 24h (High), 48h (Medium) | "Spec mơ hồ" |
| Escalation | Slack urgent / Phone | 1h (Critical), 4h (High) | "Blocker" |
| Information | Email / Channel | Không cần response | "RADIO mới" |

### Khi Human không respond

```
0–24h:  AI chờ, làm task khác không phụ thuộc
24–48h: AI nhắc lại + ghi RADIO "D"
48–72h: AI escalate PM + tạo ISSUE
> 72h:  AI tạm dừng feature, report blocker
```

## Quy tắc

1. **Async-first:** Mặc định async, chỉ sync khi Critical
2. **Thread-based:** Mỗi topic 1 thread
3. **Actionable:** Mỗi message rõ: ai làm gì, deadline khi nào
4. **Recorded:** Decision phải ghi trong spec/QA/RADIO
5. **AI tự notify:** AI gửi notification khi cần approve
6. **Không skip level:** Escalate theo thứ tự (trừ Critical)
