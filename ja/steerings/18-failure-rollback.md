---
inclusion: auto
---

# Steering: 失敗処理＆ロールバック

## 適用タイミング
AI出力が間違っている時、テストが連続失敗する時、変更のロールバックが必要な時。

## リトライポリシー

```
1回目: AIがエラーメッセージ/フィードバックに基づき自己修正
2回目: AIが別のアプローチを試行 (異なるalgorithm/architecture)
3回目: AIが試行内容 + root causeをまとめ → Humanにescalate
```

**3回失敗後 → AIは必ず:**
1. 停止する
2. レポート生成: "3 attempts failed, root cause: [X], suggested approach: [Y]"
3. タスクをHuman override (Developer role) に移管
4. RADIO "D"に記録 + 必要に応じてISSUE-XXXを作成

## 状況別の対応

| 状況 | アクション | 決定者 |
|-----------|----------|---------------|
| Specのロジック誤り | AIが自己修正 (ループ) → それでも誤り → Human request changes | Human |
| Designが実現不可能 | AIが再設計 → 2回失敗 → escalate Tech Lead | Tech Lead |
| Codeがテスト失敗 | AIが修正 → 3回retry → escalate | Tech Lead |
| Codeがテストパスだがロジック誤り | HumanがPR reject → AIがspec再読 → 修正 | Human |
| AIが要件を理解できない | AIがQA作成 → clarification待ち → retry | Human |

## ロールバック戦略

| スコープ | ロールバック方法 | ツール |
|-------|--------------|------|
| Code変更の誤り | `git revert` commit | Git |
| Spec変更の誤り | 前バージョンにrevert | Git |
| Designの誤り | Revert + specから再設計 | Git |
| Feature方向性の誤り | Revert branch、specフェーズに戻る | Git branch |
| Production issue | Deployment rollback → hotfix | CI/CD |

## ルール

1. **履歴を削除しない:** `git revert`を使用、`git push --force`は使わない
2. **ロールバック理由を記録:** commit message + RADIO "D"に記載
3. **Post-mortem:** ロールバック後 → AIが5WHY analysisを生成
4. **Prevention:** 各ロールバック → 改善 (テスト追加、チェック追加)
5. **無限リトライ禁止:** 最大3回、その後escalate
6. **Human override:** AI失敗時 → Developerが直接コーディングする権限あり
