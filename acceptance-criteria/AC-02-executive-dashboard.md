# AC-02: Firmwide Executive Dashboard

## Feature Overview
Single-screen view of firm health for C-suite: revenue, capacity, pipeline, collections, and forecasting with drill-down from firmwide → office → practice → team → individual.

---

## AC-02.1 — Access Control

**Given** a user with role CLO, CEO, COO, or CFO navigates to Dashboard
**Then** the Firmwide Executive Dashboard is displayed

**Given** a user with role Attorney, Intake, Paralegal, Billing, or Marketing navigates to Dashboard
**Then** the message "You don't have access to this dashboard. Contact your administrator." is displayed

---

## AC-02.2 — Filter Bar

**Given** the dashboard is loaded
**Then** the filter bar shows: Office (multi-select, default: All), Practice Area (multi-select, default: All), Time Period (default: This Month)

**Given** Time Period options are shown
**Then** available presets are: Today, This Week, This Month, This Quarter, YTD, Custom

**Given** filters are changed
**Then** all panels refresh with a subtle fade transition

**Given** a filter combination yields no data
**Then** the message displays: "No data matches the selected filters. [Clear filters]"

**Given** the filter bar is scrolled past
**Then** the filter bar remains sticky at the top of the page

---

## AC-02.3 — Auto-Refresh

**Given** the dashboard is open
**Then** data auto-refreshes every 5 minutes

**Given** the user clicks the manual refresh (⟳) button
**Then** all panels reload immediately

**Given** a panel's data is > 15 minutes stale
**Then** a subtle warning icon appears next to the refresh button

**Given** a data source is unavailable
**Then** the panel shows: "Data unavailable — showing last known values from [timestamp]" in muted styling

---

## AC-02.4 — KPI Summary Cards

**Given** the dashboard loads
**Then** five KPI cards display: Revenue MTD/QTD, New Matters, Average Fee per Case, Average Cycle Time (days), Collections Rate (%)

**Given** a KPI is trending upward compared to prior period
**Then** a green ▲ indicator is shown (accounting for value-awareness: lower cycle time = ▲ green)

**Given** a KPI is trending downward
**Then** a red ▼ indicator is shown

**Given** a KPI is flat
**Then** a gray ► indicator is shown

**Given** a user clicks a KPI card
**Then** a slide-over panel opens with a chart and table breakdown by office and practice area

---

## AC-02.5 — Capacity Heat Map

**Given** the capacity heat map loads
**Then** attorneys are shown with: name, utilization %, and status:
- 🔴 Over (>90% or threshold)
- 🟡 Near (approaching threshold)
- 🟢 Good
- 🔵 Under

**Given** more than 20 attorneys exist
**Then** the heat map defaults to the top 20 by utilization concern (Over > Near > Under), with a "Show all [N] attorneys" expandable link

**Given** open appointment count < demand
**Then** a capacity gap is shown in red: "Gap: -[N] (capacity constrained)"

**Given** demand exceeds capacity
**Then** a yellow banner displays at the top: "⚠ Intake demand exceeds available capacity across [X] practice areas. [Review capacity →]"

**Given** a user clicks an attorney row
**Then** an attorney detail slide-over opens showing matters and capacity breakdown

**Given** a user clicks "Manage capacity →"
**Then** they are navigated to Screen 6 (Capacity Control Panel)

**Given** the user is CLO
**Then** a "What-if" simulation button is visible in the capacity heat map

---

## AC-02.6 — Pipeline by Stage

**Given** the pipeline panel loads
**Then** a horizontal funnel visualization shows matter counts at each stage: Lead → Qualified → Consult Scheduled → Consult Completed → Engaged

**Given** the pipeline is displayed
**Then** overall conversion rate (Lead → Engaged) and stage-over-stage drop-off percentages are shown

**Given** a user clicks a stage
**Then** a slide-over opens listing matters at that stage

**Given** a user clicks "View intake console →"
**Then** they are navigated to Screen 4

---

## AC-02.7 — Forecasting Panel

**Given** the forecasting panel loads
**Then** a line chart shows: actual vs. forecast vs. budget for a 90-day forward period

**Given** the scenario selector is used
**Then** three scenarios are available: Optimistic, Base, Conservative

**Given** the forecast type toggle is used
**Then** four types are available: Revenue, Cash, Collections, Capacity

**Given** the user is CFO
**Then** they can adjust financial forecast assumptions

**Given** the user is CLO
**Then** they can read financial forecasts but only adjust capacity forecast inputs

**Given** the user is CEO or COO
**Then** the forecasting panel is read-only

**Given** an AI confidence interval is calculated
**Then** confidence interval bands are shown on the forecast chart

---

## AC-02.8 — Risk & Compliance Panel

**Given** the risk panel loads
**Then** it shows counts and lists of: missing engagement letters, approaching deadlines, overdue statutory deadlines, low retainers, high AR aging

**Given** a matter has been in "Engaged" status > 48 hours without an engagement letter
**Then** it is escalated to 🔴 in the risk panel

**Given** a user clicks a risk item
**Then** they are navigated to the relevant matter

**Given** a user clicks "View all risks →"
**Then** they are navigated to a filtered risk report

---

## AC-02.9 — Office Comparison Table

**Given** the office comparison table loads
**Then** it shows per office: Office name, Revenue, Matter count, Utilization %, Collections %, Pipeline count

**Given** a user clicks an office row
**Then** they are navigated to Screen 3 (Office Dashboard) filtered to that office

**Given** a user clicks a column header
**Then** the table sorts by that column

**Given** a user clicks "Export CSV"
**Then** a CSV download is triggered

---

## AC-02.10 — AI Anomaly Detection

**Given** a KPI deviates significantly from historical norm
**Then** a subtle blue 🤖 chip appears next to the metric with a tooltip explaining: "Revenue is [X]% above/below typical for this period"

**Given** a user clicks a 🤖 chip
**Then** a tooltip explains the insight

**Given** AI insights are shown
**Then** they can be collapsed/dismissed per session and reset daily

---

## AC-02.11 — Loading & Error States

**Given** the dashboard is loading
**Then** each panel loads independently with a skeleton shimmer; KPI cards show placeholder "—" values; charts show axis labels with shimmer in the chart area

**Given** a panel fails to load
**Then** it shows: "Unable to load [panel name]. [Retry]" without affecting other panels

---

## AC-02.12 — Data Freshness

**Given** each panel loads
**Then** a "Last updated: [timestamp]" label is shown in the panel footer

---

## AC-02.13 — Role-Specific Restrictions

| Feature | CLO | CEO | COO | CFO |
|---------|-----|-----|-----|-----|
| Capacity heat map (attorney-level detail) | Full | Aggregated view by default, can drill in | Full | Visible |
| Forecasting (financial) | Read-only | Read-only | Read-only | Can adjust assumptions |
| Forecasting (capacity inputs) | Can adjust | Read-only | Read-only | Read-only |
| Pricing variance in KPI drill-downs | ✅ | — | — | — |
| Trust balance summary | — | — | — | ✅ |
| Editable preferences | Filter preferences | Filter preferences | Filter preferences | Forecast assumptions + Filter preferences |
