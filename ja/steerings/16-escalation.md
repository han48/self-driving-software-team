---
inclusion: auto
---

# Steering: エスカレーションマトリクス＆コミュニケーションプロトコル

## 適用タイミング
AIが問題をエスカレーションする時、Humanが応答しない時、役割間のコミュニケーション時。

## エスカレーションマトリクス

### Severity別

| Severity | エスカレーション先 | SLA応答 | SLA対応 | チャネル |
|----------|----------------|--------------|-----------|------|
| **Critical** | PM + Tech Lead + Stakeholder | 1時間 | 4時間 | Slack urgent + Phone |
| **High** | Tech Lead + PM | 4時間 | 24時間 | Slack channel |
| **Medium** | Tech Lead | 24時間 | 3日 | Slack / Email |
| **Low** | 記録、帯域がある時に対応 | 48時間 | Next sprint | Email / Backlog |

### 問題種類別

| 問題 | エスカレーションパス | トリガー |
|--------|--------------|---------|
| Production down | AI → Tech Lead → PM → Stakeholder | 即座に |
| Risk Highが発生 | AI → Tech Lead → PM | 4時間以内 |
| Q&A High期限超過 (> 48h) | AI flag RADIO "D" → PM escalate顧客 | 48h後 |
| AI fail > 3回 | AI → Tech Lead (Human override) | 3回retry後 |
| Specコンフリクト | AI → Tech Lead + BA | 検出時即座に |
| セキュリティ脆弱性 | AI → Tech Lead → Security Engineer | 即座に |
| Human承認未応答 | AIリマインド (24h) → リマインド (48h) → escalate PM | 48h後 |

## コミュニケーションプロトコル

### コミュニケーションチャネル

| 種類 | チャネル | SLA | 例 |
|------|------|-----|-------|
| 承認リクエスト | Slack + In-tool | 4h (High)、24h (Medium) | "PR ready"、"Spec承認必要" |
| 確認事項 | Slack thread + QAファイル | 24h (High)、48h (Medium) | "Specが曖昧" |
| エスカレーション | Slack urgent / Phone | 1h (Critical)、4h (High) | "Blocker" |
| 情報共有 | Email / Channel | 応答不要 | "新しいRADIO" |

### Humanが応答しない場合

```
0–24h:  AIは待機、依存しない他のタスクを実行
24–48h: AIが再リマインド + RADIO "D"に記録
48–72h: AIがPMにescalate + ISSUEを作成
> 72h:  AIがfeatureを一時停止、blockerとして報告
```

## ルール

1. **Async-first:** デフォルトは非同期、同期はCriticalの場合のみ
2. **Thread-based:** 各トピックに1スレッド
3. **Actionable:** 各メッセージに明記: 誰が何を、期限はいつ
4. **Recorded:** 決定はspec/QA/RADIOに記録必須
5. **AI自動通知:** 承認が必要な時AIが通知を送信
6. **レベルスキップ禁止:** 順序に従いescalate (Criticalを除く)
