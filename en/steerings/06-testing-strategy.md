---
inclusion: fileMatch
fileMatchPattern: '**/test*'
---

# Steering: Testing Strategy

## When to Apply
When AI writes tests or generates manual test cases.

## Rules

1. **6 test levels:**

   | Level | Type | Performed by | When |
   |-------|------|-------------|------|
   | L1 | Unit Test | AI | Every commit (CI) |
   | L2 | Integration Test | AI | Every PR |
   | L3 | E2E Automation (UI) | AI | Every PR / nightly |
   | L4 | Desktop/Native Automation | AI | Nightly / release |
   | L5 | Manual Test | QA/Tester (Human) | Before release |
   | L6 | Property-Based Test | AI | Every PR (optional) |

2. **AI generates all:**
   - Test scripts (L1–L4, L6)
   - Manual test cases for QA (L5)
   - AI runs all automation in CI/CD

3. **E2E tools:** Selenium, Playwright, browser-use (web), AutoIt, RobotFramework, browser-use/desktop (native)

4. **Manual test (L5) required when:**
   - UX/usability testing
   - Exploratory testing
   - Cross-device visual check
   - Business flow requiring human judgment
   - Accessibility testing

5. **Property-Based Test (L6):**
   - Generated from Correctness Properties in design.md
   - Format: `*For any* [input], [property holds]`
   - Tools: fast-check (JS), Hypothesis (Python), QuickCheck (Haskell)

6. **Coverage target:** ≥ 80% for unit + integration
