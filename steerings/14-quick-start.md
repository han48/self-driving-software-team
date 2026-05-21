---
inclusion: manual
---

# Steering: Quick Start Guide

## Khi nào áp dụng
Khi team mới bắt đầu áp dụng quy trình AI-First, Spec-Driven.

> 📖 Chi tiết đầy đủ: xem file [`quick-start.md`](../quick-start.md)

## Tóm tắt luồng

```
Yêu cầu thô → [AI: Spec] → ✅ → [AI: Design] → ✅ → [AI: Tasks] → ✅ → [AI: Code+Test] → [AI: RADIO] → ✅ Done
```

## 6 bước

1. **Chuẩn bị:** Tạo folder `openspec/`
2. **Yêu cầu:** Viết mô tả thô cho AI
3. **Spec:** AI sinh → Human approve
4. **Design:** AI sinh + HTML prototype → Human approve
5. **Tasks:** AI sinh → Human approve → AI thực thi
6. **Review:** AI sinh RADIO → Human approve PR

## Checklist sẵn sàng

- [ ] Folder `openspec/` đã tạo
- [ ] AI Agent đã setup (Kiro/Claude)
- [ ] Team hiểu: AI làm, Human approve
- [ ] Ít nhất 1 người biết approve checklist
- [ ] CI/CD pipeline có chạy test tự động
