---
name: model-period-update
description: Update an existing financial model workbook with actuals for one specific period (a fiscal year, a quarter, or a year the user names) without disturbing any other period. Use this whenever the user asks to "update the model with Q3", "plug the latest 10-Q/10-K", "refresh FY26 actuals", "add the new quarter to my model", or hands over a filing and an Excel model together. Also use it when the model's structure needs to be documented before editing. Do NOT use for building a new model, re-forecasting, or restating history.
---

# Model Period Update

Update **only** the period the user names. Every other column stays byte-identical.

## 0. Non-negotiables

- Edit only the target period's cells. Never touch prior years, other quarters, or forecast columns unless the user explicitly says to.
- Never overwrite a formula with a hardcoded number unless the user approves that cell.
- A financial model is **not** a financial statement. Rows are clubbed, renamed, sign-flipped, and re-grouped. Map before you write (§3).
- Confirm the data source with the user before any edit (§2).
- Web search: **maximum 3 searches**, only to locate an official filing. Never source numbers from news articles, aggregators, or summary sites.

## 1. Orient in the workbook

1. List sheets. Identify the model sheet and read its header rows to learn the column layout — historical FYs, quarter blocks per year, forecast (`P`) columns, and any right-hand driver/assumption columns (`% of revenue`, margin ratios).
2. Locate the target period column by matching the header exactly (e.g. `Jun` under `2026 Quarter Ending,`, or `2025` in the FY block). If two columns could match, ask.
3. Check whether a sheet named **`Financial Model Logic`** exists. Search sheet names case-insensitively and ignoring spaces. If missing, create it (§5). If present, read it — it is your mapping guide — and update it only if the structure changed.

## 2. Get and confirm the data

- If the user attached or pasted the filing → use it. Say which document and period you read.
- If not → ask for it. Offer to look for the official filing, and if they agree, search for the SEC EDGAR filing index / company IR page only. Then **state the exact source and period back to the user and wait for confirmation** before editing:

  > "I'll use Apple's Form 10-Q for the quarter ended June 27, 2026 (SEC EDGAR). Confirm and I'll update only the Jun-26 column."

- Never edit on an unconfirmed source. Never mix sources within one period.

## 3. Map statement → model

Build the mapping first, then write. Typical differences to expect:

| Model row | Where it comes from |
|---|---|
| Revenue lines (iPhone, Mac, Services…) | Revenue disaggregation note, not the P&L face |
| `Products` / `Services` cost rows | Cost of sales split, entered **negative** |
| `Selling & General Expenses` | SG&A as reported (model may or may not include marketing separately) |
| `Interest Expense`, `Interest & Dividend Income` | Often broken out of a single reported `Other income/(expense), net` — if the filing gives only the net line, put the net in the model's Other Income row and leave the split rows alone |
| `EBT`, `EBITDA`, margins, `% growth` | Almost always formulas — do not type over them |
| Cash flow lines | Quarterly models need **period-only** figures; filings give year-to-date. Derive: `Q3 = 9M − 6M`. Say when you have derived rather than read a number. |

Rules:
- Match on economic meaning, not label similarity.
- Respect the sheet's sign convention (costs and outflows negative in the example above).
- Respect units and scaling shown in the header (e.g. USD in millions).
- If a filing line has no home in the model, or one model row needs two filing lines combined, **ask rather than guess**.

## 4. Write, then check

1. Enter values into the target column only, one line item at a time.
2. Verify: revenue lines sum to total revenue; gross profit, operating income, EBT, net income recompute to the filing; the EPS/share-count rows (if present) tie.
3. Report every derived, estimated, or skipped cell, and any subtotal that no longer ties.
4. Confirm in one line that no other column was modified.

## 5. `Financial Model Logic` sheet

Create it if absent (or refresh it if the structure changed). Plain language, short — a new analyst should be able to update the next period from it alone. Keep it to a single column of text, roughly 15–25 lines:

```
FINANCIAL MODEL — HOW IT'S BUILT

Units: USD millions. FY ends last Saturday of September.
Columns: FY18–FY24 actuals | FY25 quarters | FY26 quarters | FY26P–FY28P forecast | driver ratios at right.

Revenue: 5 product/service lines from the revenue note; each has a % growth row below it.
Costs: Products and Services cost of sales, entered as negatives; not split by segment.
Opex: R&D and SG&A only. Marketing sits inside SG&A.
Below the line: Interest expense and interest income are broken out separately from
  reported "Other income, net"; if the filing gives only the net figure it goes in Other Income.
Subtotals (Gross Profit, Operating Income, EBT, Net Income, EBITDA, margins, growth) are
  formulas — never hardcode.
Cash flow: quarterly columns are period-only, derived from YTD filing figures by subtraction.
Forecast columns are driven by the ratio cells on the right (% of revenue), not typed in.
Not in this model: balance sheet, segment P&L, share count roll-forward.

Update rule: one period column at a time; leave all other columns untouched.
```

Adapt every line to the workbook actually in front of you — the block above is a shape, not a template to copy.
