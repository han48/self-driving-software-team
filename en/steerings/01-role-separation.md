---
inclusion: auto
---

# Steering: Role Separation & Approve Rules

## When to Apply
Always – applies to every action in the process.

## Rules

1. **AI is Responsible, Human is Accountable:**
   - AI generates all output (spec, design, tasks, code, test, report, risk, Q&A)
   - Human only approves/confirms
   - AI never self-approves

2. **No self-approval of own output:**
   - The person providing requirements ≠ the person approving spec
   - The person writing code (overriding AI) ≠ the person approving PR
   - If one person holds multiple roles → explicitly state which role is acting

3. **Annotate role on every action:**
   - "Approved by: [Name] (as [Role])"
   - Example: "Approved by: John D (as Tech Lead)"

4. **Mandatory approve gates (Human must approve before AI continues):**
   - Requirements → approve → then generate Design
   - Design → approve → then generate Tasks
   - Tasks → approve → then execute Code
   - PR → approve → then merge
   - Risk/Issue → confirm → then track
   - Q&A conclusion → confirm → then update spec
   - CR → approve → then execute

5. **Mandatory checklist:** Each approve gate has its own checklist (see section 14 in the process)

6. **Rotate reviewer:** Same person must not approve > 3 consecutive PRs for the same feature
