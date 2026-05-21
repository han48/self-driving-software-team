---
inclusion: auto
---

# Steering: Toolchain & Công cụ

## Khi nào áp dụng
Luôn luôn – reference khi cần biết dùng tool gì cho từng phần.

> 📖 Chi tiết đầy đủ: xem file [`toolchain.md`](../toolchain.md)

## Tóm tắt nhanh

| Phase | Tool chính | Output |
|-------|-----------|--------|
| Spec/Design/Tasks | Kiro + Claude | Markdown files |
| Code + Test | Kiro + Claude | Source + test files |
| CI/CD | GitHub Actions | CI report |
| E2E | Playwright / browser-use | Test results |
| Security | SonarQube + Snyk | Vulnerability report |
| RADIO | Kiro + Claude | `radio.md` |
| Notification | Slack/Teams | Alerts |
| Dashboard | Grafana | Metrics |

## Quy tắc chọn tool

1. **Ưu tiên file-based:** Tool output file (Git-friendly) > tool cần API
2. **AI-readable:** Output format AI đọc được (JSON, Markdown, YAML)
3. **CI-compatible:** Chạy được headless trong CI/CD
4. **Team familiar:** Ưu tiên tool team đã biết
5. **Cost-effective:** Open source first
