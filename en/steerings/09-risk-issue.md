---
inclusion: fileMatch
fileMatchPattern: '**/risk-issue/**'
---

# Steering: Risk & Issue Management

## When to Apply
When AI detects a risk or issue during work.

## Rules

1. **AI self-creates Risk/Issue:** Do not wait for Human – AI detects and creates immediately.

2. **Risk format (in `openspec/risk-issue/risks.md`):**
   ```
   ### RISK-XXX: [Name]
   - **Date detected:** YYYY-MM-DD
   - **Detected by:** AI
   - **Probability:** High / Medium / Low
   - **Impact:** High / Medium / Low
   - **Severity:** Critical / High / Medium / Low
   - **Description:** [Details]
   - **Prevention action:** [Prevent risk from occurring]
   - **Mitigation action:** [If it occurs → minimize consequences]
   - **Owner:** [Who monitors]
   - **Trigger:** [Warning signs]
   - **Status:** Open
   ```

3. **Issue format (in `openspec/risk-issue/issues.md`):**
   ```
   ### ISSUE-XXX: [Name]
   - **Date occurred:** YYYY-MM-DD
   - **Detected by:** AI
   - **Severity:** Critical / High / Medium / Low
   - **Impact:** [Impact]
   - **Description:** [Details]
   - **5WHY Analysis:**
     - Why 1: [Why?] → [Answer]
     - Why 2: [Why?] → [Answer]
     - Why 3: [Why?] → [Answer]
     - Why 4: [Why?] → [Answer]
     - Why 5: [Why?] → [Root cause]
   - **Root cause:** [Conclusion from 5WHY]
   - **Resolution plan:** [Address root cause]
   - **Prevention:** [Avoid recurrence]
   - **Owner:** [Who resolves]
   - **Deadline:** YYYY-MM-DD
   - **Status:** Open
   ```

4. **Distinction:**
   - Risk = has not occurred yet (potential) → Prevention + Mitigation
   - Issue = has already occurred → Resolution

5. **Closed conditions for Risk:**
   - Both actions must be completed:
     - ✅ Prevention applied (reduce probability)
     - ✅ Mitigation prepared (reduce consequences)
   - Only 1/2 → status remains Mitigated, not Closed

6. **5WHY mandatory for Issues:**
   - AI self-performs 5WHY analysis when creating an issue
   - Each "Why" must have evidence, no guessing
   - Root cause must be something fixable
   - Resolution plan addresses root cause, not just symptoms
   - Prevention stops issue from recurring

7. **RADIO integration:** Flag risk/issue in RADIO "D" (Difficulty) items

8. **Human only confirms:** AI creates, Human confirms severity + approves plan
