---
inclusion: fileMatch
fileMatchPattern: '**/requirements.md'
---

# Steering: Spec（要件）の作成

## 適用タイミング
AIが新機能またはCRのrequirements.mdを生成する時。

## ルール

1. **必須フォーマット:**
   - Heading: `# Requirements Document`
   - Sections: `## Introduction` → `## Glossary` → `---` → `## Requirements`
   - 各requirement: `### Requirement N: [名前]` + `**User Story:**` + `#### Acceptance Criteria`

2. **Acceptance CriteriaにはEARS Notation必須:**
   - `THE [Component] SHALL [behavior]` – 常に成立する動作
   - `WHEN [condition] THE [Component] SHALL [behavior]` – トリガーあり
   - `IF [condition] THEN THE [Component] SHALL [behavior]` – 条件あり
   - Component名は大文字: THE API、THE System、THE Admin_Panel...

3. **Human承認前のセルフチェックループ:**
   - Specは明確で完全か？
   - AC間に論理的矛盾はないか？
   - 技術的な困難はないか？
   - リスク検出 → `openspec/risk-issue/risks.md`にRISK-XXXを作成
   - 曖昧さ検出 → `openspec/qa/`にQA-XXXを作成
   - 問題がなくなるまで繰り返す

4. **品質:**
   - 各ACはテスト可能 (テストケースに変換可能)
   - happy pathとerror casesの両方を含む
   - 隠れた前提なし – すべての仮定を明記
   - Glossaryですべての専門用語を定義

5. **出力場所:** `openspec/specs/{feature-name}/requirements.md`

