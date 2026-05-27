---
inclusion: fileMatch
fileMatchPattern: '**/radio.md'
---

# Steering: Metrics & KPIs Tracking

## Khi nào áp dụng
Khi AI sinh RADIO report, tổng hợp deliverables, hoặc cần đánh giá hiệu quả quy trình.

## KPIs cần track

| Metric | Target | Tần suất | Alert |
|--------|--------|----------|-------|
| Spec Compliance Rate | ≥ 95% | Mỗi PR | < 90% → RADIO "D" |
| RADIO Accuracy | ≥ 90% | Mỗi sprint | < 80% → review |
| AI Task Completion Rate | ≥ 80% | Mỗi sprint | < 60% → escalate |
| Time to Delivery | Giảm 50% vs baseline | Mỗi feature | > 2x baseline → "D" |
| Human Review Time | < 30 phút/PR | Mỗi PR | > 2h → nhắc |
| Defect Escape Rate | < 5% | Mỗi sprint | > 10% → review |
| Test Coverage | ≥ 80% | Mỗi PR | < 70% → block merge |
| Risk Detection Rate | ≥ 70% | Mỗi sprint | < 50% → improve |
| Q&A Resolution Time | < 48h (High) | Mỗi sprint | High > 72h → escalate |
| CR Throughput | Tăng dần | Mỗi sprint | Giảm 2 sprint → review |

## Thiết lập Baseline

1. **Sprint 1–2:** Đo metrics ở trạng thái hiện tại
2. **Ghi baseline:** `openspec/deliverables/baseline.md`
3. **So sánh:** Từ Sprint 3, so với baseline mỗi sprint
4. **Adjust:** Sau 3 sprint, điều chỉnh target nếu cần

## Quy tắc

1. **AI tự tính metrics** từ data thực (CI, Git, tasks.md, test results)
2. **Đưa vào RADIO "O" (Outcome):** Mỗi RADIO report phải có ít nhất 2 metrics
3. **Đưa vào deliverables.md:** Summary table cuối file
4. **Alert khi dưới target:** AI flag trong RADIO "D" nếu metric dưới target
5. **Trend tracking:** So sánh với sprint/phase trước để thấy xu hướng
