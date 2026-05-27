---
inclusion: fileMatch
fileMatchPattern: '**/requirements.md,**/design.md,**/bugfix.md'
---

# Steering: スペックバージョニング戦略

## 適用タイミング
スペックファイルを作成または更新する時 (requirements.md、design.md、bugfix.md)。

## バージョンルール

| 変更種類 | Version | 再承認必要？ | 例 |
|---------------|---------|-----------------|-------|
| **Major** (scope/logic) | v1.0 → v2.0 | ✅ 必須 | requirement追加、architecture変更 |
| **Minor** (補足) | v1.0 → v1.1 | ✅ 必須 | edge case追加、AC明確化 |
| **Patch** (typo) | v1.0 → v1.0.1 | ❌ 不要 | typo修正、フォーマット |

## 各specファイルの必須ヘッダー

```markdown
**Version:** X.Y  
**Last updated:** YYYY-MM-DD  
**Updated by:** [AI Agent / Human name]  
**Approved by:** [Role] – YYYY-MM-DD  
```

## 必須Change Log

```markdown
## Change Log

| Version | 日付 | 変更内容 | Approved by | Trigger |
|---------|------|----------|-------------|---------|
| 1.2 | 2025-03-20 | Rate limiting AC追加 | Tech Lead | CR-005 |
| 1.1 | 2025-03-15 | Lock duration明確化 | Tech Lead | QA-003 |
| 1.0 | 2025-03-10 | 初版 | Tech Lead | — |
```

## ルール

1. **各specファイルにversion** – headerメタデータに記載
2. **Change Log必須** – 各変更ごとに
3. **マイルストーンにGit tag:** major承認時に`spec/{feature}-v{X.Y}`
4. **承認済みspecを無断変更しない** – 再承認なしでは変更不可 (patchを除く)
5. **Q&A → minor version:** conclusionがspec変更につながる場合
6. **CR → major version:** 大規模CRは新しいmajor versionを作成
7. **トレーサビリティ:** 各変更にtriggerを参照 (QA-XXX、CR-XXX、RISK-XXX)
8. **履歴を削除しない:** change logに追記、古いエントリを削除しない
