---
inclusion: auto
---

# Steering: ツールチェーン＆ツール

## 適用タイミング
常に – 各部分にどのツールを使うか参照する時。

> 📖 詳細: [`toolchain.md`](../toolchain.md)を参照

## クイックサマリー

| フェーズ | 主要ツール | Output |
|-------|-----------|--------|
| Spec/Design/Tasks | Kiro + Claude | Markdown files |
| Code + Test | Kiro + Claude | Source + test files |
| CI/CD | GitHub Actions | CI report |
| E2E | Playwright / browser-use | Test results |
| Security | SonarQube + Snyk | Vulnerability report |
| RADIO | Kiro + Claude | `radio.md` |
| Notification | Slack/Teams | Alerts |
| Dashboard | Grafana | Metrics |

## ツール選定ルール

1. **ファイルベース優先:** ファイル出力ツール (Git-friendly) > API必要なツール
2. **AI可読:** AI読み取り可能な出力形式 (JSON、Markdown、YAML)
3. **CI互換:** CI/CDでheadless実行可能
4. **チーム習熟:** チームが既に知っているツールを優先
5. **コスト効率:** オープンソース優先
