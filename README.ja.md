# Self-Driving Software Team – AI-First、Spec-Drivenプロセス

> **人間が承認し責任を負う。AIが定義、実行、報告する。**

AI-firstデリバリーモデルへの移行を目指すソフトウェアチーム向けの包括的なプロジェクト管理フレームワーク。RACI、RADIOレポーティング、Specification駆動開発、AIエージェントを統合したシステム。

🌐 **言語:** [Tiếng Việt](README.vi.md) | [English](README.md) | 日本語

---

## 概要

このフレームワークはソフトウェアプロジェクト管理を以下のように変革します：

```
「人が作業するのを管理」→「AIシステムが作業を実行するのを管理」
```

**4つの柱：**

| 柱 | 機能 |
|----|------|
| **RACI** | 人間とAI間の明確な責任分担 |
| **RADIO** | 標準化されたレポート、AIによる自動化 |
| **Spec** | 唯一の真実の源、AIへの入力 |
| **AIエージェント** | 自動的に実行、検証、報告 |

---

## ドキュメント構造

```
📁 project-processing/
├── README.md                    ← 英語README
├── README.vi.md                 ← ベトナム語README
├── README.ja.md                 ← ここ（日本語）
├── 📁 vi/                       ← ベトナム語（原本、完全版）
│   ├── self-driving-software-team.md   ← メインドキュメント（2800行以上）
│   ├── quick-start.md
│   ├── toolchain.md
│   └── prompt-templates.md
├── 📁 en/                       ← 英語（サマリー＋補足）
│   ├── self-driving-software-team.md   ← サマリー翻訳
│   ├── quick-start.md
│   ├── toolchain.md
│   └── prompt-templates.md
├── 📁 ja/                       ← 日本語（サマリー＋補足）
│   ├── self-driving-software-team.md   ← サマリー翻訳
│   ├── quick-start.md
│   ├── toolchain.md
│   └── prompt-templates.md
├── 📁 steerings/                ← AIステアリングファイル（19ファイル）
└── 📁 skills/                   ← AIスキルファイル（7ファイル）
```

---

## クイックスタート

1. [`ja/quick-start.md`](ja/quick-start.md) を読む（15分）
2. ツールチェーンをセットアップ：[`ja/toolchain.md`](ja/toolchain.md)
3. プロンプトテンプレートを使用：[`ja/prompt-templates.md`](ja/prompt-templates.md)
4. 完全な詳細はメインドキュメント参照：[`ja/self-driving-software-team.md`](ja/self-driving-software-team.md)

---

## 主な特徴

- **Spec駆動：** すべてのfeatureは正式なspecification（EARS記法）から開始
- **AI実行：** AIがspec、design、tasks、code、test、reportを生成
- **人間が承認：** 人間がすべてのゲートでレビューし承認
- **RADIOレポーティング：** 実行データに基づく自動進捗レポート
- **リスク管理：** AIが5WHY分析でリスクと課題を自動検出
- **CR管理：** バックログトラッキング付きの構造化されたCRライフサイクル
- **Q&A管理：** Spec明確化のためのスレッド型Q&A
- **Definition of Done：** すべての成果物とフェーズに明確なDoD
- **導入レベル：** 基本からフルAI自律までの4レベルロードマップ

---

## 対象者

| 役割 | 使い方 |
|------|--------|
| **プロジェクトマネージャー** | ガバナンス、KPI、エスカレーション、CR承認 |
| **テックリード** | Spec、Design、PRの承認；技術的意思決定 |
| **BA** | 要件提供、ビジネスロジック承認 |
| **QA/テスター** | 手動テスト（L5）、テストカバレッジレビュー |
| **開発者** | 技術コンサルテーション、必要時にAIをオーバーライド |
| **AIエージェント** | すべてを実行（spec → code → test → report） |

---

## バージョン

**現在：** v1.2 (2026-05-21)

| バージョン | 日付 | 変更内容 |
|-----------|------|----------|
| 1.0 | 2026-05-21 | 初版 |

---

## 著者

**DungDV (Mr4)** – Sabitech

---

## ライセンス

内部使用。配布権については著者に連絡してください。
