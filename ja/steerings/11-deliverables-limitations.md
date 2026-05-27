---
inclusion: fileMatch
fileMatchPattern: '**/deliverables/**'
---

# Steering: 成果物＆制限事項管理

## 適用タイミング
スプリント/フェーズ/リリース終了時またはプロジェクト状態の統合が必要な時。

## ルール

1. **AIが自動統合:** tasks.md、テスト結果、RADIOをすべてスキャンして成果物を生成

2. **フォルダ構成:**
   ```
   openspec/deliverables/
   ├── index.md                ← 全デリバリー回の一覧表
   ├── sprint-XX/deliverables.md   ← スプリント別
   ├── phase-X/deliverables.md     ← フェーズ別 (複数sprint統合)
   └── YYYY-MM-DD/deliverables.md  ← 日付別 (hotfix、urgent)
   ```

3. **index.mdフォーマット:**
   ```markdown
   # Deliverables Index
   
   | 回 | 日付 | デリバリー済みFeatures | 制限事項 | リンク |
   |-----|------|-------------------|-------------|------|
   | Sprint 3 | 2025-03-28 | Login, Register | 2 LIM | `sprint-03/deliverables.md` |
   ```

4. **Deliverableエントリ:**
   ```
   ### Feature: [名前]
   - **Spec:** `openspec/specs/{feature-name}/`
   - **Status:** ✅ Complete
   - **Test coverage:** XX%
   - **Spec compliance:** XX%
   - **Deployed:** [environment]
   - **説明:** [要約]
   ```

5. **Limitationエントリ:**
   ```
   ### LIM-XXX: [名前]
   - **種類:** Not Implemented / Partial / Technical Constraint / Known Issue
   - **関連:** [Spec/CR reference]
   - **説明:** [詳細]
   - **理由:** [なぜ]
   - **影響:** [Impact]
   - **計画:** Phase 2 / Backlog / Won't Fix
   - **Workaround:** [ある場合]
   ```

6. **Limitationsの分類:**
   - Not Implemented: requirementはあるが未実装
   - Partial: 実装済みだが不完全
   - Technical Constraint: 技術的制約
   - Known Issue: 既知のバグで未修正

7. **デリバリー回ごとに分離:**
   - Sprint: 各sprintに1ファイル
   - Phase: 複数sprintsの統合
   - Hotfix/urgent: 日付別 (YYYY-MM-DD)

8. **index.mdを更新** – 新しいデリバリー回がある度に

9. **Human確認:** すべてのlimitationは顧客に送付前にHumanの確認が必要

10. **サマリー必須:** 各deliverables.mdファイル末尾にメトリクス集計テーブル
