---
inclusion: auto
---

# Steering: Failure Handling & Rollback

## Khi nào áp dụng
Khi AI output sai, test fail liên tục, hoặc cần rollback thay đổi.

## Retry Policy

```
Lần 1: AI tự sửa dựa trên error message / feedback
Lần 2: AI thử approach khác (different algorithm/architecture)
Lần 3: AI tổng hợp attempts + root cause → escalate Human
```

**Sau 3 lần fail → AI PHẢI:**
1. Dừng lại
2. Sinh report: "3 attempts failed, root cause: [X], suggested approach: [Y]"
3. Chuyển task sang Human override (Developer role)
4. Ghi RADIO "D" + tạo ISSUE-XXX nếu cần

## Xử lý theo tình huống

| Tình huống | Hành động | Ai quyết định |
|-----------|----------|---------------|
| Spec sai logic | AI tự sửa (vòng lặp) → vẫn sai → Human request changes | Human |
| Design không khả thi | AI redesign → fail 2 lần → escalate Tech Lead | Tech Lead |
| Code fail test | AI fix → retry 3 lần → escalate | Tech Lead |
| Code pass test nhưng sai logic | Human reject PR → AI re-read spec → fix | Human |
| AI không hiểu yêu cầu | AI tạo QA → chờ clarification → retry | Human |

## Rollback Strategy

| Scope | Cách rollback | Tool |
|-------|--------------|------|
| Code change sai | `git revert` commit | Git |
| Spec change sai | Revert về version trước | Git |
| Design sai | Revert + re-design từ spec | Git |
| Feature sai hướng | Revert branch, quay lại spec phase | Git branch |
| Production issue | Rollback deployment → hotfix | CI/CD |

## Quy tắc

1. **Không xóa history:** Dùng `git revert`, không `git push --force`
2. **Ghi lý do rollback:** Trong commit message + RADIO "D"
3. **Post-mortem:** Sau rollback → AI sinh 5WHY analysis
4. **Prevention:** Mỗi rollback → improvement (thêm test, thêm check)
5. **Không retry vô hạn:** Tối đa 3 lần, sau đó escalate
6. **Human override:** Khi AI fail → Developer có quyền code trực tiếp
