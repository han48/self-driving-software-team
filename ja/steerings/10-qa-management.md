---
inclusion: fileMatch
fileMatchPattern: '**/qa/**'
---

# Steering: Q&A管理

## 適用タイミング
AIがspec/designの曖昧さを検出した時またはステークホルダーに確認が必要な時。

## ルール

1. **AIが自らQ&Aを作成:** specの曖昧さを検出した時 → 自らQA-XXXを作成、Humanを待たない。

2. **構成:**
   - Index: `openspec/qa/index.md` (一覧表)
   - Thread: `openspec/qa/QA-XXX/thread.md` (詳細な会話)

3. **thread.md必須フォーマット:**
   ```markdown
   # QA-XXX: [トピック]
   
   **Status:** Open / Answered / Cancelled
   **作成日:** YYYY-MM-DD
   **質問者:** AI
   **対象者:** [顧客 / Architect / Lead]
   **優先度:** High / Medium / Low
   **関連:** [spec/CR/Risk reference]
   
   ---
   ## 元の質問
   [内容]
   
   ---
   ## Thread
   ### [YYYY-MM-DD] [担当者] →
   [Reply]
   
   ---
   ## 結論
   **決定:** [要約]
   **Action items:**
   - [ ] [アクション]
   ```

4. **結論が出た時:**
   - AIが結論をまとめる → Human確認
   - AIが結論に基づきspec/designを自動更新 → Human変更承認
   - index.mdでステータスを"Answered"に変更

5. **各Q&Aに1フォルダ:** 複数トピックを1つのthreadにまとめない

6. **期限:** High priorityのQAは48時間以内に回答必須

