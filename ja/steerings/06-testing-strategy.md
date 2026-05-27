---
inclusion: fileMatch
fileMatchPattern: '**/test*'
---

# Steering: テスト戦略

## 適用タイミング
AIがテストを書くまたは手動テストケースを生成する時。

## ルール

1. **6つのテストレベル:**

   | レベル | 種類 | 実行者 | タイミング |
   |--------|------|--------|-----------|
   | L1 | Unit Test | AI | 各commit (CI) |
   | L2 | Integration Test | AI | 各PR |
   | L3 | E2E Automation (UI) | AI | 各PR / nightly |
   | L4 | Desktop/Native Automation | AI | Nightly / release |
   | L5 | Manual Test | QA/Tester (Human) | リリース前 |
   | L6 | Property-Based Test | AI | 各PR (optional) |

2. **AIがすべて生成:**
   - テストスクリプト (L1–L4、L6)
   - QA向け手動テストケース (L5)
   - AIがCI/CD内ですべての自動テストを実行

3. **E2Eツール:** Selenium、Playwright、browser-use (web)、AutoIt、RobotFramework、browser-use/desktop (native)

4. **手動テスト (L5) が必要な場合:**
   - UX/ユーザビリティテスト
   - 探索的テスト
   - クロスデバイスのビジュアルチェック
   - 人間の判断が必要なビジネスフロー
   - アクセシビリティテスト

5. **Property-Based Test (L6):**
   - design.mdのCorrectness Propertiesから生成
   - フォーマット: `*For any* [input], [property holds]`
   - 使用: fast-check (JS)、Hypothesis (Python)、QuickCheck (Haskell)

6. **カバレッジ目標:** unit + integrationで ≥ 80%

