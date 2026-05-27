---
inclusion: fileMatch
fileMatchPattern: '**/risk-issue/**'
---

# Steering: リスク＆課題管理

## 適用タイミング
AIが作業中にリスクまたは課題を検出した時。

## ルール

1. **AIが自らRisk/Issueを作成:** Humanを待たない – AIが検出し即座に作成。

2. **Riskフォーマット (`openspec/risk-issue/risks.md`内):**
   ```
   ### RISK-XXX: [名前]
   - **検出日:** YYYY-MM-DD
   - **検出者:** AI
   - **発生確率:** High / Medium / Low
   - **影響度:** High / Medium / Low
   - **重要度:** Critical / High / Medium / Low
   - **説明:** [詳細]
   - **予防措置 (Prevention):** [リスク発生を防ぐ]
   - **軽減措置 (Mitigation):** [発生時 → 影響を最小化]
   - **Owner:** [追跡担当者]
   - **Trigger:** [認識すべき兆候]
   - **Status:** Open
   ```

3. **Issueフォーマット (`openspec/risk-issue/issues.md`内):**
   ```
   ### ISSUE-XXX: [名前]
   - **発生日:** YYYY-MM-DD
   - **検出者:** AI
   - **Severity:** Critical / High / Medium / Low
   - **影響:** [Impact]
   - **説明:** [詳細]
   - **5WHY Analysis:**
     - Why 1: [なぜ？] → [回答]
     - Why 2: [なぜ？] → [回答]
     - Why 3: [なぜ？] → [回答]
     - Why 4: [なぜ？] → [回答]
     - Why 5: [なぜ？] → [Root cause]
   - **Root cause:** [5WHYからの結論]
   - **Resolution plan:** [Root causeの解決]
   - **Prevention:** [再発防止]
   - **Owner:** [対応担当者]
   - **Deadline:** YYYY-MM-DD
   - **Status:** Open
   ```

4. **区別:**
   - Risk = まだ発生していない (潜在的) → Prevention + Mitigation
   - Issue = すでに発生している → Resolution

5. **RiskのClosed条件:**
   - 両方のアクションが完了していること:
     - ✅ Preventionが適用済み (発生確率を低減)
     - ✅ Mitigationが準備済み (影響を低減)
   - 片方のみ → ステータスはMitigatedのまま、Closedにはできない

6. **Issueには5WHY必須:**
   - AIがissue作成時に5WHY analysisを自ら実施
   - 各"Why"にはエビデンスが必要、推測しない
   - Root causeは修正可能なものであること
   - Resolution planはroot causeを解決する、症状だけではない
   - Preventionはissueの再発を防ぐ

7. **RADIO連携:** RADIO "D" (Difficulty) itemsにrisk/issueをフラグ

8. **Humanは確認のみ:** AIが作成、Humanがseverityを確認 + planを承認

