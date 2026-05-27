# Quick Start Guide – Get Started in 15 Minutes

> For teams new to the AI-First, Spec-Driven process who want to try it immediately without reading the full documentation.

---

## Step 1: Preparation (5 minutes)

```bash
# Create directory structure
mkdir -p openspec/specs openspec/bugs openspec/change-request openspec/risk-issue openspec/qa openspec/deliverables
```

## Step 2: Provide Requirements (2 minutes)

Write raw requirements into the AI Agent prompt. Example:
> "Need a user registration API that accepts email + password + name, validates input, saves to DB, and sends a confirmation email."

No standard format needed – can be a ticket, meeting note, or chat. AI will ask for clarification if ambiguous.

## Step 3: AI Generates Spec → You Approve (3 minutes)

AI will generate `openspec/specs/{feature}/requirements.md`. You:
1. Read acceptance criteria (EARS format: THE/WHEN/IF + SHALL)
2. Check if edge cases are sufficient
3. Approve or request changes

## Step 4: AI Generates Design → You Approve (3 minutes)

AI generates `design.md` + `gui/` (HTML prototype). You:
1. Open HTML prototype in browser, click through the flow
2. Review architecture diagram
3. Approve or request changes

## Step 5: AI Generates Tasks → Approve → AI Executes (2 minutes)

AI generates `tasks.md`, you approve, AI starts coding + testing.

## Step 6: Review Results

AI generates RADIO report. You review PR + RADIO → approve or request changes.

---

## One-Line Summary

```
Raw requirement → [AI: Spec] → ✅ Approve → [AI: Design] → ✅ Approve → [AI: Tasks] → ✅ Approve → [AI: Code+Test] → [AI: RADIO] → ✅ Final Approve
```

---

## "Ready to Go" Checklist

- [ ] `openspec/` folder created
- [ ] AI Agent set up (Kiro/Claude)
- [ ] Team understands: AI executes, Human approves
- [ ] At least 1 person knows the approve checklist (see Section 14 in main document)
- [ ] CI/CD pipeline runs automated tests

---

## Next Steps

After successfully running through 1 small feature:
1. Read the main document (`self-driving-software-team.md`) for deeper understanding
2. Apply Adoption Level 1 (Section 5)
3. Gradually expand scope following the Level 1 → 4 roadmap
