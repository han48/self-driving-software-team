---
inclusion: manual
---

# Skill: Pre-Approve Check – Design

## Purpose
Run automated checklist verification for design.md before presenting to Human for approval.

## Input
File `design.md` + `requirements.md` (already approved) to check.

## Automated Checklist

### Format & Structure
- [ ] Heading is `# Design Document: [Name]`
- [ ] Has section `## Overview` (not empty)
- [ ] Has section `## Site Map`
- [ ] Has section `## GUI Design` with reference to gui/ folder
- [ ] Has section `## Architecture` with at least 1 Mermaid diagram
- [ ] Has section `## Data Models / Database Design + ERD`
- [ ] Has section `## Infrastructure Design`
- [ ] Has section `## Components and Interfaces`
- [ ] Has section `## Correctness Properties` with at least 1 property

### Requirements Coverage
- [ ] Each requirement in requirements.md has at least 1 component/section addressing it
- [ ] No requirement is missed (cross-check requirement list vs design)
- [ ] Correctness Properties reference correct requirement numbers (`**Validates: Requirements X.X**`)

### Architecture Quality
- [ ] Mermaid diagrams render correctly (valid syntax)
- [ ] Sequence diagrams cover main flows (happy path + error)
- [ ] No "orphan" components (not connected to anything)
- [ ] Data flow is clear (know where data goes from/to)

### Database Design
- [ ] Each table has Primary Key
- [ ] Foreign Keys are clearly defined
- [ ] No duplicate field names between tables that aren't FKs
- [ ] Has index strategy for frequently queried fields

### Security (basic)
- [ ] No plaintext password storage in schema
- [ ] Sensitive fields are marked (nullable, encrypted...)
- [ ] Auth/authorization flow is described

### GUI Prototype
- [ ] gui/ folder exists and has at least index.html
- [ ] HTML files have navigation between screens
- [ ] Responsive (has viewport meta tag or CSS framework)

## Output format

```markdown
# Pre-Approve Check: Design

**File:** [path]
**Check date:** YYYY-MM-DD
**Result:** X/Y items PASS

## Requirements Coverage Matrix

| Requirement | Covered by | Status |
|-------------|-----------|--------|
| Req 1: Login | Architecture (sequence), Components (AuthController) | ✅ |
| Req 2: Register | Components (AuthController) | ✅ |
| Req 3: Notifications | ❌ NOT COVERED | ❌ |

## Issues to fix before approval

1. **Requirement 3** not covered in design
2. **Table login_attempts** missing index on `user_id`

## Recommendation

- [ ] Fix issues → re-run pre-approve check
- [ ] After all PASS → present to Human for approval
```
