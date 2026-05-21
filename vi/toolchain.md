# Toolchain & Công cụ

Danh sách công cụ khuyến nghị cho mô hình AI-First, Spec-Driven.

---

## AI Agent Layer

| Thành phần | Công cụ khuyến nghị | Thay thế |
|------------|-------------------|----------|
| AI IDE | **Kiro** (AWS) | Cursor, Windsurf |
| LLM Backend | Claude (Anthropic) | GPT-4o, Gemini |
| Spec Management | **OpenSpec** (file-based trong Git) | Custom markdown |
| MCP Integration | Kiro MCP servers | Custom MCP |

---

## CI/CD & Testing

| Thành phần | Công cụ khuyến nghị | Thay thế |
|------------|-------------------|----------|
| CI/CD | GitHub Actions | GitLab CI, Jenkins, Azure DevOps |
| Unit Test | Jest / PHPUnit / pytest | Vitest, Mocha |
| E2E Test | Playwright | Selenium, Cypress |
| Desktop Test | RobotFramework | AutoIt |
| Browser Automation | browser-use | Puppeteer |
| SAST/Security | SonarQube + Snyk | CodeQL, Semgrep |
| Coverage | Istanbul / Coverage.py | Built-in framework |
| Property-Based Test | fast-check / Hypothesis | — |

---

## Communication & Monitoring

| Thành phần | Công cụ khuyến nghị | Thay thế |
|------------|-------------------|----------|
| Notification | Slack / MS Teams | Discord, Email |
| Dashboard | Grafana + custom | Datadog, New Relic |
| Git Hosting | GitHub | GitLab, Bitbucket |
| PR Review | GitHub PR | GitLab MR |

---

## Integrate AI vào CI/CD Pipeline

```yaml
# Ví dụ GitHub Actions workflow
name: AI-First Pipeline
on: [push, pull_request]

jobs:
  ai-validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run AI spec compliance check
        run: |
          # AI validate code against spec
          npm run ai:validate-spec
      - name: Run all test levels (L1-L4)
        run: |
          npm run test:unit          # L1
          npm run test:integration   # L2
          npm run test:e2e           # L3
      - name: Security scan (SAST)
        run: npm run security:scan
      - name: Generate RADIO report
        run: npm run ai:radio-report
```

---

## Quy tắc chọn tool

1. **Ưu tiên file-based:** Tool output file (Git-friendly) > tool cần API
2. **AI-readable:** Tool phải output format AI đọc được (JSON, Markdown, YAML)
3. **CI-compatible:** Phải chạy được trong CI/CD pipeline (headless)
4. **Team familiar:** Ưu tiên tool team đã biết > tool mới tốt hơn
5. **Cost-effective:** Open source first, paid tool khi cần scale

---

## Mapping Tool → Quy trình

| Phase | Tool chính | Output |
|-------|-----------|--------|
| Spec sinh | Kiro + Claude | `requirements.md` |
| Design sinh | Kiro + Claude | `design.md` + `gui/*.html` |
| Tasks sinh | Kiro + Claude | `tasks.md` |
| Code sinh | Kiro + Claude | Source code |
| Test sinh | Kiro + Claude + Jest/Playwright | Test files |
| Test chạy | GitHub Actions + Jest/Playwright | CI report |
| Security scan | SonarQube + Snyk | Vulnerability report |
| RADIO sinh | Kiro + Claude | `radio.md` |
| Notification | Slack/Teams webhook | Alert messages |
| Dashboard | Grafana | Metrics visualization |
