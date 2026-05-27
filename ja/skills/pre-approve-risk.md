---
inclusion: manual
---

# Skill: Pre-Approve Check – Risk

## 目的
リスクエントリを Human に確認提示する前に検証する。

## 入力
`openspec/risk-issue/risks.md` のリスクエントリ。

## 自動チェックリスト

### フォーマット
- [ ] 見出しが `### RISK-XXX: [Name]` である
- [ ] `**Date detected:**` がある（形式 YYYY-MM-DD）
- [ ] `**Probability:**` がある（High/Medium/Low）
- [ ] `**Impact:**` がある（High/Medium/Low）
- [ ] `**Severity:**` がある（Critical/High/Medium/Low）
- [ ] `**Description:**` がある（空でない）
- [ ] `**Prevention action:**` がある（具体的、実行可能）
- [ ] `**Mitigation action:**` がある（具体的、実行可能）
- [ ] `**Owner:**` がある
- [ ] `**Trigger:**` がある
- [ ] `**Status:**` がある

### 品質
- [ ] Prevention ≠ Mitigation（2つの異なるアクション）
- [ ] Prevention は発生確率を下げる（結果ではない）
- [ ] Mitigation は発生した場合の結果を軽減する（確率ではない）
- [ ] Trigger は観察可能な兆候である（"発生した時" ではない）
- [ ] Severity = Probability × Impact（ロジックが正しい）
- [ ] 既存の RISK と重複していない

### トレーサビリティ
- [ ] 仕様に関連する場合 → 参照がある
- [ ] CR に関連する場合 → 参照がある

## 出力フォーマット

```markdown
# Pre-Approve Check: Risk

**Risk:** RISK-XXX
**結果:** X/Y 項目 PASS

## 検証

| チェック | ステータス | 備考 |
|---------|--------|------|
| Prevention が実行可能 | ✅ | "Add circuit breaker" – 具体的 |
| Mitigation が実行可能 | ✅ | "Fallback to cache" – 具体的 |
| Prevention ≠ Mitigation | ✅ | 異なる |
| Severity ロジック | ❌ | High × Low = Medium だが Critical と記載 |

## 問題

1. **Severity:** High × Low は Medium であるべきだが、Critical と記載されている

## 推奨事項

- [ ] Severity を修正 → Human に確認を提示
```
