---
name: model-period-update
description: Update an existing financial model Excel file with actual figures for a specific period (quarter or fiscal year) without touching any other period. Use when the user says "update the model", "plug actuals", "refresh with latest numbers", "add new quarter to model", or provides a filing alongside a model file. Also creates/updates a "Financial Model Logic" sheet documenting how the model maps to filings. Do NOT use for building new models from scratch or re-forecasting.
---

# Model Period Update

Update one period only. Everything else stays untouched. Every reported subtotal must tie out exactly to its components before the update is considered done.

## Rules

- Edit only the target period column. Keep formatting consistent with adjacent periods.
- Never touch other years, quarters, or forecasts.
- Never overwrite a formula with a hardcode unless the user approves.
- Max 3 web searches, official sources only (SEC EDGAR, company IR page, press release).
- Confirm the data source with the user before any edit.
- Financial model ≠ financial statement. Rows are grouped, renamed, and sign-flipped differently. Map before you write.
- **Identify the anchor number before writing any line items.** When a group of rows rolls up to a subtotal that is *also independently reported* in the filing (e.g., Free Cash Flow, Net Income, Total Revenue, Ending Cash), that filed subtotal is the source of truth — not the sum of whatever components you happen to find. Pull it from the filing on its own, first.
- **Never let a hardcoded subtotal and hardcoded components silently disagree.** If the subtotal cell is a formula, components must foot to the filed subtotal exactly. If the subtotal cell is hardcoded, it overrides — solve the components to match it, not the reverse.
- **Treat "Other / Others / FX & Others / Misc / Adjustments" rows as plugs, not lookups.** Do not search the filing for an "Other" figure and drop it in. Compute it as: `Plug = Reported Anchor Total − Sum(all explicitly-sourced components)`. If the resulting plug is unreasonably large or has an unexpected sign, stop and flag it to the user instead of forcing it in.

## Agent Steps

### Step 1 — Identify target period
Ask or infer which period to update. Locate the exact column in the workbook.

### Step 2 — Check for "Financial Model Logic" sheet
Search sheet names (case-insensitive). If it exists, read it as your mapping guide. If missing, create it in Step 5.

### Step 3 — Get data source
If the user attached/pasted filing data, use it. Otherwise ask, or offer to search official sources. State the exact source and period back to the user. Wait for confirmation before editing.

### Step 4 — Build the mapping (statement → model)
This is the critical step. The model groups line items differently from the filing.

- Read the model's row labels and understand what each represents.
- Read the filing's reported figures.
- Map each model row to its filing source.
- Common differences: revenue lines may come from a disaggregation note, not the P&L face; cost rows may be sign-flipped; a single filing line may split into multiple model rows or vice versa.
- Subtotals, margins, and growth rows that are formulas — skip them, they compute themselves.
- **Flag every plug/residual/catch-all row up front** (names like "Other," "FX & Others," "Miscellaneous," "Adjustments"). These are solved *last*, after every other component feeding the same subtotal is sourced.
- **Check filing terminology equivalence explicitly.** A model label may not exist verbatim in the filing — e.g., model's "Free Cash Flow" may map to the filing's "Increase in cash, cash equivalents, and restricted cash and cash equivalents." Don't assume equivalence silently. State the mapping as an assumption and get user confirmation, since multiple real definitions (operating-cash-flow-based vs. change-in-cash-based FCF, for example) can plausibly fit the same row label.
- If a mapping is ambiguous, ask the user — don't guess.

### Step 5 — Create or update "Financial Model Logic" sheet
If missing, create a new sheet with plain-language notes (15–25 lines) covering:
- Units, fiscal year convention, column layout.
- How each revenue/cost/opex/below-the-line row maps to filing sources.
- Which rows are formulas vs. hardcodes, and which rows are plugs/residuals.
- Sign conventions, any YTD-to-period derivations.
- What is NOT in the model.

If it already exists, update only if the structure changed.

### Step 6 — Write actuals
Enter values into the target column only, one cell at a time, respecting sign conventions and units, in this order:
1. Write the anchor/subtotal figure first, sourced directly from the filing.
2. Write all explicitly-sourced component rows next.
3. Compute the plug/residual row last, as whatever value makes the components foot to the anchor — never as an independently "found" figure.

### Step 7 — Verify (mandatory, not optional)
- **Reconciliation check:** `Sum(component rows) must equal the subtotal cell`, to the decimal shown. If it doesn't tie out, the update is NOT complete — do not report success.
- If a mismatch appears, fix it by adjusting the plug/residual row. Never silently accept a mismatched subtotal, and never quietly alter the anchor number just to make the difference disappear.
- Check that all subtotals recompute correctly (revenue total, gross profit, operating income, net income, free cash flow, ending cash, etc.).
- **Explicitly report the reconciliation** in the summary back to the user, e.g.:
  `Free Cash Flow = 9,383.0 (filed) = Sum of components (9,383.0) ✓`
- Report any derived, estimated, skipped, or plugged cells by name.
- Confirm no other column was modified.
