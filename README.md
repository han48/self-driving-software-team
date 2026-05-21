# Self-Driving Software Team – AI-First, Spec-Driven Process

> **Humans approve and take responsibility. AI defines, executes, and reports.**

A comprehensive project management framework for software teams transitioning to an AI-first delivery model. Combines RACI, RADIO reporting, Specification-driven development, and AI Agents into a cohesive system.

🌐 **Language:** [Tiếng Việt](README.vi.md) | English | [日本語](README.ja.md)

---

## Overview

This framework transforms software project management from:

```
"Managing people doing work" → "Managing AI systems executing work"
```

**Four Pillars:**

| Pillar | Function |
|--------|----------|
| **RACI** | Clear responsibility between Human and AI |
| **RADIO** | Standardized reporting, automated by AI |
| **Spec** | Single source of truth, input for AI |
| **AI Agent** | Execute, validate, and report automatically |

---

## Document Structure

```
📁 project-processing/
├── README.md                    ← You are here (English)
├── README.vi.md                 ← Vietnamese README
├── README.ja.md                 ← Japanese README
├── 📁 vi/                       ← Vietnamese (original, complete)
│   ├── self-driving-software-team.md   ← Main document (2800+ lines)
│   ├── quick-start.md
│   ├── toolchain.md
│   └── prompt-templates.md
├── 📁 en/                       ← English (summary + supplements)
│   ├── self-driving-software-team.md   ← Summary translation
│   ├── quick-start.md
│   ├── toolchain.md
│   └── prompt-templates.md
├── 📁 ja/                       ← Japanese (summary + supplements)
│   ├── self-driving-software-team.md   ← サマリー翻訳
│   ├── quick-start.md
│   ├── toolchain.md
│   └── prompt-templates.md
├── 📁 steerings/                ← AI steering files (19 files)
└── 📁 skills/                   ← AI skill files (7 files)
```

---

## Quick Start

1. Read [`en/quick-start.md`](en/quick-start.md) (15 minutes)
2. Set up your toolchain: [`en/toolchain.md`](en/toolchain.md)
3. Use prompt templates: [`en/prompt-templates.md`](en/prompt-templates.md)
4. For full details, read the main document: [`vi/self-driving-software-team.md`](vi/self-driving-software-team.md)

---

## Key Features

- **Spec-Driven:** Every feature starts with a formal specification (EARS notation)
- **AI Executes:** AI generates specs, designs, tasks, code, tests, and reports
- **Human Approves:** Humans review and approve at every gate
- **RADIO Reporting:** Automated progress reports based on real execution data
- **Risk Management:** AI auto-detects risks and issues with 5WHY analysis
- **Change Request Management:** Structured CR lifecycle with backlog tracking
- **Q&A Management:** Threaded Q&A for spec clarification
- **Definition of Done:** Clear DoD for every artifact and phase
- **Adoption Levels:** 4-level roadmap from basic to full AI autonomy

---

## Who Is This For?

| Role | How You Use This |
|------|-----------------|
| **Project Manager** | Governance, KPIs, escalation, CR approval |
| **Tech Lead** | Approve specs, designs, PRs; technical decisions |
| **BA** | Provide requirements, approve business logic |
| **QA/Tester** | Manual testing (L5), review test coverage |
| **Developer** | Technical consultation, override AI when needed |
| **AI Agent** | Execute everything (spec → code → test → report) |

---

## Version

**Current:** v1.2 (2026-05-21)

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-05-21 | Initial version |

---

## Author

**DungDV (Mr4)** – Sabitech

---

## License

Internal use. Contact author for distribution rights.
