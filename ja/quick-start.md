# クイックスタートガイド – 15分で始める

> AI-First、Spec-Drivenプロセスを初めて導入するチーム向け。メインドキュメントを全て読まなくてもすぐに試せます。

---

## ステップ1：準備（5分）

```bash
# フォルダ構造を作成
mkdir -p openspec/specs openspec/bugs openspec/change-request openspec/risk-issue openspec/qa openspec/deliverables
```

## ステップ2：要件を提供（2分）

AIエージェントのプロンプトに生の要件を入力します。例：
> 「ユーザー登録APIが必要。email + password + nameを受け取り、入力をバリデーションし、DBに保存し、確認メールを送信する。」

標準フォーマットは不要 – チケット、会議メモ、チャットメッセージでOK。曖昧な場合はAIが確認を求めます。

## ステップ3：AIがSpec生成 → あなたが承認（3分）

AIが `openspec/specs/{feature}/requirements.md` を生成します。あなたは：
1. 受入基準を読む（EARS形式：THE/WHEN/IF + SHALL）
2. エッジケースが網羅されているか確認
3. 承認またはchanges要求

## ステップ4：AIがDesign生成 → あなたが承認（3分）

AIが `design.md` + `gui/`（HTMLプロトタイプ）を生成します。あなたは：
1. HTMLプロトタイプをブラウザで開き、フローをクリックして確認
2. アーキテクチャ図をレビュー
3. 承認またはchanges要求

## ステップ5：AIがTasks生成 → 承認 → AI実行（2分）

AIが `tasks.md` を生成、あなたが承認、AIがコーディング＋テストを開始。

## ステップ6：結果をレビュー

AIがRADIOレポートを生成。PR + RADIOをレビュー → 承認またはchanges要求。

---

## 一行サマリー

```
生の要件 → [AI: Spec] → ✅ 承認 → [AI: Design] → ✅ 承認 → [AI: Tasks] → ✅ 承認 → [AI: Code+Test] → [AI: RADIO] → ✅ 最終承認
```

---

## 「準備完了」チェックリスト

- [ ] `openspec/` フォルダ作成済み
- [ ] AIエージェント設定済み（Kiro/Claude）
- [ ] チームが理解：AIが実行、人間が承認
- [ ] 少なくとも1人が承認チェックリストを把握（メインドキュメントSection 14参照）
- [ ] CI/CDパイプラインが自動テスト実行可能

---

## 次のステップ

小さなfeatureで1回成功した後：
1. メインドキュメント（`self-driving-software-team.md`）を読んで深く理解
2. Adoption Level 1を適用（Section 5）
3. Level 1 → 4のロードマップに沿って段階的に拡大
