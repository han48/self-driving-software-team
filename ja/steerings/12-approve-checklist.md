---
inclusion: manual
---

# Steering: 承認チェックリスト（クイックリファレンス）

## 適用タイミング
HumanがAIの出力を承認する必要がある時。AIはHumanに適切なチェックリストの使用をリマインドする必要がある。

## マッピング: Output → チェックリスト

| 承認対象のOutput | 適用チェックリスト |
|-------------------|-------------------|
| requirements.md | Checklist Approve Requirements |
| design.md | Checklist Approve Design |
| tasks.md | Checklist Approve Tasks |
| Pull Request (code) | Checklist Approve PR |
| RADIO report | Checklist Approve RADIO |
| CR (accept/reject) | Checklist Approve CR |
| Bugfix (bugfix.md + design.md) | Checklist Approve Bugfix |
| Risk (RISK-XXX) | Checklist Approve Risk |
| Issue resolution | Checklist Approve Issue Resolution |
| Q&A conclusion | Checklist Approve Q&A Conclusion |
| deliverables.md | Checklist Approve Deliverables & Limitations |

## ルール

1. **AIがHumanにリマインド:** outputをHumanに承認依頼する際、AIは明記する: 「承認前に[チェックリスト名]に従ってレビューしてください。」
2. **全項目パスで承認:** レビュアーはチェックリストの全項目をパスする必要あり
3. **パスしない場合:** 変更要求、AIが修正、再提出
4. **記録:** 誰が承認、いつ、どのroleで: "Approved by: [名前] (as [Role]) – YYYY-MM-DD"
5. **ローテーション:** 同一人物が同一featureで3つ以上連続してPRを承認しない

## 承認リマインドテンプレート

AIがoutputをHumanに提出する際のフォーマット:

```
✅ [Output type] がレビュー可能です。
📋 適用チェックリスト: [チェックリスト名]
👤 推奨レビュアー: [適切なRole]
```
