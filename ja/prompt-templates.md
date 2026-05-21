# プロンプトテンプレート – 各フェーズのAIプロンプト

> プロセスの正しいフォーマットでAI出力を生成する必要がある時にこれらのテンプレートを使用してください。
> `{...}` を実際の内容に置き換えてください。

---

## 1. Requirements生成（Spec）

### 基本プロンプト

```
以下のfeatureのrequirements.mdを生成してください：

{生の要件記述 – 1-2文または会議メモ}

要件：
- EARS記法を使用（THE/WHEN/IF + SHALL）
- 各requirementにUser Storyを含める
- ハッピーパスとエラーケースの両方をカバー
- ドメイン固有用語のGlossaryを定義
- 受入基準に番号付け（1, 2, 3...）
- 曖昧さ検出時 → QA-XXXを作成
- リスク検出時 → RISK-XXXを作成

出力フォーマット：付録A.2の構造に従う
ファイル場所：openspec/specs/{feature-name}/requirements.md
```

### 上級プロンプト（コンテキスト付き）

```
コンテキスト：
- プロジェクト：{プロジェクト名}
- 技術スタック：{Node.js + PostgreSQL + React...}
- 既存Spec：{関連Specへの参照（あれば）}
- 制約：{期限、予算、技術的制限}

クライアントからの生の要件：
"{元の要件をコピーペースト}"

requirements.mdを生成してください。注意：
- 現在のアーキテクチャと互換性があること
- 既存Specとの競合をチェック
- 契約スコープを超える場合はフラグ
```

---

## 2. Design生成

### 基本プロンプト

```
承認済みrequirements.md（ファイル：openspec/specs/{feature}/requirements.md）に基づき、
以下を含むdesign.mdを生成してください：

1. Overview（技術スタック、アプローチ）
2. Site Map（Mermaidグラフ）
3. GUI Design（gui/フォルダにHTMLプロトタイプ生成）
4. Architecture（シーケンス図）
5. Database Design + ERD
6. Infrastructure Design
7. Components & Interfaces
8. Correctness Properties（プロパティベーステスト用）
9. Error Handling戦略
10. Testing Strategy

要件：
- 各コンポーネントは対応するrequirementを参照すること
- GUIプロトタイプはブラウザで直接開けること
- Database designはインデックス戦略を含むこと
- Correctness Propertiesは"For any..."形式

出力フォーマット：付録A.3の構造に従う
```

### 小規模feature用プロンプト（セクション省略）

```
Feature規模：Small（1日未満）
design.mdは以下のセクションのみ生成：
- Overview
- Components & Interfaces
- Testing Strategy

省略：Site Map, GUI, Architecture, Database, Infrastructure
（Featureが小さすぎるためフルデザイン不要）
```

---

## 3. Tasks生成

### 基本プロンプト

```
承認済みdesign.md（ファイル：openspec/specs/{feature}/design.md）に基づき、
tasks.mdを生成してください：

要件：
- 各タスク4時間未満の工数
- ネストチェックボックス形式（- [ ] 1. / - [ ] 1.1）
- 各サブタスクに _Requirements: X.X_ 参照
- Task Dependency Graph（JSON waves）
- コード、ユニットテスト、統合テスト、E2Eテストのタスクを含む
- 論理的順序：セットアップ → コアロジック → テスト → 統合

出力フォーマット：付録A.4の構造に従う
```

---

## 4. RADIOレポート生成

### 基本プロンプト

```
feature {名前} のRADIOレポートを生成/更新してください：

データソース：
- tasks.md：{X}/{Y} タスク完了
- CI結果：{pass/fail, カバレッジ %}
- ブロッカー：{リスト（あれば）}
- 最近の変更：{最近のコミット}

フォーマット：
- R：ステータス + 具体的メトリクス
- A：現在のタスク + 次のステップ
- D：ブロッカー/リスク（または"None"）
- I：進捗に影響するコンテキスト（または"—"）
- O：達成結果 + 数値

深刻なD項目がある場合 → RISK-XXX/ISSUE-XXXが存在するか確認、なければ作成。
```

---

## 5. Bugfix Spec生成

### 基本プロンプト

```
バグレポート："{バグの説明}"
再現手順：{手順}
期待動作：{期待される動作}
実際の動作：{実際の動作}

bugfix.mdを生成：
1. Current Behavior (Defect) – EARS形式
2. Expected Behavior (Correct) – EARS形式
3. Unchanged Behavior (Regression Prevention) – EARS形式

次にdesign.mdを生成：
- 5WHY根本原因分析（実際のroot causeまで掘り下げ）
- Fix Implementation（外科的、最小限の変更）
- Files Changed（明確な表）
- Prevention（このタイプのバグが再発しないようにする方法）
- Correctness Properties

出力：openspec/bugs/{bug-name}/bugfix.md + design.md + tasks.md
```

---

## 6. CR（Change Request）評価

### 基本プロンプト

```
新しいCR受領：
- 送信元：{ソース}
- 説明："{CR内容}"
- 詳細：{箇条書き}

以下を実施：
1. 現在のシステムへの影響を評価
2. 工数見積もり（時間/日）
3. 実装時のリスクを評価
4. 優先度を提案（High/Medium/Low）理由付き
5. 既存specs/designとの競合をチェック
6. backlog.mdの"Pending"セクションに正しいフォーマットで追加
```

---

## 7. リスク＆課題検出

### リスク検出プロンプト

```
{ファイル：spec/design/code} をレビューしてリスクを検出してください：

チェック項目：
- セキュリティリスク（インジェクション、認証バイパス、データ漏洩）
- パフォーマンスリスク（N+1、メモリリーク、タイムアウト）
- スケーラビリティリスク（単一障害点、ボトルネック）
- 依存関係リスク（非推奨ライブラリ、破壊的変更）
- ビジネスリスク（スコープクリープ、不明確な要件）

検出された各リスクについて：
- 確率（H/M/L）+ 影響（H/M/L）を評価
- Prevention（回避方法）を記述
- Mitigation（発生時の対応）を記述
- Trigger（早期警告サイン）を特定

出力：openspec/risk-issue/risks.mdにRISK-XXX形式で追加
```

---

## 8. Q&A生成

### 曖昧さ検出時のプロンプト

```
spec/designを読んでいて曖昧な点を発見しました：
"{曖昧さの説明}"

以下を実施：
1. 明確な質問とコンテキストでQA-XXXを作成
2. 可能なオプションをリスト（わかれば）
3. デフォルト回答を提案（推論可能であれば）
4. 回答者を特定（クライアント / アーキテクト / リード）
5. 優先度を評価（実装をブロックする場合はHigh）

出力：openspec/qa/QA-XXX/thread.md + index.mdを更新
```

---

## プロンプト使用のヒント

1. **コンテキストは多いほど良い：** 関連ファイルを添付、説明だけでなく
2. **既存Specを参照：** 関連Specがあれば言及する
3. **制約を明示：** 期限、技術スタック、チーム規模は出力に影響
4. **反復的に：** 出力が正しくない場合 → 具体的フィードバック、最初からやり直さない
5. **フォーマット確認：** AI生成後 → 付録A/Bのフォーマットと一致するか確認
