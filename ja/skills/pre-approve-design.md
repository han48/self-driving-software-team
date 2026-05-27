---
inclusion: manual
---

# Skill: Pre-Approve Check – Design

## 目的
design.md を Human に承認提示する前に、自動チェックリスト検証を実行する。

## 入力
チェック対象のファイル `design.md` + `requirements.md`（承認済み）。

## 自動チェックリスト

### フォーマット＆構造
- [ ] 見出しが `# Design Document: [Name]` である
- [ ] セクション `## Overview` がある（空でない）
- [ ] セクション `## Site Map` がある
- [ ] セクション `## GUI Design` があり、gui/ フォルダへの参照がある
- [ ] セクション `## Architecture` があり、少なくとも1つの Mermaid 図がある
- [ ] セクション `## Data Models / Database Design + ERD` がある
- [ ] セクション `## Infrastructure Design` がある
- [ ] セクション `## Components and Interfaces` がある
- [ ] セクション `## Correctness Properties` があり、少なくとも1つのプロパティがある

### 要件カバレッジ
- [ ] requirements.md の各要件に対して、少なくとも1つのコンポーネント/セクションが対応している
- [ ] 漏れている要件がない（要件リストとデザインをクロスチェック）
- [ ] Correctness Properties が正しい要件番号を参照している（`**Validates: Requirements X.X**`）

### アーキテクチャ品質
- [ ] Mermaid 図が正しくレンダリングされる（有効な構文）
- [ ] シーケンス図が主要フローをカバーしている（ハッピーパス + エラー）
- [ ] 「孤立」コンポーネントがない（何にも接続されていない）
- [ ] データフローが明確（データの入出力先がわかる）

### データベース設計
- [ ] 各テーブルに主キーがある
- [ ] 外部キーが明確に定義されている
- [ ] FK でないテーブル間で重複するフィールド名がない
- [ ] 頻繁にクエリされるフィールドにインデックス戦略がある

### セキュリティ（基本）
- [ ] スキーマに平文パスワード保存がない
- [ ] 機密フィールドがマークされている（nullable, encrypted...）
- [ ] 認証/認可フローが記述されている

### GUI プロトタイプ
- [ ] gui/ フォルダが存在し、少なくとも index.html がある
- [ ] HTML ファイル間にナビゲーションがある
- [ ] レスポンシブ対応（viewport meta タグまたは CSS フレームワークがある）

## 出力フォーマット

```markdown
# Pre-Approve Check: Design

**File:** [path]
**チェック日:** YYYY-MM-DD
**結果:** X/Y 項目 PASS

## 要件カバレッジマトリクス

| 要件 | カバー元 | ステータス |
|------|---------|--------|
| Req 1: Login | Architecture (sequence), Components (AuthController) | ✅ |
| Req 2: Register | Components (AuthController) | ✅ |
| Req 3: Notifications | ❌ カバーされていない | ❌ |

## 承認前に修正すべき問題

1. **Requirement 3** がデザインでカバーされていない
2. **テーブル login_attempts** に `user_id` のインデックスがない

## 推奨事項

- [ ] 問題を修正 → pre-approve check を再実行
- [ ] すべて PASS 後 → Human に承認を提示
```
