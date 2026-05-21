# Toolchain & Tools

Recommended tools for the AI-First, Spec-Driven model.

---

## AI Agent Layer

| Component | Recommended Tool | Alternative |
|-----------|-----------------|-------------|
| AI IDE | **Kiro** (AWS) | Cursor, Windsurf |
| LLM Backend | Claude (Anthropic) | GPT-4o, Gemini |
| Spec Management | **OpenSpec** (file-based in Git) | Custom markdown |
| MCP Integration | Kiro MCP servers | Custom MCP |

---

## CI/CD & Testing

| Component | Recommended Tool | Alternative |
|-----------|-----------------|-------------|
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

| Component | Recommended Tool | Alternative |
|-----------|-----------------|-------------|
| Notification | Slack / MS Teams | Discord, Email |
| Dashboard | Grafana + custom | Datadog, New Relic |
| Git Hosting | GitHub | GitLab, Bitbucket |
| PR Review | GitHub PR | GitLab MR |

---

## Integrating AI into CI/CD Pipeline

```yaml
# GitHub Actions workflow example
name: AI-First Pipeline
on: [push, pull_request]

jobs:
  ai-validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run AI spec compliance check
        run: |
          # AI validates code against spec
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

## Tool Selection Rules

1. **Prefer file-based:** Tools that output files (Git-friendly) > tools requiring APIs
2. **AI-readable:** Tools must output AI-readable formats (JSON, Markdown, YAML)
3. **CI-compatible:** Must run in CI/CD pipeline (headless)
4. **Team familiar:** Prefer tools the team already knows > better but unfamiliar tools
5. **Cost-effective:** Open source first, paid tools when scaling requires it

---

## Tool → Process Mapping

| Phase | Primary Tool | Output |
|-------|-------------|--------|
| Spec generation | Kiro + Claude | `requirements.md` |
| Design generation | Kiro + Claude | `design.md` + `gui/*.html` |
| Tasks generation | Kiro + Claude | `tasks.md` |
| Code generation | Kiro + Claude | Source code |
| Test generation | Kiro + Claude + Jest/Playwright | Test files |
| Test execution | GitHub Actions + Jest/Playwright | CI report |
| Security scan | SonarQube + Snyk | Vulnerability report |
| RADIO generation | Kiro + Claude | `radio.md` |
| Notification | Slack/Teams webhook | Alert messages |
| Dashboard | Grafana | Metrics visualization |
