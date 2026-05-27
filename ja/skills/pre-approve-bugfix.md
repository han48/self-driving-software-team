---
inclusion: manual
---

# Skill: Pre-Approve Check – Bugfix

## 目的
バグ修正仕様を Human に承認提示する前に、自動チェックリスト検証を実行する。

## 入力
バグ修正フォルダ内のファイル: `bugfix.md` + `design.md` + `tasks.md`

## 自動チェックリスト

### bugfix.md
- [ ] 見出しが `# Bugfix Requirements Document` である
- [ ] `## Introduction` がある（空でない）
- [ ] `## Bug Analysis` があり3つのサブセクションがある
- [ ] `### Current Behavior (Defect)` に少なくとも1つのステートメントがある（1.x）
- [ ] `### Expected Behavior (Correct)` に少なくとも1つのステートメントがある（2.x）
- [ ] `### Unchanged Behavior (Regression Prevention)` に少なくとも1つのステートメントがある（3.x）
- [ ] Defect ステートメントが "THEN the system"（小文字）を使用している
- [ ] Correct ステートメントが "THE SYSTEM SHALL"（大文字）を使用している
- [ ] Unchanged ステートメントが "THE SYSTEM SHALL CONTINUE TO" を使用している
- [ ] 番号付けが正しい: 1.x, 2.x, 3.x

### design.md
- [ ] `## Glossary` がある
- [ ] `## Bug Details` があり影響を受けるコンポーネントが記載されている
- [ ] `## Hypothesized Root Cause` がある（単なる説明ではなく WHY を説明している）
- [ ] `## Fix Implementation` があり具体的なコードがある
- [ ] `## Correctness Properties` がある（少なくとも2つ: valid passes + invalid fails）
- [ ] `## Testing Strategy` がある
- [ ] `## Files Changed` がある（テーブル形式のリスト）
- [ ] Files Changed が必要なファイルのみ含んでいる（外科的修正）

### tasks.md
- [ ] 参照が `_Bugfix: X.X_` を使用している（`_Requirements:_` ではない）
- [ ] バグを再現するタスクがある（修正前にテストが FAIL する）
- [ ] 修正を実装するタスクがある
- [ ] 修正を検証するタスクがある（修正後にテストが PASS する）
- [ ] リグレッションテストのタスクがある
- [ ] `## Task Dependency Graph` がある

### クロスチェック
- [ ] bugfix.md の Unchanged Behavior → tasks.md にリグレッションテストがある
- [ ] design.md の Files Changed → タスクがそれらのファイルのみを変更する
- [ ] Correctness Properties → 対応するプロパティテストタスクがある

## 出力フォーマット

```markdown
# Pre-Approve Check: Bugfix

**Bug:** [name]
**チェック日:** YYYY-MM-DD
**結果:** X/Y 項目 PASS

## 外科的修正の検証

| チェック | ステータス |
|---------|--------|
| Files Changed ≤ 5 ファイル | ✅ |
| Unchanged Behavior が記載されている | ✅ (3 項目) |
| リグレッションテストが Unchanged をカバー | ✅ |
| 修正が最小限 | ✅ |

## 承認前に修正すべき問題

1. **Unchanged Behavior 3.2** に対応するリグレッションテストが tasks.md にない

## 推奨事項

- [ ] 問題を修正 → pre-approve check を再実行
- [ ] すべて PASS 後 → Human に承認を提示
```
