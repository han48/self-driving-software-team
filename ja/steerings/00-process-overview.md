---
inclusion: auto
---

# Steering: AI-First、Spec-Drivenプロセス（概要）

## コア原則

> 人間が承認し責任を負う。AIが定義、実行、報告する。

## メインワークフロー

```
Humanが粗い要件を提供
    ↓
AIがSpec生成 (requirements.md) → 🔄 AIセルフチェック → Human承認
    ↓
AIがDesign生成 (design.md + gui/) → 🔄 AIセルフチェック → Human承認
    ↓
AIがTasks生成 (tasks.md) → Human承認
    ↓
AIが実行 (code + test L1–L4, L6)
    ↓
AIが検証 (spec compliance + test results)
    ↓
AIがRADIOレポート生成
    ↓
Human承認 (PR + RADIO)
```

## フォルダ構成

```
📁 openspec/
├── 📁 specs/{feature}/        ← Feature specs + gui/
├── 📁 bugs/{bug}/             ← Bugfix specs
├── 📁 change-request/         ← CRバックログ + specs
├── 📁 risk-issue/             ← risks.md + issues.md
├── 📁 qa/                     ← Q&Aインデックス + threads
└── deliverables.md            ← 成果物 + 制限事項
```

## AIが作業中に自動作成するもの

- **Risk:** 潜在的リスクを検出した時 → `openspec/risk-issue/risks.md`
- **Issue:** 発生中の問題を検出した時 → `openspec/risk-issue/issues.md`
- **Q&A:** specの曖昧さを検出した時 → `openspec/qa/QA-XXX/thread.md`
- **RADIO:** 各task/milestone後 → specフォルダ内またはインライン

## 10のゴールデンルール

1. フェーズをスキップしない: Requirements → Design → Tasks → Execute
2. 承認なしに実行しない
3. すべてのoutputはトレーサブル (Requirements/Bugfixを参照)
4. AIが自らrisk/issue/Q&Aを検出する – Humanを待たない
5. Humanは承認のみ、自ら作業しない (手動テストL5を除く)
6. AIは最大3回リトライ、その後Humanにエスカレーション
7. RADIOはアクティブ時に少なくとも1日1回更新
8. すべてのspec変更にはversion + change logが必要
9. SLAに従いエスカレーション: Critical 1h、High 4h、Medium 24h
10. Async-firstコミュニケーション、syncはCriticalの場合のみ

> 📖 略語・用語: `self-driving-software-team.md`のGlossaryを参照

