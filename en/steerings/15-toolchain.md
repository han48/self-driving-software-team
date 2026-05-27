---
inclusion: auto
---

# Steering: Toolchain & Tools

## When to Apply
Always – reference when needing to know which tool to use for each part.

> 📖 Full details: see file [`toolchain.md`](../toolchain.md)

## Quick Summary

| Phase | Primary Tool | Output |
|-------|-------------|--------|
| Spec/Design/Tasks | Kiro + Claude | Markdown files |
| Code + Test | Kiro + Claude | Source + test files |
| CI/CD | GitHub Actions | CI report |
| E2E | Playwright / browser-use | Test results |
| Security | SonarQube + Snyk | Vulnerability report |
| RADIO | Kiro + Claude | `radio.md` |
| Notification | Slack/Teams | Alerts |
| Dashboard | Grafana | Metrics |

## Tool Selection Rules

1. **Prefer file-based:** Tool outputs files (Git-friendly) > tools requiring API
2. **AI-readable:** Output format readable by AI (JSON, Markdown, YAML)
3. **CI-compatible:** Must run headless in CI/CD
4. **Team familiar:** Prefer tools the team already knows
5. **Cost-effective:** Open source first
