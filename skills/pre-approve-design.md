---
inclusion: manual
---

# Skill: Pre-Approve Check – Design

## Mục đích
Chạy automated checklist verification cho design.md trước khi đưa Human approve.

## Input
File `design.md` + `requirements.md` (đã approve) cần kiểm tra.

## Checklist tự động

### Format & Structure
- [ ] Heading là `# Design Document: [Tên]`
- [ ] Có section `## Overview` (không rỗng)
- [ ] Có section `## Site Map`
- [ ] Có section `## GUI Design` với reference đến folder gui/
- [ ] Có section `## Architecture` với ít nhất 1 Mermaid diagram
- [ ] Có section `## Data Models / Database Design + ERD`
- [ ] Có section `## Infrastructure Design`
- [ ] Có section `## Components and Interfaces`
- [ ] Có section `## Correctness Properties` với ít nhất 1 property

### Requirements Coverage
- [ ] Mỗi requirement trong requirements.md có ít nhất 1 component/section address nó
- [ ] Không có requirement bị bỏ sót (cross-check requirement list vs design)
- [ ] Correctness Properties reference đúng requirement numbers (`**Validates: Requirements X.X**`)

### Architecture Quality
- [ ] Mermaid diagrams render được (syntax đúng)
- [ ] Sequence diagrams cover main flows (happy path + error)
- [ ] Không có component "mồ côi" (không kết nối với gì)
- [ ] Data flow rõ ràng (biết data đi từ đâu đến đâu)

### Database Design
- [ ] Mỗi table có Primary Key
- [ ] Foreign Keys được define rõ ràng
- [ ] Không có field trùng tên giữa các tables mà không phải FK
- [ ] Có index strategy cho fields thường query

### Security (basic)
- [ ] Không lưu password dạng plaintext trong schema
- [ ] Sensitive fields được đánh dấu (nullable, encrypted...)
- [ ] Auth/authorization flow được mô tả

### GUI Prototype
- [ ] Folder gui/ tồn tại và có ít nhất index.html
- [ ] HTML files có navigation giữa các màn hình
- [ ] Responsive (có viewport meta tag hoặc CSS framework)

## Output format

```markdown
# Pre-Approve Check: Design

**File:** [path]
**Ngày check:** YYYY-MM-DD
**Kết quả:** X/Y items PASS

## Requirements Coverage Matrix

| Requirement | Covered by | Status |
|-------------|-----------|--------|
| Req 1: Login | Architecture (sequence), Components (AuthController) | ✅ |
| Req 2: Register | Components (AuthController) | ✅ |
| Req 3: Notifications | ❌ NOT COVERED | ❌ |

## Issues cần sửa trước khi approve

1. **Requirement 3** chưa được cover trong design
2. **Table login_attempts** thiếu index trên `user_id`

## Recommendation

- [ ] Sửa issues → chạy lại pre-approve check
- [ ] Sau khi PASS hết → đưa Human approve
```
