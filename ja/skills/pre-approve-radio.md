---
inclusion: manual
---

# Skill: Pre-Approve Check – RADIO Report

## 目的
RADIO レポートを Human にレビュー提示する前に検証する。レポートが主観ではなく実際のデータに基づいていることを確認する。

## 入力
RADIO レポート内容 + tasks.md のステータス + テスト結果。

## 自動チェックリスト

### フォーマット
- [ ] 5項目すべてがある: R, A, D, I, O
- [ ] 各項目が空でない

### 正確性（データとのクロスチェック）
- [ ] R (Review): ステータスが tasks.md のステータスと一致（X/Y タスク完了）
- [ ] A (Action): 進行中タスクが tasks.md と一致
- [ ] O (Outcome): メトリクスが CI/テスト結果と一致（カバレッジ %、テスト pass）
- [ ] 証拠のない主張がない（例: "100% pass" だが CI が実行されていない）

### 完全性
- [ ] D (Difficulty): テストが失敗している場合 → 言及必須
- [ ] D (Difficulty): RISK/ISSUE が Open の場合 → 言及必須
- [ ] D (Difficulty): QA が 3日以上 Open の場合 → 言及必須
- [ ] I (Information): 進捗に影響するコンテキストが記載されている
- [ ] O (Outcome): 少なくとも1つの具体的なメトリクスがある（数値、%）

### リスク/イシュー統合
- [ ] 各重大な D 項目 → 対応する RISK-XXX または ISSUE-XXX がある
- [ ] リスク/イシューのない新しい D 項目 → 作成が必要とフラグ

## 出力フォーマット

```markdown
# Pre-Approve Check: RADIO Report

**チェック日:** YYYY-MM-DD
**結果:** X/Y 項目 PASS

## データ検証

| RADIO 項目 | 主張 | 証拠 | 一致? |
|-----------|------|------|--------|
| R: 8/10 タスク完了 | tasks.md | 8 [x] / 10 total | ✅ |
| O: カバレッジ 87% | CI レポート | 87.2% | ✅ |
| D: "ブロッカーなし" | risk-issue/ | ISSUE-002 Open | ❌ 不一致 |

## 問題

1. **D 項目が不完全:** ISSUE-002 が Open だが RADIO は "ブロッカーなし" と記載

## 推奨事項

- [ ] D 項目を ISSUE-002 を反映するよう修正
- [ ] PASS 後 → Human にレビューを提示
```
