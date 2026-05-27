---
inclusion: manual
---

# Skill: Pre-Approve Check – Tasks

## Purpose
Run automated checklist verification for tasks.md before presenting to Human for approval.

## Input
File `tasks.md` + `design.md` (already approved) to check.

## Automated Checklist

### Format & Structure
- [ ] Heading is `# Implementation Plan: [Name]`
- [ ] Has section `## Overview`
- [ ] Has section `## Tasks` with nested checkboxes
- [ ] All tasks use format `- [ ] N.` or `- [ ] N.M`
- [ ] Has `## Task Dependency Graph` with JSON waves format
- [ ] JSON waves syntax is valid (parseable)

### Granularity & Quality
- [ ] Each sub-task has description clear enough for AI to execute (not too generic)
- [ ] No task is too large (estimate > 4h if estimates exist)
- [ ] Each sub-task ends with `_Requirements: X.X_`

### Design Coverage
- [ ] All components in design.md have corresponding tasks
- [ ] All data models have migration/model tasks
- [ ] Has tasks for testing (unit, integration, E2E)
- [ ] Has tasks for property-based tests (if design has Correctness Properties)

### Dependencies
- [ ] Task dependency graph covers all task numbers
- [ ] No circular dependencies
- [ ] Tasks in later waves correctly depend on earlier wave tasks
- [ ] No "orphan" tasks (not in any wave)

### Traceability
- [ ] Each sub-task references at least 1 requirement number
- [ ] All requirements are referenced by at least 1 task
- [ ] Property test tasks have format: `**Property N: [Name]**` + `**Validates: Requirements X.X**`

## Output format

```markdown
# Pre-Approve Check: Tasks

**File:** [path]
**Check date:** YYYY-MM-DD
**Result:** X/Y items PASS

## Requirements Traceability

| Requirement | Tasks covering | Status |
|-------------|---------------|--------|
| Req 1.1 | Task 3.1, 4.2 | ✅ |
| Req 2.3 | ❌ No task | ❌ |

## Dependency Graph Validation

- Waves: 4
- Total tasks: 12
- Orphan tasks: 0
- Circular deps: None ✅

## Issues to fix before approval

1. **Requirement 2.3** has no task covering it
2. **Task 5** not in dependency graph

## Recommendation

- [ ] Fix issues → re-run pre-approve check
- [ ] After all PASS → present to Human for approval
```
