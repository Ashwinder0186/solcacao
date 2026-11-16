# SolCacao – Phase 2 Browser Tests (Team 14, CSE 5335)

Automated browser test suite for the SolCacao React front end, written with Playwright (Python) and run in Chromium.

## Contents
- `tests/browser/run_browser_tests.py` – test suite (91 test cases: navigation, catalog and filters, product page, cart, checkout validation, login/account, sampler, B2B pages, contact form, EN/ES language switch, and layout at 375, 768, 1440 and 320 px)
- `results/results.json` – results of the run: every test case with steps, expected result, actual result, status, viewport and screenshot name
- `results/run.log` – console output of the run
- `results/screenshots/` – screenshot captured for each test case

## Results
- 91 test cases: 88 PASS, 3 EXPECTED FAIL (Phase 3)
- The 3 expected failures (CHK-16, CHK-17, B2B-09) concern order and quote reference numbers, which will come from the backend API and database in Phase 3.
- Cart at 375/320 px and Checkout at 320 px were checked manually on the deployed site.

## How to run
```
pip install playwright
playwright install chromium
python3 tests/browser/run_browser_tests.py <site-url> <output-folder> <label>
# example
python3 tests/browser/run_browser_tests.py http://localhost:5173 results phase2
```
Start the app first (`cd client && npm install && npm run dev`) or pass the live URL. Test data is dummy data only; no payment is processed and nothing is sent to a server.
