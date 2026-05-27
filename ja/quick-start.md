# クイックスタートガイド – 15分で始める

> AI-First、Spec-Drivenプロセスを、メインドキュメントを全て読まずにすぐ試したい新しいチーム向け。

---

## ステップ1：準備（5分）

```bash
# ディレクトリ構造を作成
mkdir -p openspec/specs openspec/bugs openspec/change-request openspec/risk-issue openspec/qa openspec/deliverables
```

## ステップ2：要件を提供（2分）

AI Agentのプロンプトに生の要件を記述します。例：
> 「ユーザー登録APIが必要。email + password + nameを受け取り、入力をバリデーションし、DBに保存し、確認メールを送信する。」

標準フォーマットは不要 – チケット、ミーティングノート、チャットでも可。曖昧な場合はAIが確認を求めます。

## ステップ3：AIがSpec生成 → あなたが承認（3分）

AIが`openspec/specs/{feature}/requirements.md`を生成します。あなたは：
1. 受入基準を読む（EARS形式：THE/WHEN/IF + SHALL）
2. エッジケースが十分かチェック
3. 承認または変更を要求

## ステップ4：AIが設計生成 → あなたが承認（3分）

AIが`design.md` + `gui/`（HTMLプロトタイプ）を生成。あなたは：
1. ブラウザでHTMLプロトタイプを開き、フローをクリックして確認
2. アーキテクチャ図をレビュー
3. 承認または変更を要求

## ステップ5：AIがタスク生成 → 承認 → AI実行（2分）

AIが`tasks.md`を生成、あなたが承認、AIがコーディング＋テストを開始。

## ステップ6：結果をレビュー

AIがRADIOレポートを生成。あなたはPR + RADIOをレビュー → 承認または変更を要求。

---

## 一行まとめ

```
生の要件 → [AI: Spec] → ✅ 承認 → [AI: Design] → ✅ 承認 → [AI: Tasks] → ✅ 承認 → [AI: Code+Test] → [AI: RADIO] → ✅ 最終承認
```

---

## 「準備完了」チェックリスト

- [ ] `openspec/`フォルダを作成済み
- [ ] AI Agentをセットアップ済み（Kiro/Claude）
- [ ] チームが理解している：AIが実行、人間が承認
- [ ] 少なくとも1人が承認チェックリストを把握（メインドキュメントのSection 14参照）
- [ ] CI/CDパイプラインで自動テストが実行される

---

## 次のステップ

小さな機能1つを正常に実行した後：
1. メインドキュメント（`self-driving-software-team.md`）を読んで深く理解する
2. Adoption Level 1を適用（Section 5）
3. Level 1 → 4のロードマップに従って徐々にスコープを拡大
