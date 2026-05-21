# AI-First, Spec-Driven Project Management Process

## "Self-Driving Software Team" Model

**Version:** 1.0
**Author:** DungDV (Mr4) – Sabitech  
**For:** Software development organizations transitioning to AI-first model  
**Audience:** Project Manager, Tech Lead, Delivery Manager, QA/Tester, BA, Developer

### Change Log

| Version | Date | Changes | By |
|---------|------|---------|-----|
| 1.0 | 2026-05-21 | Initial version | DungDV (Mr4) |

> ⚠️ This is a summary translation. For the complete detailed document (2800+ lines), see the Vietnamese original: [`../vi/self-driving-software-team.md`](../vi/self-driving-software-team.md)

---

## Core Principle

> Humans approve and take responsibility. AI defines, executes, and reports.

---

## Four Pillars

| Pillar | Role |
|--------|------|
| **RACI** | Clear responsibility between Human and AI |
| **RADIO** | Standardized reporting, automated by AI |
| **Spec** | Single source of truth, input for AI |
| **AI Agent** | Execute, validate, and report automatically |

---

## Workflow Summary

```
Raw requirements → [AI: Spec] → ✅ Approve → [AI: Design] → ✅ Approve → [AI: Tasks] → ✅ Approve → [AI: Code+Test] → [AI: RADIO] → ✅ Final Approve
```

---

## Key Concepts

### RACI in AI-First Model

| Role | Definition | Entity |
|------|-----------|--------|
| **R** – Responsible | Executes work | 🤖 AI Agent |
| **A** – Accountable | Final responsibility | 👤 Human (Lead/PM) |
| **C** – Consulted | Consulted before decisions | AI + Human |
| **I** – Informed | Notified of results | Stakeholders |

### RADIO – Progress Reporting Framework

| Letter | Content | Description |
|--------|---------|-------------|
| **R** | Review | Current status vs spec |
| **A** | Action | Work in progress |
| **D** | Difficulty | Blockers, risks, issues to escalate |
| **I** | Information | Additional context affecting progress |
| **O** | Outcome | Results achieved, specific metrics |

### Testing Strategy – Test Levels

| Level | Type | Executor | When |
|-------|------|----------|------|
| L1 | Unit Test | AI Agent | Every commit |
| L2 | Integration Test | AI Agent | Every PR |
| L3 | E2E Automation | AI Agent | Every PR / nightly |
| L4 | Desktop Automation | AI Agent | Nightly / release |
| L5 | Manual Test | QA/Tester (Human) | Before release |
| L6 | Property-Based Test | AI Agent | Every PR (optional) |

### Adoption Levels

```
Level 1: AI-Spec+Execute (Week 1–4)
Level 2: AI-Managed (Week 5–8)
Level 3: AI-Autonomous (Week 9–12)
Level 4: AI-Full (Week 13+)
```

### When NOT to Use Full Process

| Situation | Approach |
|-----------|----------|
| Production hotfix (< 30 min) | Fix directly → PR → document after |
| Config change | PR directly → approve |
| Typo / copy change | PR directly → approve |
| Prototype / POC | requirements.md only (brief) |
| Dependency update | PR + tests pass → approve |
| Small UI tweak | requirements + tasks only |

### Definition of Done

| Artifact | DoD |
|----------|-----|
| Spec | ✅ Approved + ✅ Versioned + ✅ No open QA + ✅ EARS format |
| Design | ✅ Approved + ✅ GUI reviewable + ✅ Covers all requirements |
| Feature | ✅ All tasks done + ✅ Tests pass + ✅ Spec compliance ≥ 95% + ✅ PR merged |
| Bugfix | ✅ Root cause confirmed + ✅ Fix merged + ✅ Regression pass + ✅ Prevention applied |

---

## Supplementary Documents

- 📖 [`quick-start.md`](quick-start.md) – Get started in 15 minutes
- 🔧 [`toolchain.md`](toolchain.md) – Tools & integration guide
- 💬 [`prompt-templates.md`](prompt-templates.md) – AI prompt templates for each phase

---

## Full Document Sections (in Vietnamese original)

1. Overview
2. Component Definitions (RACI, RADIO, Spec, AI Agent)
3. Execution Workflow
4. Real-World Examples (API Login, Dashboard UI, Bugfix)
5. Adoption Levels (1–4)
6. System Architecture
7. Risk Management + Exceptions + Definition of Done
8. Metrics & KPIs
9. Roles & Responsibilities (RACI matrix, Escalation, Versioning, Failure Handling, Communication)
10. Change Request Management
11. Risk & Issue Management (5WHY)
12. Q&A Management
13. Deliverables & Limitations
14. Approve Checklists
15. Implementation Checklist
16. Conclusion + Next Steps
- Appendix A: Feature Spec Structure
- Appendix B: Bugfix Spec Structure
