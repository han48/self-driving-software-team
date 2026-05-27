---
inclusion: fileMatch
fileMatchPattern: '**/bugfix.md'
---

# Steering: バグ修正スペックの作成

## 適用タイミング
AIが根本原因分析とリグレッション防止が必要な複雑なバグを処理する時。

## ルール

1. **bugfix.md – 必須フォーマット:**
   - Heading: `# Bugfix Requirements Document`
   - Sections: `## Introduction` → `## Bug Analysis`
   - Bug Analysisには3つのsub-sections:
     - `### Current Behavior (Defect)` – WHEN...THEN the system [不正]
     - `### Expected Behavior (Correct)` – WHEN...THE SYSTEM SHALL [正常]
     - `### Unchanged Behavior (Regression Prevention)` – WHEN...THE SYSTEM SHALL CONTINUE TO [維持]
   - 番号付け: 1.x (Defect)、2.x (Correct)、3.x (Unchanged)

2. **design.md – 必須フォーマット:**
   - Sections: Overview → Glossary → Bug Details → Expected Behavior → Hypothesized Root Cause (5WHY) → Fix Implementation → Correctness Properties → Testing Strategy → Files Changed
   - `## Hypothesized Root Cause (5WHY)` は5WHY手法を使用:
     - 連続する5レベルの"Why"
     - Root causeの結論
     - Prevention (再発防止)
   - `## Files Changed` は変更ファイルの一覧表 (New/Modified)
   - Correctness Propertiesに含む: valid passes、invalid fails、no side effects

3. **tasks.md – 必須フォーマット:**
   - 参照は `_Bugfix: X.X_` を使用 (`_Requirements:_` ではない)
   - フロー: Fix code → Apply → Unit tests → Feature tests
   - Task Dependency Graph必須

4. **surgical fixの原則:**
   - 最小限の変更
   - Unchanged Behaviorを明確にリストアップ必須
   - Files Changedで変更許可ファイルを明確にリストアップ必須

5. **出力場所:** `openspec/bugs/{bug-name}/`

