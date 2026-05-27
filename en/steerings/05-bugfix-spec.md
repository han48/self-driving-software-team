---
inclusion: fileMatch
fileMatchPattern: '**/bugfix.md'
---

# Steering: Creating Bugfix Spec

## When to Apply
When AI handles complex bugs requiring root cause analysis and regression prevention.

## Rules

1. **bugfix.md – Mandatory format:**
   - Heading: `# Bugfix Requirements Document`
   - Sections: `## Introduction` → `## Bug Analysis`
   - Bug Analysis has 3 sub-sections:
     - `### Current Behavior (Defect)` – WHEN...THEN the system [incorrect]
     - `### Expected Behavior (Correct)` – WHEN...THE SYSTEM SHALL [correct]
     - `### Unchanged Behavior (Regression Prevention)` – WHEN...THE SYSTEM SHALL CONTINUE TO [preserve]
   - Numbering: 1.x (Defect), 2.x (Correct), 3.x (Unchanged)

2. **design.md – Mandatory format:**
   - Sections: Overview → Glossary → Bug Details → Expected Behavior → Hypothesized Root Cause (5WHY) → Fix Implementation → Correctness Properties → Testing Strategy → Files Changed
   - `## Hypothesized Root Cause (5WHY)` must use the 5WHY method:
     - 5 consecutive "Why" levels
     - Root cause conclusion
     - Prevention (avoid recurrence)
   - `## Files Changed` is a table listing changed files (New/Modified)
   - Correctness Properties include: valid passes, invalid fails, no side effects

3. **tasks.md – Mandatory format:**
   - Reference uses `_Bugfix: X.X_` (not `_Requirements:_`)
   - Flow: Fix code → Apply → Unit tests → Feature tests
   - Task Dependency Graph is mandatory

4. **Surgical fix principles:**
   - Change as little as possible
   - Unchanged Behavior MUST be explicitly listed
   - Files Changed MUST explicitly list files allowed to be modified

5. **Output location:** `openspec/bugs/{bug-name}/`
