Update an existing financial model Excel file with actual figures for a specific period (quarter or fiscal year) without touching any other period. Use when the user says "update the model", "plug actuals", "refresh with latest numbers", "add new quarter to model", or provides a filing alongside a model file. Also creates a "Financial Model Logic" sheet documenting how the model maps to filings. Do NOT use for building new models from scratch or re-forecasting.

Model Period Update

Update one period only. Everything else stays untouched.

Rules
Edit only the target period column and keep the formatting same as before or consistent. Never touch other years, quarters, or forecasts.
Never overwrite a formula with a hardcode unless user approves.
Max 3 web searches, official sources only (e.g. SEC EDGAR, company IR page).
Confirm data source with user before any edit.
Financial model ≠ financial statement. Rows are grouped, renamed, sign-flipped differently. Map before you write.
Agent Steps
Step 1 — Identify target period

Ask or infer which period to update. Locate the exact column in the workbook.

Step 2 — Check for "Financial Model Logic" sheet

Search sheet names (case-insensitive). If it exists, read it as your mapping guide. If missing, you must create it in Step 5.

Step 3 — Get data source
If user attached/pasted filing data → use it.
If not → ask user. Offer to search for the official filing (SEC EDGAR, company IR, etc.).
State the exact source and period back to the user. Wait for confirmation before editing.
Step 4 — Build the mapping (statement → model)

This is the critical step. The model groups line items differently from the filing.

Read the model's row labels and understand what each represents.
Read the filing's reported figures.
Map each model row to its filing source. Common differences:
Revenue lines may come from a disaggregation note, not the P&L face.
Cost rows may be sign-flipped (entered as negatives).
A single filing line (e.g. "Other income, net") may be split into multiple model rows or vice versa.
Subtotals, margins, and growth rows are usually formulas — skip them.
If a mapping is ambiguous, ask the user — don't guess.
Step 5 — Create or update "Financial Model Logic" sheet

If missing, create a new sheet with plain-language notes (15–25 lines) explaining:

Units, fiscal year convention, column layout.
How each revenue/cost/opex/below-the-line row maps to filing sources.
Which rows are formulas vs hardcodes.
Sign conventions, any YTD-to-period derivations.
What is NOT in the model.

If it already exists, update only if the structure changed.

Step 6 — Write actuals

Enter values into the target column only, one cell at a time. Respect sign conventions and units.

Step 7 — Verify
Check that subtotals recompute correctly (revenue total, gross profit, operating income, net income, etc.).
Report any derived, estimated, or skipped cells.
Confirm no other column was modified.
