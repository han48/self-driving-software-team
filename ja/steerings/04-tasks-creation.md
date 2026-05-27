---
inclusion: fileMatch
fileMatchPattern: '**/tasks.md'
---

# Steering: タスク（実装計画）の作成

## 適用タイミング
AIが承認済みデザインからtasks.mdを生成する時。

## ルール

1. **必須フォーマット:**
   - Heading: `# Implementation Plan: [Feature名]`
   - Sections: `## Overview` → `## Tasks` → `## Task Dependency Graph` → `## Notes`
   - Tasksはネストされたチェックボックス: `- [ ] 1.` → `- [ ] 1.1`
   - 各sub-taskの末尾: `_Requirements: X.X, Y.Y_`

2. **タスク粒度:**
   - 各タスク < 4時間の作業量
   - 各タスクに検証可能な期待結果あり
   - テスト用タスクを含む (unit、integration、E2E)

3. **Dependency GraphはJSON waves形式:**
   ```json
   {
     "waves": [
       {"tasks": ["1"]},
       {"tasks": ["2", "3"]},
       {"tasks": ["4"]}
     ]
   }
   ```
   - 同一wave内のタスクは並列実行
   - 次のwaveは前のwaveの完了を待つ

4. **Property testタスク:**
   - フォーマット: `- [ ] N.M Property testを記述 — Property P`
   - 内部: `**Property P: [名前]**` + `**Validates: Requirements X.X**`

5. **ステータス追跡:**
   - `- [ ]` = 未着手
   - `- [x]` = 完了
   - AIがタスク完了時にステータスを自動更新

6. **出力場所:** `openspec/specs/{feature-name}/tasks.md`

