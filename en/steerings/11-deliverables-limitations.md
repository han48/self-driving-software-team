---
inclusion: fileMatch
fileMatchPattern: '**/deliverables/**'
---

# Steering: Deliverables & Limitations Management

## When to Apply
When ending a sprint/phase/release or when consolidating project status.

## Rules

1. **AI self-consolidates:** Scan all tasks.md, test results, RADIO to generate deliverables

2. **Folder structure:**
   ```
   openspec/deliverables/
   ├── index.md                ← Summary table of all delivery batches
   ├── sprint-XX/deliverables.md   ← Per sprint
   ├── phase-X/deliverables.md     ← Per phase (grouped sprints)
   └── YYYY-MM-DD/deliverables.md  ← Per date (hotfix, urgent)
   ```

3. **index.md format:**
   ```markdown
   # Deliverables Index
   
   | Batch | Date | Features delivered | Limitations | Link |
   |-------|------|-------------------|-------------|------|
   | Sprint 3 | 2025-03-28 | Login, Register | 2 LIM | `sprint-03/deliverables.md` |
   ```

4. **Deliverable entry:**
   ```
   ### Feature: [Name]
   - **Spec:** `openspec/specs/{feature-name}/`
   - **Status:** ✅ Complete
   - **Test coverage:** XX%
   - **Spec compliance:** XX%
   - **Deployed:** [environment]
   - **Description:** [Summary]
   ```

5. **Limitation entry:**
   ```
   ### LIM-XXX: [Name]
   - **Type:** Not Implemented / Partial / Technical Constraint / Known Issue
   - **Related:** [Spec/CR reference]
   - **Description:** [Details]
   - **Reason:** [Why]
   - **Impact:** [Impact]
   - **Plan:** Phase 2 / Backlog / Won't Fix
   - **Workaround:** [If any]
   ```

6. **Limitation classification:**
   - Not Implemented: requirement exists but not yet built
   - Partial: built but not complete
   - Technical Constraint: technical limitation
   - Known Issue: known bug not yet fixed

7. **Separate by delivery batch:**
   - Sprint: one file per sprint
   - Phase: consolidation of multiple sprints
   - Hotfix/urgent: by date (YYYY-MM-DD)

8. **Update index.md** each time a new delivery batch occurs

9. **Human confirm:** All limitations must be confirmed by Human before sending to customer

10. **Mandatory summary:** Metric summary table at the end of each deliverables.md file
