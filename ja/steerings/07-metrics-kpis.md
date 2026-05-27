---
inclusion: fileMatch
fileMatchPattern: '**/radio.md'
---

# Steering: メトリクス＆KPIトラッキング

## 適用タイミング
AIがRADIOレポート生成、成果物統合、またはプロセス効果評価時。

## 追跡すべきKPI

| メトリクス | 目標 | 頻度 | アラート |
|-----------|------|------|---------|
| Spec Compliance Rate | ≥ 95% | 各PR | < 90% → RADIO "D" |
| RADIO Accuracy | ≥ 90% | 各sprint | < 80% → review |
| AI Task Completion Rate | ≥ 80% | 各sprint | < 60% → escalate |
| Time to Delivery | baselineから50%削減 | 各feature | > 2x baseline → "D" |
| Human Review Time | < 30分/PR | 各PR | > 2h → リマインド |
| Defect Escape Rate | < 5% | 各sprint | > 10% → review |
| Test Coverage | ≥ 80% | 各PR | < 70% → merge禁止 |
| Risk Detection Rate | ≥ 70% | 各sprint | < 50% → improve |
| Q&A Resolution Time | < 48h (High) | 各sprint | High > 72h → escalate |
| CR Throughput | 漸増 | 各sprint | 2sprint連続減少 → review |

## ベースラインの確立

1. **Sprint 1–2:** 現状のメトリクスを測定
2. **ベースライン記録:** `openspec/deliverables/baseline.md`
3. **比較:** Sprint 3以降、各sprintでbaselineと比較
4. **調整:** 3sprint後、必要に応じて目標を調整

## ルール

1. **AIが自動でメトリクスを計算** – 実データから (CI、Git、tasks.md、test results)
2. **RADIO "O" (Outcome) に記載:** 各RADIOレポートに少なくとも2つのメトリクスを含む
3. **deliverables.mdに記載:** ファイル末尾にサマリーテーブル
4. **目標未達時にアラート:** メトリクスが目標以下の場合、AIがRADIO "D"にフラグ
5. **トレンド追跡:** 前sprint/phaseと比較して傾向を把握

