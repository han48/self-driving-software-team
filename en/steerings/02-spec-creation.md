---
inclusion: fileMatch
fileMatchPattern: '**/requirements.md'
---

# Steering: Creating Spec (Requirements)

## When to Apply
When AI generates requirements.md for a new feature or CR.

## Rules

1. **Mandatory format:**
   - Heading: `# Requirements Document`
   - Sections: `## Introduction` → `## Glossary` → `---` → `## Requirements`
   - Each requirement: `### Requirement N: [Name]` + `**User Story:**` + `#### Acceptance Criteria`

2. **EARS Notation mandatory for Acceptance Criteria:**
   - `THE [Component] SHALL [behavior]` – always-true behavior
   - `WHEN [condition] THE [Component] SHALL [behavior]` – trigger-based
   - `IF [condition] THEN THE [Component] SHALL [behavior]` – conditional
   - Component name in UPPERCASE: THE API, THE System, THE Admin_Panel...

3. **Self-check loop before presenting to Human for approval:**
   - Is the spec clear and complete?
   - Are there logical contradictions between ACs?
   - Are there technical difficulties?
   - Risk detected → create RISK-XXX in `openspec/risk-issue/risks.md`
   - Ambiguity detected → create QA-XXX in `openspec/qa/`
   - Repeat until no issues remain

4. **Quality:**
   - Each AC must be testable (convertible to a test case)
   - Include both happy path and error cases
   - No hidden assumptions – all assumptions stated explicitly
   - Glossary defines all domain-specific terms

5. **Output location:** `openspec/specs/{feature-name}/requirements.md`
