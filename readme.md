# Test methodology: three AI browser agents, three data analysis tasks, one session

**Working headline:** I ran three AI browser agents on the same data analysis tasks. Here's how they fared
**Agents:** Perplexity Comet, Gemini in Chrome (auto browse), Copilot in Microsoft Edge (Browse with Copilot)
**Format:** 3 tasks × 3 agents × 1 run = 9 runs, about 3 hours including setup

---

## Before test day

These carry over from the original plan and still apply.

- **Edge's agent is now called Browse with Copilot.** Microsoft retired Copilot Mode on May 13, 2026; the agentic feature is limited to Microsoft 365 Premium subscribers in the US on Edge desktop, with usage limits ([Microsoft Edge Blog](https://blogs.windows.com/msedgedev/2026/05/13/new-updates-to-edge-across-desktop-and-mobile/)). Test from New York on US accounts.
- **Plans, roughly $20 each:** Perplexity Pro (Comet Agent), Google AI Pro (Chrome auto browse, up to 20 requests a day ([Google](https://support.google.com/gemini/answer/16821166))), Microsoft 365 Premium (Browse with Copilot). Confirm and screenshot all three prices at checkout.
- **Comet's consumer agent allowance isn't published.** Screenshot what your account page shows. Nine runs won't strain any of the three quotas.
- **Host the datasets on a neutral URL** ([Netlify Drop](https://app.netlify.com/drop) is quickest). Upload only the `task2_` and `task3_` HTML, CSV and PDF files. Never upload the answer key or `task3_reference_charts.png`.

---

## Session plan

| Time | Step |
|---|---|
| 0:00–0:30 | **Setup.** Fresh browser profile per agent, no extensions, memory and history personalization off, same network, no VPN. Start screen recording with a visible clock. |
| 0:30–0:50 | **Ground truth for Task 1.** Capture the three pricing pages by hand (see Task 1). |
| 0:50–1:30 | **Task 1** × 3 agents |
| 1:30–2:10 | **Task 2** × 3 agents |
| 2:10–2:50 | **Task 3** × 3 agents |
| 2:50–3:00 | Fill in `scoring_sheet.csv` while it's fresh. Detailed scoring later, from recordings. |

**Run order rotates** so no agent always goes first:

| | 1st | 2nd | 3rd |
|---|---|---|---|
| Task 1 | Comet | Gemini | Edge |
| Task 2 | Gemini | Edge | Comet |
| Task 3 | Edge | Comet | Gemini |

---

## Rules (these go in the "How I tested" box)

1. **Same prompt, verbatim, every agent.** No rewording, even if one struggles.
2. **Agent mode, new thread, every run.** If a product answers in chat mode instead of operating the browser, record it. That's a finding.
3. **Hands off.** Only answer clarifying questions or approve harmless prompts. Never point it to the right page or fix a number. Count every intervention.
4. **12-minute cap.** Stop the agent and score what exists.
5. **No logins, purchases or form submissions.** None of the tasks needs one.

**A note on sample size:** one run per agent means the article reports what happened, not success rates. "Gemini caught the outlier; Comet charted it as a boom month" is honest. "Gemini is 100% accurate" isn't.

---

## Task 1: Compare SaaS vendor pricing (live web)

**Tests:** navigating JavaScript-heavy pricing pages, billing toggles, seat minimums, simple arithmetic.

**Ground truth (do this by hand first):** open the official pricing pages for **HubSpot Sales Hub, Pipedrive and monday CRM** in a clean, signed-out browser. Fill in `task1_pricing_ground_truth_template.csv` and screenshot each page with both the annual and monthly toggle states. Run all three agents within an hour of this capture.

**Prompt:**

> Go to the official pricing pages for HubSpot Sales Hub, Pipedrive, and monday CRM. For each one, find the lowest-priced paid plan and the next plan up. Record the price per user per month in US dollars, both when billed annually and when billed monthly, and note any minimum number of seats. Then calculate what the second-tier plan would cost per year for a team of 8 on annual billing. Put everything in one table, tell me which is cheapest for that team, and list the URL each price came from. If you can't confirm a number, say so instead of estimating.

**Watch for:** whether it actually flips the billing toggle, whether it applies seat minimums to the 8-seat math, and whether it pulls numbers from review sites instead of the vendor (scores zero for that cell).

**Accuracy (19 points):** 12 price cells, 3 seat-minimum cells, 3 calculations, 1 cheapest verdict.

---

## Task 2: Reconcile a small business statement (synthetic data)

**Tests:** cross-referencing two documents, and judgment about what counts as a real discrepancy.

**Files:** `task2_bank_statement_oct2026` (33 lines) and `task2_expense_ledger_oct2026` (25 entries) for Lantern & Loom Studio, a fictional design studio. Every page is labeled as sample data. Nine real discrepancies and two decoys are planted; see the answer key.

**Setup:** open the hosted statement in tab 1 and the ledger in tab 2 before pasting the prompt.

**Prompt (swap in your URLs):**

> I run a small design studio and need to reconcile October's business account. The bank statement is at [STATEMENT URL] and our expense ledger is at [LEDGER URL]. Match every ledger expense against the bank statement. Then list: (1) bank transactions missing from the ledger, (2) ledger entries missing from the bank statement, (3) amounts that don't match, with the difference, and (4) any duplicate charges, unexpected fees, or unrecorded refunds. Client deposits are income, so ignore them. Finally, confirm the statement's opening balance, debits and credits add up to its closing balance. Explain anything you flag.

**Watch for:** whether it combines the two WeWork charges into one match, whether it wrongly flags the two-day B&H Photo posting lag, and whether it invents transactions.

**Accuracy (11 points):** per the answer key, with deductions for false positives and fabrications.

---

## Task 3: Clean, chart and interpret a dataset (synthetic data)

**Tests:** spotting bad data before charting it, then reading what the charts actually say.

**File:** `task3_tallyhawk_saas_metrics` — 216 rows of monthly metrics for a fictional SaaS company. Planted: a 10x outlier, a missing value, and an unlabeled price increase that drives a churn spike.

**Where charts get made:** let each agent choose (its own panel or a free web tool) and note which. Don't steer anyone into Google Sheets or Excel.

**Prompt (swap in your URL):**

> The dataset at [DATASET URL] has monthly subscription metrics for a software company. First, check it for errors, outliers or missing values, and tell me what you found and how you handled it. Then create three charts: (A) total monthly recurring revenue over time by plan tier, (B) total MRR by region for the latest three months, and (C) monthly customer churn rate over time, with your formula. Finally, tell me the three most important things these charts reveal, including anything that looks like a problem.

**Watch for:** whether it catches the March 2026 outlier (it nearly doubles that month's MRR), whether it narrates the spike as good news, and whether it explains the June churn jump with the Starter price increase or just reports it.

**Accuracy (12 points):** per the answer key. Compare against `task3_reference_charts.png`.

---

## Scoring (100 points per run)

| Dimension | Points | How |
|---|---|---|
| Accuracy | 50 | Raw score ÷ task max × 50 |
| Autonomy | 20 | 20 with no help; −5 per intervention; 0 if it needed redirecting |
| Honesty | 20 | Cites sources, states formulas, admits gaps. **Any fabricated figure presented as fact scores 0 here and caps Accuracy at 25.** |
| Speed | 10 | Fastest correct run per task = 10; others scale as fastest ÷ yours × 10 |

Report a per-task winner, an overall score, and a "trust" tally of fabrications plus false positives. The trust tally is the number a finance lead will care about.

---

## Launch notes per agent

UI labels change often. Do one throwaway prompt per agent during setup and screenshot the controls.

- **Comet:** sign in with Perplexity Pro, turn off memory, disconnect email and calendar. Comet separates the Assistant (answers about pages) from the Agent (operates the browser), with separate limits. Make sure the run uses the Agent.
- **Gemini in Chrome:** sign in with Google AI Pro, confirm auto browse is allowed in Chrome settings, clear any auto sign-in sites. Gemini replies "Task started" when auto browse kicks in ([Mashable via AOL](https://www.aol.com/articles/google-chrome-unveils-gemini-powered-222514349.html)); a plain chat reply means it didn't. Stop button is in the panel ([Google](https://support.google.com/chrome/answer/16821166)).
- **Copilot in Edge:** sign in with Microsoft 365 Premium. In Settings > Copilot and AI > Browse with Copilot, make sure your data host and the three CRM domains aren't blocked ([Microsoft Support](https://support.microsoft.com/en-US/microsoft-copilot/browse-with-copilot)). Open Copilot from the top-right icon and select Browse with Copilot before pasting.

---

## Kit contents

| File | Agent can see it? |
|---|---|
| `METHODOLOGY_ai_browser_agents_data_tasks.md` | No |
| `ANSWER_KEY_do_not_upload.md` | **Never** |
| `task1_pricing_ground_truth_template.csv` | No |
| `task2_bank_statement_oct2026.html` / `.csv` / `.pdf` | Yes, hosted |
| `task2_expense_ledger_oct2026.html` / `.csv` | Yes, hosted |
| `task3_tallyhawk_saas_metrics.html` / `.csv` | Yes, hosted |
| `task3_reference_charts.png` | **Never** |
| `scoring_sheet.csv` (9 rows, pre-ordered) | No |

---

## Sources

- [New updates to Edge across desktop and mobile — Microsoft Edge Blog](https://blogs.windows.com/msedgedev/2026/05/13/new-updates-to-edge-across-desktop-and-mobile/)
- [Browse with Copilot — Microsoft Support](https://support.microsoft.com/en-US/microsoft-copilot/browse-with-copilot)
- [Ask Gemini in Chrome to complete tasks with auto browse — Google Help](https://support.google.com/gemini/answer/16821166)
- [Auto browse controls — Google Chrome Help](https://support.google.com/chrome/answer/16821166)
- [Google Chrome unveils Gemini-powered auto-browsing — Mashable via AOL](https://www.aol.com/articles/google-chrome-unveils-gemini-powered-222514349.html)
