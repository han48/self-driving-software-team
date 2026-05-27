---
inclusion: manual
---

# Skill: Pre-Approve Check – Requirements (Spec)

## 目的
requirements.md を Human に承認提示する前に、自動チェックリスト検証を実行する。AI が先に問題を検出することで、Human のレビュー時間を短縮する。

## 入力
チェック対象のファイル `requirements.md`。

## 自動チェックリスト

requirements.md ファイルを読み込み、以下の各項目をチェックする。各項目に対して PASS/FAIL の結果を出力する：

### フォーマット＆構造
- [ ] 見出しが `# Requirements Document` である
- [ ] セクション `## Introduction` がある（空でない）
- [ ] セクション `## Glossary` がある（少なくとも1つの用語）
- [ ] `## Requirements` の前にセパレータ `---` がある
- [ ] 少なくとも1つの `### Requirement N:` セクションがある
- [ ] 各要件に `**User Story:**` があり、"As a... I want... so that..." 形式である
- [ ] 各要件に `#### Acceptance Criteria` があり、少なくとも1項目ある

### EARS 記法
- [ ] すべての AC が THE/WHEN/IF + SHALL パターンを使用している
- [ ] コンポーネント名が大文字である（THE API, THE System, THE Admin_Panel...）
- [ ] 各 AC に番号が付いている（1, 2, 3...）
- [ ] 曖昧な表現を使用している AC がない（"should", "might", "better"）

### ロジック＆完全性
- [ ] 矛盾する AC がない（同じ条件 → 異なる動作）
- [ ] エラーケース / エッジケースがある（ハッピーパスだけでない）
- [ ] 用語集が AC に出現するすべてのドメイン固有用語をカバーしている
- [ ] 隠れた前提がない（すべての前提が AC または Introduction に明示されている）

### テスト可能性
- [ ] 各 AC が具体的なテストケースに変換可能（明確な入力 + 期待出力がある）
- [ ] 汎用的すぎる AC がない（"システムは高速であること", "UI は美しいこと"）

### トレーサビリティ
- [ ] 関連するリスクがある場合 → RISK-XXX を参照している
- [ ] 関連する Q&A がある場合 → QA-XXX を参照しているか、Q&A が回答済みである

## 出力フォーマット

```markdown
# Pre-Approve Check: Requirements

**File:** [path]
**チェック日:** YYYY-MM-DD
**結果:** X/Y 項目 PASS

## 結果

| # | 項目 | ステータス | 備考 |
|---|------|--------|------|
| 1 | 見出しが正しいフォーマット | ✅ PASS | |
| 2 | Introduction が空でない | ✅ PASS | |
| 3 | EARS 記法が正しい | ❌ FAIL | AC 3.2 に SHALL がない |
| ... | ... | ... | ... |

## 承認前に修正すべき問題

1. **AC 3.2:** キーワード SHALL が欠落 – 現在 "system returns error" → "THE System SHALL return error" に修正
2. **Glossary:** 用語 "DashboardMessage" が AC に出現するが用語集にない

## 推奨事項

- [ ] 上記2件の問題を修正 → pre-approve check を再実行
- [ ] すべて PASS 後 → Human に承認を提示
```
