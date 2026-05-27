---
inclusion: fileMatch
fileMatchPattern: '**/qa/**'
---

# Steering: Q&A Management

## When to Apply
When AI detects ambiguity in spec/design or needs clarification from stakeholders.

## Rules

1. **AI self-creates Q&A:** When spec ambiguity is detected → self-create QA-XXX, do not wait for Human.

2. **Structure:**
   - Index: `openspec/qa/index.md` (summary table)
   - Thread: `openspec/qa/QA-XXX/thread.md` (detailed conversation)

3. **thread.md mandatory format:**
   ```markdown
   # QA-XXX: [Topic]
   
   **Status:** Open / Answered / Cancelled
   **Date created:** YYYY-MM-DD
   **Asked by:** AI
   **Target:** [Customer / Architect / Lead]
   **Priority:** High / Medium / Low
   **Related:** [spec/CR/Risk reference]
   
   ---
   ## Original Question
   [Content]
   
   ---
   ## Thread
   ### [YYYY-MM-DD] [Person] →
   [Reply]
   
   ---
   ## Conclusion
   **Decision:** [Summary]
   **Action items:**
   - [ ] [Action]
   ```

4. **When conclusion is reached:**
   - AI summarizes conclusion → Human confirms
   - AI self-updates spec/design based on conclusion → Human approves changes
   - Change status to "Answered" in index.md

5. **One folder per Q&A:** Do not merge multiple topics into one thread

6. **Deadline:** High priority Q&A must have a response within 48h
