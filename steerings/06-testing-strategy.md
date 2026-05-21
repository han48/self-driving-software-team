---
inclusion: fileMatch
fileMatchPattern: '**/test*'
---

# Steering: Testing Strategy

## Khi nào áp dụng
Khi AI viết tests hoặc sinh manual test cases.

## Quy tắc

1. **6 tầng test:**

   | Tầng | Loại | Ai thực hiện | Khi nào |
   |------|------|-------------|---------|
   | L1 | Unit Test | AI | Mỗi commit (CI) |
   | L2 | Integration Test | AI | Mỗi PR |
   | L3 | E2E Automation (UI) | AI | Mỗi PR / nightly |
   | L4 | Desktop/Native Automation | AI | Nightly / release |
   | L5 | Manual Test | QA/Tester (Human) | Trước release |
   | L6 | Property-Based Test | AI | Mỗi PR (optional) |

2. **AI sinh tất cả:**
   - Test scripts (L1–L4, L6)
   - Manual test cases cho QA (L5)
   - AI chạy tất cả automation trong CI/CD

3. **E2E tools:** Selenium, Playwright, browser-use (web), AutoIt, RobotFramework, browser-use/desktop (native)

4. **Manual test (L5) cần khi:**
   - UX/usability testing
   - Exploratory testing
   - Cross-device visual check
   - Business flow cần human judgment
   - Accessibility testing

5. **Property-Based Test (L6):**
   - Sinh từ Correctness Properties trong design.md
   - Format: `*For any* [input], [property holds]`
   - Dùng: fast-check (JS), Hypothesis (Python), QuickCheck (Haskell)

6. **Coverage target:** ≥ 80% cho unit + integration
