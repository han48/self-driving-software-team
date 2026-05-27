---
inclusion: manual
---

# Skill: Pre-Approve Check – Risk

## Mục đích
Verify risk entry trước khi đưa Human confirm.

## Input
Risk entry trong `openspec/risk-issue/risks.md`.

## Checklist tự động

### Format
- [ ] Có heading `### RISK-XXX: [Tên]`
- [ ] Có `**Ngày phát hiện:**` (format YYYY-MM-DD)
- [ ] Có `**Xác suất:**` (High/Medium/Low)
- [ ] Có `**Ảnh hưởng:**` (High/Medium/Low)
- [ ] Có `**Mức độ:**` (Critical/High/Medium/Low)
- [ ] Có `**Mô tả:**` (không rỗng)
- [ ] Có `**Hành động phòng ngừa (Prevention):**` (cụ thể, actionable)
- [ ] Có `**Hành động giảm thiểu (Mitigation):**` (cụ thể, actionable)
- [ ] Có `**Owner:**`
- [ ] Có `**Trigger:**`
- [ ] Có `**Status:**`

### Quality
- [ ] Prevention ≠ Mitigation (2 hành động khác nhau)
- [ ] Prevention giảm xác suất xảy ra (không phải giảm hậu quả)
- [ ] Mitigation giảm hậu quả nếu xảy ra (không phải giảm xác suất)
- [ ] Trigger là dấu hiệu observable (không phải "khi xảy ra")
- [ ] Mức độ = Xác suất × Ảnh hưởng (logic đúng)
- [ ] Không duplicate với RISK đã tồn tại

### Traceability
- [ ] Nếu liên quan đến spec → có reference
- [ ] Nếu liên quan đến CR → có reference

## Output format

```markdown
# Pre-Approve Check: Risk

**Risk:** RISK-XXX
**Kết quả:** X/Y items PASS

## Validation

| Check | Status | Note |
|-------|--------|------|
| Prevention actionable | ✅ | "Thêm circuit breaker" – cụ thể |
| Mitigation actionable | ✅ | "Fallback sang cache" – cụ thể |
| Prevention ≠ Mitigation | ✅ | Khác nhau |
| Mức độ logic | ❌ | High × Low = Medium, nhưng ghi Critical |

## Issues

1. **Mức độ:** High × Low nên là Medium, không phải Critical

## Recommendation

- [ ] Sửa mức độ → đưa Human confirm
```
