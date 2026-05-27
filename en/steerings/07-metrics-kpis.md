---
inclusion: fileMatch
fileMatchPattern: '**/radio.md'
---

# Steering: Metrics & KPIs Tracking

## When to Apply
When AI generates RADIO reports, consolidates deliverables, or needs to evaluate process effectiveness.

## KPIs to Track

| Metric | Target | Frequency | Alert |
|--------|--------|-----------|-------|
| Spec Compliance Rate | ≥ 95% | Every PR | < 90% → RADIO "D" |
| RADIO Accuracy | ≥ 90% | Every sprint | < 80% → review |
| AI Task Completion Rate | ≥ 80% | Every sprint | < 60% → escalate |
| Time to Delivery | 50% reduction vs baseline | Every feature | > 2x baseline → "D" |
| Human Review Time | < 30 min/PR | Every PR | > 2h → remind |
| Defect Escape Rate | < 5% | Every sprint | > 10% → review |
| Test Coverage | ≥ 80% | Every PR | < 70% → block merge |
| Risk Detection Rate | ≥ 70% | Every sprint | < 50% → improve |
| Q&A Resolution Time | < 48h (High) | Every sprint | High > 72h → escalate |
| CR Throughput | Increasing trend | Every sprint | Decreasing 2 sprints → review |

## Establishing Baseline

1. **Sprint 1–2:** Measure metrics at current state
2. **Record baseline:** `openspec/deliverables/baseline.md`
3. **Compare:** From Sprint 3, compare against baseline each sprint
4. **Adjust:** After 3 sprints, adjust targets if needed

## Rules

1. **AI self-calculates metrics** from real data (CI, Git, tasks.md, test results)
2. **Include in RADIO "O" (Outcome):** Each RADIO report must have at least 2 metrics
3. **Include in deliverables.md:** Summary table at end of file
4. **Alert when below target:** AI flags in RADIO "D" if metric is below target
5. **Trend tracking:** Compare with previous sprint/phase to show trends
