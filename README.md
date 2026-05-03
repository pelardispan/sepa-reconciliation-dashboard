# SEPA Transaction Reconciliation Dashboard

A working Excel-based reconciliation model that demonstrates the day-to-day
workflow of a Payment Operations / Finance Operations analyst at an acquirer,
PSP or fintech back-office.

Built as a portfolio piece while studying the
[Wharton Fintech Specialization](https://www.coursera.org/specializations/wharton-fintech)
and self-directed payments coursework (PSD2, KYC/AML, SEPA, SCA/3DS2).

---

## What it does

Matches 100 mock SEPA transactions from an internal system against a bank
statement that contains realistic discrepancies — missing entries, amount
mismatches, settlement lag, reference variations, and phantom bank entries —
then surfaces them through a live operational dashboard.

This is the same workflow used at acquirers (Adyen, Stripe), PSPs (Mollie,
Worldline) and fintech back-offices when matching merchant transaction logs
against settlement files received from card schemes or banking partners.
Discrepancies that aren't caught here become disputed chargebacks, lost
revenue, or compliance issues downstream.

---

## Features

### 1. Multi-tier match logic (6 outcomes, not just true/false)

| Result | Meaning |
|---|---|
| ✅ Matched | Found in bank, amount matches, within window, reference matches |
| ❌ Missing in Bank | Internal record exists, no bank record found |
| ⚠ Amount Mismatch | Found in bank, but amount differs beyond tolerance |
| ⏰ Late Settlement | Found in bank, but outside settlement window |
| ⚠ Reference Mismatch | Found in bank, amount OK, but reference doesn't match |
| 🚨 Phantom | Bank record with no internal counterpart (potential fraud) |

### 2. Settlement-window matching

Bank settlements typically lag 1–3 business days behind the transaction date
(T+1 / T+2 / T+3 cycles). Hard-matching on dates breaks instantly. The model
matches transactions if they arrive at the bank within a configurable window
(default: 3 days). Lag distribution is shown on the dashboard.

### 3. Fuzzy reference matching

Bank references are rarely identical to internal references. `"Bol.com Order"`
might appear as `"BOL.COM ORDER"`, `"PMT Bol.com Order"`, or
`"Bol.com Order - REF1234"`. The model uses three matching strategies:

- **Exact** — case-insensitive equality
- **Fuzzy (first-word)** — first word of internal reference appears in bank reference
- **Fuzzy (no-spaces)** — references match when whitespace is stripped

### 4. Threshold-based health alerts

The dashboard shows a status banner that automatically goes
🟢 **HEALTHY** / 🟡 **WARNING** / 🔴 **CRITICAL** based on the live match rate
versus configurable thresholds. Defaults: ≥95% green, 85–95% amber, <85% red.

### 5. Exception queue

A pre-filtered, severity-sorted view of transactions needing manual
investigation — exactly what an ops analyst opens first thing each morning.

### 6. Configuration-driven design

All thresholds (settlement window, amount tolerance, match-rate targets,
fuzzy-match toggle) live in a single `Settings` sheet. Change a value, the
entire workbook recalculates. No hardcoded magic numbers in formulas.

---

## Workbook structure

```
README                  → this overview, in-workbook
Settings                → configurable thresholds
Internal Transactions   → 100 mock SEPA payments
Bank Statement          → bank-side records with deliberate discrepancies
Reconciliation          → 12-column match logic (heart of the model)
Dashboard               → KPIs, alerts, charts, breakdowns
Exception Queue         → severity-sorted manual-review list
```

---

## Tech notes

- **Pure Excel** — no macros, no VBA, no Python, no external data
- **2,036 formulas**, zero `#REF!` / `#DIV/0!` / `#VALUE!` errors
- **Fully dynamic** — modify any source value or setting and outputs
  recalculate automatically
- **Conditional formatting** drives the visual exception management
- Built with `openpyxl` (generation script not included in this repo —
  the deliverable is the spreadsheet itself)

---

## How to read this in 60 seconds

1. Open `Dashboard` → check the alert banner at the top (green/amber/red)
2. Open `Exception Queue` → see what needs investigation today, sorted
   by severity
3. Open `Reconciliation` → drill into any specific transaction's match
   logic, column by column
4. Open `Settings` → change `Settlement window` to `1` and reopen the
   dashboard to see how exceptions spike

---

## Skills demonstrated

- SEPA / IBAN / payments data structures
- Multi-step reconciliation logic (presence, amount, date-window, fuzzy match)
- Excel formulas: VLOOKUP, INDEX/MATCH, IFERROR, COUNTIF, SUMIF, AND/OR,
  ABS, EXACT, SEARCH, SUBSTITUTE, nested IF
- Conditional formatting and threshold-based status indicators
- Pivot-style breakdowns and chart-based reporting
- Dashboard design: KPI cards, status banners, exception queues
- Configuration-driven design (separation of logic from parameters)

---

## About me

Economics graduate based in Amsterdam, building toward a career in payment
operations and fintech ops. Currently completing the Wharton Fintech
Specialization (University of Pennsylvania, Coursera).

[LinkedIn](https://www.linkedin.com/in/pelardispan/)

---

*Built October 2025. Mock data only — no real customer or bank information.*
