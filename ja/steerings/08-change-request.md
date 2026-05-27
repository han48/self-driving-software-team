---
inclusion: fileMatch
fileMatchPattern: '**/change-request/**'
---

# Steering: 変更要求（CR）管理

## 適用タイミング
新しいCRがある時またはバックログのCRを処理する時。

## ルール

1. **すべてのCRはバックログに入れる:** `openspec/change-request/backlog.md`

2. **CRエントリの必須フォーマット:**
   ```
   ### CR-XXX: [タイトル]
   - **受領日:** YYYY-MM-DD
   - **発信元:** [ソース]
   - **優先度:** High / Medium / Low
   - **説明:** [簡潔な説明]
   ```

3. **ライフサイクル:** Pending → Approved → In Progress → Review → Done (またはRejected)

4. **In Progressに移行時:**
   - フォルダ作成 `openspec/change-request/CR-XXX/`
   - AIがrequirements.md生成 → Human承認
   - AIがdesign.md生成 → Human承認
   - AIがtasks.md生成 → Human承認
   - AIがtasksを実行

5. **ステータス別の必須フィールド:**
   - Approved: `**Approved by:**` + `**Scheduled:**` を追加
   - In Progress: `**Started:**` + `**Spec:**` を追加
   - Done: `**Completed:**` を追加
   - Rejected: `**Rejected by:**` + `**理由:**` を追加

6. **小規模CR (< 1日):** designをスキップ可能、requirements + tasksのみ
7. **大規模CR (> 1日):** 完全なspec必須 (requirements + design + tasks)

8. **承認なし = 実行なし:** AIは未承認のCRを実行してはならない

