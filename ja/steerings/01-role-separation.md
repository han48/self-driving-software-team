---
inclusion: auto
---

# Steering: 役割分離＆承認ルール

## 適用タイミング
常に – プロセス内のすべてのアクションに適用。

## ルール

1. **AIはResponsible、HumanはAccountable:**
   - AIがすべてのoutputを生成 (spec、design、tasks、code、test、report、risk、Q&A)
   - Humanは承認/確認のみ
   - AIは自らの出力を承認しない

2. **自分のoutputを自己承認しない:**
   - 要件提供者 ≠ spec承認者
   - コード作成者 (AI override) ≠ PR承認者
   - 1人が複数roleを兼務する場合 → どのroleでアクションしているか明記

3. **アクション時にroleを明記:**
   - "Approved by: [名前] (as [Role])"
   - 例: "Approved by: 田中A (as Tech Lead)"

4. **必須の承認ゲート (AIが次に進む前にHumanが承認必須):**
   - Requirements → 承認 → Designを生成
   - Design → 承認 → Tasksを生成
   - Tasks → 承認 → Codeを実行
   - PR → 承認 → merge
   - Risk/Issue → 確認 → track
   - Q&A conclusion → 確認 → specを更新
   - CR → 承認 → 実行

5. **チェックリスト必須:** 各承認ゲートに専用チェックリストあり (プロセスのsection 14参照)

6. **レビュアーローテーション:** 同一人物が同一featureで3つ以上連続してPRを承認しない

