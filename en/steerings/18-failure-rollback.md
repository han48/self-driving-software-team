---
inclusion: auto
---

# Steering: Failure Handling & Rollback

## When to Apply
When AI output is incorrect, tests fail continuously, or changes need to be
rolled back.

## Retry Policy

```
Attempt 1: AI self-fixes based on error message / feedback
Attempt 2: AI tries a different approach (different algorithm/architecture)
Attempt 3: AI summarizes attempts + root cause → escalates to Human
```

**After 3 failures → AI MUST:**
1. Stop
2. Generate report: "3 attempts failed, root cause: [X], suggested approach: [Y]"
3. Transfer task to Human override (Developer role)
4. Record in RADIO "D" + create ISSUE-XXX if needed

## Handling by Situation

| Situation | Action | Who decides |
|-----------|--------|-------------|
| Spec has logic error | AI self-fixes (loop) → still wrong → Human request changes | Human |
| Design not feasible | AI redesigns → fails 2 times → escalate Tech Lead | Tech Lead |
| Code fails test | AI fixes → retries 3 times → escalate | Tech Lead |
| Code passes test but wrong logic | Human rejects PR → AI re-reads spec → fix | Human |
| AI does not understand requirement | AI creates QA → waits for clarification → retry | Human |

## Rollback Strategy

| Scope | How to rollback | Tool |
|-------|----------------|------|
| Wrong code change | `git revert` commit | Git |
| Wrong spec change | Revert to previous version | Git |
| Wrong design | Revert + re-design from spec | Git |
| Feature wrong direction | Revert branch, go back to spec phase | Git branch |
| Production issue | Rollback deployment → hotfix | CI/CD |

## Rules

1. **Do not delete history:** Use `git revert`, not `git push --force`
2. **Record rollback reason:** In commit message + RADIO "D"
3. **Post-mortem:** After rollback → AI generates 5WHY analysis
4. **Prevention:** Each rollback → improvement (add test, add check)
5. **No infinite retries:** Maximum 3 times, then escalate
6. **Human override:** When AI fails → Developer has the right to code directly
