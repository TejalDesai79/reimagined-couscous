# AC-10: Accounting & Forecasting Dashboard

## Feature Overview
Financial command center for CFO and finance team: cash and accrual reporting, budget vs. actual analysis, and multi-scenario forecasting (revenue, cash, collections, capacity).

---

## AC-10.1 — Access Control

| Feature | CFO | CLO | CEO | COO |
|---------|-----|-----|-----|-----|
| P&L / Balance Sheet / Cash Flow | Full R/W | Read-only (revenue lines) | Read-only | Read-only |
| Budget vs. Actual | Full R/W | Read-only | Read-only | Read-only |
| Forecasting (Financial) | Full R/W (adjust assumptions) | Read-only | Read-only | Read-only |
| Forecasting (Capacity) | Read-only | R/W (adjust inputs) | Read-only | Read-only |
| Variance threshold config | Full R/W | — | — | — |
| Export | ✅ | ✅ | ✅ | ✅ |

**Given** a user with role Attorney, Intake, Paralegal, Billing, or Marketing
**Then** they have no access to this screen

---

## AC-10.2 — Header

**Given** the Accounting Dashboard loads
**Then** the header shows: basis toggle (Accrual/Cash), period selector (default: current month), and a refresh button

**Given** available tabs
**Then** they are: P&L, Balance Sheet, Cash Flow, Budget vs. Actual, Forecasting

---

## AC-10.3 — P&L Tab (Default)

**Given** the P&L tab is active
**Then** the statement shows: Revenue, Expenses, and Net Income for MTD, QTD, and YTD columns

**Given** the basis toggle is set to Accrual
**Then** accrual-basis figures are shown

**Given** the basis toggle is set to Cash
**Then** cash-basis figures are shown

**Given** a drill-down action is used
**Then** available drill-downs are: by office, by practice area, by attorney

**Given** trust balances are referenced
**Then** the label clearly states: "Trust balances are reported on Screen 11" — they are excluded from P&L/operating accounts

---

## AC-10.4 — Balance Sheet Tab

**Given** the Balance Sheet tab is active
**Then** standard balance sheet line items are shown for the selected period

---

## AC-10.5 — Cash Flow Tab

**Given** the Cash Flow tab is active
**Then** cash flow data is shown for the selected period

**Given** a significant timing difference exists between cash and accrual views
**Then** a banner displays: "Note: Significant timing differences between cash and accrual views. [View reconciliation]"

---

## AC-10.6 — Budget vs. Actual Tab

**Given** the Budget vs. Actual tab is active
**Then** each line shows: Actual amount, Budget amount, Variance ($), and Variance (%)

**Given** a line item has a negative variance > 10%
**Then** it shows a 🔴 variance flag

**Given** a line item has a negative variance between 5–10%
**Then** it shows a 🟡 variance flag

**Given** the CFO configures variance thresholds
**Then** the thresholds are saved and applied to future variance flag evaluations

**Given** the budget is not configured for a period
**Then** the message displays: "Budget not configured for this period. [Set budget →]" — actuals still display

---

## AC-10.7 — Forecasting Tab

**Given** the Forecasting tab is active
**Then** the following controls are shown: Forecast Model selector, Horizon selector, Scenario selector, and comparison toggles (Budget, other scenarios)

**Given** forecast models are available
**Then** they are: Revenue, Cash, Collections, Capacity

**Given** forecast horizon options are available
**Then** they include: 90 days (and others as configured)

**Given** scenarios are available
**Then** they are: Optimistic, Base, Conservative

**Given** the forecast chart renders
**Then** it shows overlaid lines for: actual (historical), base forecast, optimistic scenario, conservative scenario, and budget

**Given** a user hovers over a data point
**Then** the specific value and date are shown in a tooltip

**Given** forecast drivers are shown
**Then** they include: pipeline qualified lead count with conversion estimate, capacity utilization and headroom, collections rate (trailing 90-day), and average fee (trailing 90-day)

**Given** the CFO adjusts financial forecast assumptions
**Then** the chart updates with an animation and a toast shows: "Forecast updated"

**Given** the CLO adjusts capacity forecast inputs
**Then** the capacity forecast updates (financial forecast remains unchanged)

**Given** the base forecast diverges > 20% from budget
**Then** a prominent alert displays: "Forecast significantly deviates from budget. Review assumptions."

**Given** a user saves a scenario
**Then** it is stored with a name for future reference

---

## AC-10.8 — AI Assists

**Given** the dashboard is loaded
**Then** AI anomaly detection flags unusual variances with an explanation chip

**Given** a multi-month declining trend is detected
**Then** AI notes the pattern: "Elder Law revenue declining 3 consecutive months"

**Given** forecast confidence bands are calculated
**Then** they are shown as shaded areas on the forecast chart

**Given** AI insights are shown
**Then** they are advisory only — no automated financial actions are triggered

---

## AC-10.9 — Export

**Given** a user clicks "Export PDF"
**Then** a PDF of the current financial statement is generated and downloaded

**Given** a user clicks "Export XLS"
**Then** an Excel file of the current financial statement is generated and downloaded

**Given** an export is completed
**Then** a toast displays: "Report exported"

---

## AC-10.10 — Loading & Error States

**Given** the dashboard is loading
**Then** statement tables show skeleton rows; charts show axis labels with shimmer

**Given** data fails to load
**Then** the message displays: "Unable to load financial data. [Retry] [Contact finance admin]"

**Given** no financial data exists yet
**Then** the message displays: "Financial data will populate as matters generate billing activity."
