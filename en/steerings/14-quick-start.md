---
inclusion: manual
---

# Steering: Quick Start Guide

## When to Apply
When a team is starting to adopt the AI-First, Spec-Driven process.

> 📖 Full details: see file [`quick-start.md`](../quick-start.md)

## Summary Flow

```
Raw requirements → [AI: Spec] → ✅ → [AI: Design] → ✅ → [AI: Tasks] → ✅ → [AI: Code+Test] → [AI: RADIO] → ✅ Done
```

## 6 Steps

1. **Prepare:** Create folder `openspec/`
2. **Requirements:** Write raw description for AI
3. **Spec:** AI generates → Human approves
4. **Design:** AI generates + HTML prototype → Human approves
5. **Tasks:** AI generates → Human approves → AI executes
6. **Review:** AI generates RADIO → Human approves PR

## Readiness Checklist

- [ ] Folder `openspec/` created
- [ ] AI Agent set up (Kiro/Claude)
- [ ] Team understands: AI executes, Human approves
- [ ] At least 1 person knows the approve checklist
- [ ] CI/CD pipeline runs automated tests
