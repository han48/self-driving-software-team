---
inclusion: auto
---

# Steering: AI-First, Spec-Driven Process (Overview)

## Core Principles

> Humans approve and bear accountability. AI defines, executes, and reports.

## Main Workflow

```
Human provides raw requirements
    ↓
AI generates Spec (requirements.md) → 🔄 AI self-checks → Human approve
    ↓
AI generates Design (design.md + gui/) → 🔄 AI self-checks → Human approve
    ↓
AI generates Tasks (tasks.md) → Human approve
    ↓
AI executes (code + test L1–L4, L6)
    ↓
AI validates (spec compliance + test results)
    ↓
AI generates RADIO report
    ↓
Human approve (PR + RADIO)
```

## Folder Structure

```
📁 openspec/
├── 📁 specs/{feature}/        ← Feature specs + gui/
├── 📁 bugs/{bug}/             ← Bugfix specs
├── 📁 change-request/         ← CR backlog + specs
├── 📁 risk-issue/             ← risks.md + issues.md
├── 📁 qa/                     ← Q&A index + threads
└── deliverables.md            ← Deliverables + Limitations
```

## Auto-Created by AI During Work

- **Risk:** When a potential risk is detected → `openspec/risk-issue/risks.md`
- **Issue:** When an active problem is detected → `openspec/risk-issue/issues.md`
- **Q&A:** When spec ambiguity is detected → `openspec/qa/QA-XXX/thread.md`
- **RADIO:** After each task/milestone → in spec folder or inline

## Golden Rules

1. Never skip a phase: Requirements → Design → Tasks → Execute
2. Never execute without approval
3. All output must be traceable (reference Requirements/Bugfix)
4. AI self-detects risk/issue/Q&A – do not wait for Human
5. Human only approves, does not execute (except manual test L5)
6. AI retries max 3 times, then escalates to Human
7. RADIO updated at least once/day when active
8. All spec changes must have version + change log
9. Escalate per SLA: Critical 1h, High 4h, Medium 24h
10. Async-first communication, sync only when Critical

> 📖 Abbreviations & terminology: see Glossary in `self-driving-software-team.md`
