---
name: masterlist-extract
description: "Extract holdings from an uploaded bank/custodian statement (Julius Bär PDF, Goldman Sachs Excel holdings export, or UBS 'Statement of Assets' PDF) into Masterlist rows, and/or merge them with a prior month's Masterlist to produce the new month's file. Use whenever the user uploads a bank statement, portfolio valuation, or holdings export and wants it turned into or added to a Masterlist."
---

# Masterlist extraction

This is a transcription job, not an analysis job. Every figure in the output must be
traceable to a printed line on the source statement. The failure mode that matters is not
"slow" — it is a plausible-looking number that nobody can find in the source. Getting a
number wrong is worse than flagging it as unresolved, because a flagged row gets checked and
a wrong row gets reported to the client.

**Never infer a value that isn't printed, and never classify a line you can't key to the
reference tables** (a portfolio-reference table for columns L–N, and a category-mapping
table for columns D and K — ask the user for these if the mapping isn't obvious from what
they've already given you). Anything uncertain goes to an exceptions list, presented
alongside the output, not buried or omitted.

The uploaded file's bank isn't given by name — identify it from its own layout (JB: clean
PDF tables with a "Report Details" page; GS: Excel with a Description/Symbol/Market
Value/ISIN header row; UBS: long multi-column PDF "Statement of Assets").

## Output columns (exact)

| Col | Header (exact) | Content |
|---|---|---|
| B | `Month` | Reporting date — last calendar day of the month — as a real Excel date, not text |
| C | `Account/portfolio No` | Portfolio identifier as printed, prefixed `Portfolio ` (UBS rows use the `UBS Portfolio N - ...` form) |
| D | `Account Class` | The statement's section/category, mapped per bank |
| E | `` (a single space) | Bank code: `JB`, `GS`, `UBS`, or the carried-forward bank's code |
| F | `Identifier` | ISIN → CUSIP/security no. → IBAN → contract/serial no. → blank |
| G | `Name of instrument` | Cleaned single-line instrument description |
| H | `Currency` | Position currency |
| I | `Position` | Quantity/nominal as stated |
| J | `Value in USD` | Market value as stated |
| K | `Asset Class ` | Internal asset-class label (trailing space in header) |
| L | `Account Category` | From the portfolio reference table |
| M | `Asset classification ` | From the portfolio reference table (trailing space in header) |
| N | `Type` | From the portfolio reference table |

## Verify before shipping

- Every portfolio reconciles to its statement total, or the variance is explained.
- Header row is row 2; data starts row 3; column A empty; exactly columns B–N populated.
- Header strings match byte-for-byte, trailing spaces intact.
- No subtotal or total rows present.
- Loans/short options negative in both numeric columns; Loans rows blank in L, M, N.
- Carried-forward rows identical to the prior file, and no stale JB/GS/UBS rows remain.
- Exceptions list presented, not empty-by-omission.
