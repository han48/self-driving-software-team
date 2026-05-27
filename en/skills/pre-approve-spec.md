---
inclusion: manual
---

# Skill: Pre-Approve Check – Requirements (Spec)

## Purpose
Run automated checklist verification for requirements.md before presenting to Human for approval. Reduce Human review time by having AI detect issues first.

## Input
File `requirements.md` to check.

## Automated Checklist

Read the requirements.md file and check each item below. Output PASS/FAIL result for each item:

### Format & Structure
- [ ] Heading is `# Requirements Document`
- [ ] Has section `## Introduction` (not empty)
- [ ] Has section `## Glossary` (at least 1 term)
- [ ] Has separator `---` before `## Requirements`
- [ ] Has at least 1 `### Requirement N:` section
- [ ] Each requirement has `**User Story:**` with format "As a... I want... so that..."
- [ ] Each requirement has `#### Acceptance Criteria` with at least 1 item

### EARS Notation
- [ ] All AC use pattern: THE/WHEN/IF + SHALL
- [ ] Component names are UPPERCASE (THE API, THE System, THE Admin_Panel...)
- [ ] Each AC is numbered (1, 2, 3...)
- [ ] No AC uses vague language ("should", "might", "better")

### Logic & Completeness
- [ ] No 2 ACs contradict each other (same condition → different behavior)
- [ ] Has error cases / edge cases (not just happy path)
- [ ] Glossary covers all domain-specific terms appearing in AC
- [ ] No hidden assumptions (all assumptions stated explicitly in AC or Introduction)

### Testability
- [ ] Each AC can be converted to a specific test case (has clear input + expected output)
- [ ] No AC is too generic ("system must be fast", "UI must be beautiful")

### Traceability
- [ ] If related Risk exists → references RISK-XXX
- [ ] If related Q&A exists → references QA-XXX or Q&A is Answered

## Output format

```markdown
# Pre-Approve Check: Requirements

**File:** [path]
**Check date:** YYYY-MM-DD
**Result:** X/Y items PASS

## Results

| # | Item | Status | Note |
|---|------|--------|------|
| 1 | Heading correct format | ✅ PASS | |
| 2 | Introduction not empty | ✅ PASS | |
| 3 | EARS notation correct | ❌ FAIL | AC 3.2 missing SHALL |
| ... | ... | ... | ... |

## Issues to fix before approval

1. **AC 3.2:** Missing keyword SHALL – currently says "system returns error" → fix to "THE System SHALL return error"
2. **Glossary:** Term "DashboardMessage" appears in AC but not in Glossary

## Recommendation

- [ ] Fix 2 issues above → re-run pre-approve check
- [ ] After all PASS → present to Human for approval
```
