---
inclusion: fileMatch
fileMatchPattern: '**/design.md'
---

# Steering: デザインの作成

## 適用タイミング
AIが承認済み要件からdesign.mdを生成する時。

## ルール

1. **必須フォーマット:**
   - Heading: `# Design Document: [Feature名]`
   - Sections順序: Overview → Site Map → GUI Design → Architecture → Data Models / Database Design + ERD → Infrastructure Design → Components and Interfaces → Correctness Properties → Error Handling → Testing Strategy

2. **GUI Design:**
   - design.mdに直接記述しない
   - `openspec/specs/{feature-name}/gui/`にHTMLプロトタイプを作成
   - design.mdでは参照のみ: `> 📁 Refer: openspec/specs/{feature-name}/gui/`
   - HTMLプロトタイプは完全なナビゲーションフロー、クリック可能、レスポンシブ
   - 軽量CSSフレームワーク使用 (TailwindCSS、Bootstrap)

3. **Architecture:**
   - Mermaid diagrams使用 (graph TDでシステム、sequenceDiagramでフロー)
   - システム概要とシーケンス図の両方を含む

4. **Correctness Properties:**
   - フォーマット: `*For any* [condition], [property must hold]`
   - 各propertyに必須: `**Validates: Requirements X.X**`

5. **Human承認前のセルフチェックループ:**
   - Designはすべてのrequirementsをカバーしているか？
   - Architectureはスケーラブルでセキュアか？
   - DB designは正しい正規化か？
   - コンポーネント間にコンフリクトはないか？
   - 技術的リスクはないか？ → RISK-XXXを作成
   - 確認が必要な質問はないか？ → QA-XXXを作成
   - 問題がなくなるまで繰り返す

6. **出力場所:** `openspec/specs/{feature-name}/design.md`

