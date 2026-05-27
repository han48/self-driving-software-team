# Self-Driving Software Team – Quy trình AI-First, Spec-Driven

> **Con người phê duyệt và chịu trách nhiệm. AI định nghĩa, thực thi và báo cáo.**

Framework quản lý dự án toàn diện cho các team phần mềm chuyển đổi sang mô hình AI-first. Kết hợp RACI, RADIO reporting, Specification-driven development, và AI Agents thành một hệ thống thống nhất.

🌐 **Ngôn ngữ:** Tiếng Việt | [English](README.md) | [日本語](README.ja.md)

---

## Tổng quan

Framework này chuyển đổi quản lý dự án phần mềm từ:

```
"Quản lý con người làm việc" → "Quản lý hệ thống AI thực thi công việc"
```

**4 trụ cột:**

| Trụ cột | Chức năng |
|---------|-----------|
| **RACI** | Xác định rõ trách nhiệm giữa Human và AI |
| **RADIO** | Chuẩn hóa báo cáo, tự động hóa bằng AI |
| **Spec** | Nguồn sự thật duy nhất, input cho AI |
| **AI Agent** | Thực thi, validate, và báo cáo tự động |

---

## Cấu trúc tài liệu

```
📁 self-driving-software-team/
├── README.md                    ← README tiếng Anh
├── README.vi.md                 ← Bạn đang ở đây (Tiếng Việt)
├── README.ja.md                 ← README tiếng Nhật
├── 📁 vi/                       ← Tiếng Việt (bản gốc)
│   ├── self-driving-software-team.md   ← Tài liệu chính (2800+ dòng)
│   ├── quick-start.md
│   ├── toolchain.md
│   ├── prompt-templates.md
│   ├── 📁 steerings/           ← AI steering files (19 files)
│   └── 📁 skills/              ← AI skill files (7 files)
├── 📁 en/                       ← Tiếng Anh (bản dịch đầy đủ)
│   ├── self-driving-software-team.md
│   ├── quick-start.md
│   ├── toolchain.md
│   ├── prompt-templates.md
│   ├── 📁 steerings/           ← AI steering files (19 files)
│   └── 📁 skills/              ← AI skill files (7 files)
└── 📁 ja/                       ← Tiếng Nhật (bản dịch đầy đủ)
    ├── self-driving-software-team.md
    ├── quick-start.md
    ├── toolchain.md
    ├── prompt-templates.md
    ├── 📁 steerings/           ← AI steering files (19 files)
    └── 📁 skills/              ← AI skill files (7 files)
```

---

## Bắt đầu nhanh

1. Đọc [`vi/quick-start.md`](vi/quick-start.md) (15 phút)
2. Setup toolchain: [`vi/toolchain.md`](vi/toolchain.md)
3. Dùng prompt templates: [`vi/prompt-templates.md`](vi/prompt-templates.md)
4. Đọc chi tiết đầy đủ: [`vi/self-driving-software-team.md`](vi/self-driving-software-team.md)

---

## Tính năng chính

- **Spec-Driven:** Mọi feature bắt đầu bằng specification chính thức (EARS notation)
- **AI thực thi:** AI sinh spec, design, tasks, code, test, và report
- **Human approve:** Con người review và approve ở mọi gate
- **RADIO Reporting:** Báo cáo tiến độ tự động dựa trên data thực thi
- **Risk Management:** AI tự detect risk/issue với 5WHY analysis
- **CR Management:** Lifecycle CR có cấu trúc với backlog tracking
- **Q&A Management:** Q&A theo thread cho spec clarification
- **Definition of Done:** DoD rõ ràng cho mọi artifact và phase
- **Adoption Levels:** Lộ trình 4 level từ cơ bản đến AI full autonomy

---

## Dành cho ai?

| Vai trò | Cách sử dụng |
|---------|-------------|
| **Project Manager** | Governance, KPIs, escalation, CR approval |
| **Tech Lead** | Approve spec, design, PR; quyết định kỹ thuật |
| **BA** | Cung cấp yêu cầu, approve business logic |
| **QA/Tester** | Manual testing (L5), review test coverage |
| **Developer** | Tư vấn kỹ thuật, override AI khi cần |
| **AI Agent** | Thực thi toàn bộ (spec → code → test → report) |

---

## Phiên bản

**Hiện tại:** v1.2 (2026-05-21)

| Version | Ngày | Thay đổi |
|---------|------|----------|
| 1.0 | 2026-05-21 | Phiên bản đầu tiên |

---

## Tác giả

**DungDV (Mr4)** – Sabitech

---

## License

Sử dụng nội bộ. Liên hệ tác giả để phân phối.
