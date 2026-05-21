# Quick Start Guide – Bắt đầu trong 15 phút

> Dành cho team mới muốn chạy thử quy trình AI-First, Spec-Driven ngay mà không cần đọc hết tài liệu chính.

---

## Bước 1: Chuẩn bị (5 phút)

```bash
# Tạo cấu trúc thư mục
mkdir -p openspec/specs openspec/bugs openspec/change-request openspec/risk-issue openspec/qa openspec/deliverables
```

## Bước 2: Cung cấp yêu cầu (2 phút)

Viết yêu cầu thô vào prompt cho AI Agent. Ví dụ:
> "Cần API đăng ký user, nhận email + password + name, validate input, lưu vào DB, gửi email xác nhận."

Không cần format chuẩn – có thể là ticket, meeting note, hoặc chat. AI sẽ hỏi clarification nếu mơ hồ.

## Bước 3: AI sinh Spec → Bạn approve (3 phút)

AI sẽ sinh `openspec/specs/{feature}/requirements.md`. Bạn:
1. Đọc acceptance criteria (EARS format: THE/WHEN/IF + SHALL)
2. Kiểm tra edge cases có đủ không
3. Approve hoặc request changes

## Bước 4: AI sinh Design → Bạn approve (3 phút)

AI sinh `design.md` + `gui/` (HTML prototype). Bạn:
1. Mở HTML prototype trên browser, click thử flow
2. Review architecture diagram
3. Approve hoặc request changes

## Bước 5: AI sinh Tasks → Approve → AI thực thi (2 phút)

AI sinh `tasks.md`, bạn approve, AI bắt đầu code + test.

## Bước 6: Review kết quả

AI sinh RADIO report. Bạn review PR + RADIO → approve hoặc request changes.

---

## Tóm tắt 1 dòng

```
Yêu cầu thô → [AI: Spec] → ✅ Approve → [AI: Design] → ✅ Approve → [AI: Tasks] → ✅ Approve → [AI: Code+Test] → [AI: RADIO] → ✅ Final Approve
```

---

## Checklist "Đã sẵn sàng"

- [ ] Folder `openspec/` đã tạo
- [ ] AI Agent đã setup (Kiro/Claude)
- [ ] Team hiểu: AI làm, Human approve
- [ ] Ít nhất 1 người biết approve checklist (xem Section 14 trong tài liệu chính)
- [ ] CI/CD pipeline có chạy test tự động

---

## Tiếp theo

Sau khi chạy thử thành công 1 feature nhỏ:
1. Đọc tài liệu chính (`self-driving-software-team.md`) để hiểu sâu hơn
2. Áp dụng Adoption Level 1 (Section 5)
3. Dần dần mở rộng scope theo lộ trình Level 1 → 4
