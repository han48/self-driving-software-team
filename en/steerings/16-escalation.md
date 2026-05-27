---
inclusion: auto
---

# Steering: Escalation Matrix & Communication Protocol

## When to Apply
When AI needs to escalate an issue, when Human does not respond, or when
communication between roles is needed.

## Escalation Matrix

### By Severity

| Severity | Escalate to | Response SLA | Resolution SLA | Channel |
|----------|-------------|--------------|----------------|---------|
| **Critical** | PM + Tech Lead + Stakeholder | 1 hour | 4 hours | Slack urgent + Phone |
| **High** | Tech Lead + PM | 4 hours | 24 hours | Slack channel |
| **Medium** | Tech Lead | 24 hours | 3 days | Slack / Email |
| **Low** | Log, handle when bandwidth allows | 48 hours | Next sprint | Email / Backlog |

### By Issue Type

| Issue | Escalation path | Trigger |
|-------|----------------|---------|
| Production down | AI → Tech Lead → PM → Stakeholder | Immediately |
| High Risk occurs | AI → Tech Lead → PM | Within 4 hours |
| High Q&A overdue (> 48h) | AI flags RADIO "D" → PM escalates to Customer | After 48h |
| AI fails > 3 times | AI → Tech Lead (Human override) | After 3 retries |
| Spec conflict | AI → Tech Lead + BA | Immediately upon detection |
| Security vulnerability | AI → Tech Lead → Security Engineer | Immediately |
| Human not responding to approve | AI reminds (24h) → reminds (48h) → escalate PM | After 48h |

## Communication Protocol

### Communication Channels

| Type | Channel | SLA | Example |
|------|---------|-----|---------|
| Approve request | Slack + In-tool | 4h (High), 24h (Medium) | "PR ready", "Spec needs approve" |
| Clarification | Slack thread + QA file | 24h (High), 48h (Medium) | "Spec ambiguity" |
| Escalation | Slack urgent / Phone | 1h (Critical), 4h (High) | "Blocker" |
| Information | Email / Channel | No response needed | "New RADIO" |

### When Human Does Not Respond

```
0–24h:  AI waits, works on other non-dependent tasks
24–48h: AI reminds again + records in RADIO "D"
48–72h: AI escalates to PM + creates ISSUE
> 72h:  AI pauses feature, reports blocker
```

## Rules

1. **Async-first:** Default async, sync only when Critical
2. **Thread-based:** One thread per topic
3. **Actionable:** Each message clearly states: who does what, deadline when
4. **Recorded:** Decisions must be recorded in spec/QA/RADIO
5. **AI self-notifies:** AI sends notification when approval is needed
6. **No skipping levels:** Escalate in order (except Critical)
