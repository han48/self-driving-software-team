---
inclusion: fileMatch
fileMatchPattern: '**/radio.md'
---

# Steering: RADIOレポート生成

## 適用タイミング
AIが各タスク/スプリント後に進捗報告する時または重要な変更がある時。

## 更新頻度 (必須)

| トリガー | タイミング | 必須 |
|---------|---------|----------|
| Task complete | タスクが`[x]`に変わる度 | ✅ |
| Test fail | CIがfailureを報告した時 | ✅ |
| 新しいBlocker | blocker/riskを検出した時 | ✅ |
| End of day | 1日の終わり (progressがある場合) | ✅ |
| Sprint end | スプリント終了時の統合 | ✅ |
| Milestone | dependency内のwave完了時 | 推奨 |
| **最低限** | **アクティブ時に少なくとも1日1回** | ✅ |

## ルール

1. **ファイル場所:** `openspec/specs/{feature-name}/radio.md`

2. **radio.mdの構成:**
   ```markdown
   # RADIO Report: [Feature名]

   **更新日:** YYYY-MM-DD
   **Sprint/Phase:** [Sprint X]
   **生成者:** AI Agent
   **レビュー者:** [Human – レビュー済みの場合]

   ---

   ## Latest Report

   **R (Review):** [現在の状態、具体的なメトリクス]
   **A (Action):** [実行中の作業、次のステップ]
   **D (Difficulty):** [Blockers、risks、issues – または"None"]
   **I (Information):** [進捗に影響するコンテキスト – または"—"]
   **O (Outcome):** [達成した成果、数値]

   ---

   ## History

   ### [YYYY-MM-DD]
   - R: [...]
   - A: [...]
   - D: [...]
   - I: [...]
   - O: [...]

   ### [YYYY-MM-DD]
   - R: [...]
   - A: [...]
   - D: [...]
   - I: [...]
   - O: [...]
   ```

3. **RADIOコンテンツルール:**
   - R: テスト結果、spec compliance %、tasks.mdステータス (X/Y done) から
   - A: tasks.md (進行中のtask) から
   - D: risk-issueレジスター、テスト失敗、依存関係の問題から
   - I: モニタリング、外部要因、ステークホルダー更新から
   - O: CI/CD結果、coverage %、完了タスク数から

4. **"D" itemsの重要性:**
   - 重大なD item → RISK-XXXまたはISSUE-XXXが既にあるか確認
   - なければ → 即座に作成
   - CriticalなD itemsはRADIO内でescalate必須

5. **History:**
   - AIが新しいRADIOを生成する度 → Latest Reportを更新、古いレポートはHistoryに移動
   - Historyは新しい順に並べる
   - 最新10エントリを保持 (古いものはarchive可能)

6. **主観を排除:** RADIOはデータに基づく、感覚ではない

7. **必須項目:**

   | 項目 | 必須 |
   |---------|----------|
   | `# RADIO Report: [名前]` | ✅ |
   | Headerメタデータ (日付、Sprint、AI、Human) | ✅ |
   | `## Latest Report` | ✅ |
   | R、A、D、I、O (5項目すべて) | ✅ |
   | `## History` | ✅ |
