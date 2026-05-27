---
inclusion: manual
---

# Steering: Checklist Approve (Tham chiếu nhanh)

## Khi nào áp dụng
Mỗi khi Human cần approve bất kỳ output nào của AI. AI phải nhắc Human sử dụng checklist phù hợp.

## Mapping: Output → Checklist

| Output cần approve | Checklist áp dụng |
|-------------------|-------------------|
| requirements.md | Checklist Approve Requirements |
| design.md | Checklist Approve Design |
| tasks.md | Checklist Approve Tasks |
| Pull Request (code) | Checklist Approve PR |
| RADIO report | Checklist Approve RADIO |
| CR (accept/reject) | Checklist Approve CR |
| Bugfix (bugfix.md + design.md) | Checklist Approve Bugfix |
| Risk (RISK-XXX) | Checklist Approve Risk |
| Issue resolution | Checklist Approve Issue Resolution |
| Q&A conclusion | Checklist Approve Q&A Conclusion |
| deliverables.md | Checklist Approve Deliverables & Limitations |

## Quy tắc

1. **AI nhắc Human:** Khi đưa output cho Human approve, AI phải ghi rõ: "Vui lòng review theo Checklist [tên] trước khi approve."
2. **Tick hết mới approve:** Reviewer phải pass tất cả items trong checklist
3. **Nếu không pass:** Request changes, AI sửa, submit lại
4. **Ghi lại:** Ai approve, ngày nào, role nào: "Approved by: [Tên] (as [Role]) – YYYY-MM-DD"
5. **Rotate:** Cùng 1 người không approve > 3 PR liên tiếp cùng feature

## Template nhắc approve

Khi AI đưa output cho Human, dùng format:

```
✅ [Output type] đã sẵn sàng để review.
📋 Checklist áp dụng: [Tên checklist]
👤 Reviewer đề xuất: [Role phù hợp]
```
