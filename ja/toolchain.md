# ツールチェーン＆ツール

AI-First、Spec-Drivenモデルの推奨ツール一覧。

---

## AIエージェントレイヤー

| コンポーネント | 推奨ツール | 代替 |
|------------|----------|------|
| AI IDE | **Kiro** (AWS) | Cursor, Windsurf |
| LLMバックエンド | Claude (Anthropic) | GPT-4o, Gemini |
| Spec管理 | **OpenSpec**（Git内ファイルベース） | カスタムmarkdown |
| MCP連携 | Kiro MCPサーバー | カスタムMCP |

---

## CI/CD＆テスト

| コンポーネント | 推奨ツール | 代替 |
|------------|----------|------|
| CI/CD | GitHub Actions | GitLab CI, Jenkins, Azure DevOps |
| ユニットテスト | Jest / PHPUnit / pytest | Vitest, Mocha |
| E2Eテスト | Playwright | Selenium, Cypress |
| デスクトップテスト | RobotFramework | AutoIt |
| ブラウザ自動化 | browser-use | Puppeteer |
| SAST/セキュリティ | SonarQube + Snyk | CodeQL, Semgrep |
| カバレッジ | Istanbul / Coverage.py | 組み込みフレームワーク |
| プロパティベーステスト | fast-check / Hypothesis | — |

---

## コミュニケーション＆モニタリング

| コンポーネント | 推奨ツール | 代替 |
|------------|----------|------|
| 通知 | Slack / MS Teams | Discord, Email |
| ダッシュボード | Grafana + カスタム | Datadog, New Relic |
| Gitホスティング | GitHub | GitLab, Bitbucket |
| PRレビュー | GitHub PR | GitLab MR |

---

## AIをCI/CDパイプラインに統合

```yaml
# GitHub Actionsワークフロー例
name: AI-First Pipeline
on: [push, pull_request]

jobs:
  ai-validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: AI Specコンプライアンスチェック
        run: |
          # AIがコードをSpecに対して検証
          npm run ai:validate-spec
      - name: 全テストレベル実行 (L1-L4)
        run: |
          npm run test:unit          # L1
          npm run test:integration   # L2
          npm run test:e2e           # L3
      - name: セキュリティスキャン (SAST)
        run: npm run security:scan
      - name: RADIOレポート生成
        run: npm run ai:radio-report
```

---

## ツール選定ルール

1. **ファイルベース優先：** ファイル出力ツール（Git対応）> API必要ツール
2. **AI読み取り可能：** AI読み取り可能フォーマット出力必須（JSON, Markdown, YAML）
3. **CI互換：** CI/CDパイプラインで実行可能（ヘッドレス）
4. **チーム習熟：** チームが既知のツール優先 > より良いが未知のツール
5. **コスト効率：** オープンソース優先、スケール時に有料ツール

---

## ツール → プロセスマッピング

| フェーズ | 主要ツール | 出力 |
|---------|----------|------|
| Spec生成 | Kiro + Claude | `requirements.md` |
| Design生成 | Kiro + Claude | `design.md` + `gui/*.html` |
| Tasks生成 | Kiro + Claude | `tasks.md` |
| コード生成 | Kiro + Claude | ソースコード |
| テスト生成 | Kiro + Claude + Jest/Playwright | テストファイル |
| テスト実行 | GitHub Actions + Jest/Playwright | CIレポート |
| セキュリティスキャン | SonarQube + Snyk | 脆弱性レポート |
| RADIO生成 | Kiro + Claude | `radio.md` |
| 通知 | Slack/Teams webhook | アラートメッセージ |
| ダッシュボード | Grafana | メトリクス可視化 |
