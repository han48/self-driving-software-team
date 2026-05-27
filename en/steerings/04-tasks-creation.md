---
inclusion: fileMatch
fileMatchPattern: '**/tasks.md'
---

# Steering: Creating Tasks (Implementation Plan)

## When to Apply
When AI generates tasks.md from approved design.

## Rules

1. **Mandatory format:**
   - Heading: `# Implementation Plan: [Feature name]`
   - Sections: `## Overview` → `## Tasks` → `## Task Dependency Graph` → `## Notes`
   - Tasks use nested checkboxes: `- [ ] 1.` → `- [ ] 1.1`
   - Each sub-task ends with: `_Requirements: X.X, Y.Y_`

2. **Task granularity:**
   - Each task < 4h effort
   - Each task has a verifiable expected outcome
   - Include tasks for tests (unit, integration, E2E)

3. **Dependency Graph using JSON waves:**
   ```json
   {
     "waves": [
       {"tasks": ["1"]},
       {"tasks": ["2", "3"]},
       {"tasks": ["4"]}
     ]
   }
   ```
   - Tasks in the same wave run in parallel
   - Next wave waits for previous wave to complete

4. **Property test tasks:**
   - Format: `- [ ] N.M Write property test — Property P`
   - Inside: `**Property P: [Name]**` + `**Validates: Requirements X.X**`

5. **Status tracking:**
   - `- [ ]` = Not started
   - `- [x]` = Complete
   - AI self-updates status upon task completion

6. **Output location:** `openspec/specs/{feature-name}/tasks.md`
