# Screen 2: Firmwide Executive Dashboard (CLO/CEO/C-Suite)

## Purpose
Provide executives with a single-screen view of firm health: revenue performance, capacity utilization, pipeline status, collections, and forecasting. Supports drill-down from firmwide → office → practice → team → individual. This is the CLO's primary operational command center.

## Primary Users
CLO (primary), Managing Partner/CEO, COO, CFO

## When This Screen Is Used (Workflow Stage)
Daily operational review, weekly leadership meetings, strategic planning sessions. Accessed from left nav "Dashboard" or as the default home for C-suite roles.

---

## LAYOUT (TEXTUAL WIREFRAME)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ [Global Nav Shell — see Screen 1]                                       │
├─────────────────────────────────────────────────────────────────────────┤
│ DASHBOARD HEADER                                                        │
│ Firmwide Dashboard          [Office ▾] [Practice ▾] [Period ▾]  [⟳]   │
│                              [All]      [All]        [This Month]       │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐          │
│ │ KPI CARD│ │ KPI CARD│ │ KPI CARD│ │ KPI CARD│ │ KPI CARD│          │
│ │ Revenue │ │ New     │ │ Avg Fee │ │ Cycle   │ │ Collect.│          │
│ │ MTD     │ │ Matters │ │ per Case│ │ Time    │ │ Rate    │          │
│ │ $1.2M   │ │ 47      │ │ $4,800  │ │ 38 days │ │ 91%     │          │
│ │ ▲ 8% MoM│ │ ▼ 3%    │ │ ▲ 2%   │ │ ▼ 5%   │ │ ► 0%    │          │
│ └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘          │
│                                                                         │
│ ┌──────────────────────────────┐ ┌──────────────────────────────────┐  │
│ │ CAPACITY HEAT MAP            │ │ PIPELINE BY STAGE                │  │
│ │                              │ │                                  │  │
│ │ Attorney   Util%  Status    │ │  Lead  Qual  Sched  Comp  Eng   │  │
│ │ ──────────────────────────  │ │  ████  ███   ████   ██    ████  │  │
│ │ J. Smith   92%   🔴 Over   │ │  124   87    62     41    38    │  │
│ │ K. Park    78%   🟢 Good   │ │                                  │  │
│ │ L. Chen    45%   🔵 Under  │ │  Conversion funnel visual        │  │
│ │ M. Jones   88%   🟡 Near   │ │  Lead→Engaged: 31%              │  │
│ │ ...                         │ │  [View intake console →]         │  │
│ │                              │ │                                  │  │
│ │ 🔲 Open Appts: 14 this wk  │ │  Stage breakdown by practice ▾  │  │
│ │ 🔲 Demand: 22 consults req  │ │                                  │  │
│ │ Gap: -8 (capacity constrain)│ │                                  │  │
│ │                              │ │                                  │  │
│ │ [Manage capacity →]          │ │                                  │  │
│ └──────────────────────────────┘ └──────────────────────────────────┘  │
│                                                                         │
│ ┌──────────────────────────────┐ ┌──────────────────────────────────┐  │
│ │ FORECASTING PANEL            │ │ RISK & COMPLIANCE                │  │
│ │                              │ │                                  │  │
│ │ Revenue Forecast (90-day)    │ │ ⚠ 3 matters: missing eng. letter│  │
│ │ ┌────────────────────────┐  │ │ ⚠ 7 matters: deadline <7 days  │  │
│ │ │ [Line chart: actual vs │  │ │ 🔴 2 matters: statutory deadline│  │
│ │ │  forecast vs budget]   │  │ │    overdue                      │  │
│ │ └────────────────────────┘  │ │ ⚠ 12 retainers below threshold │  │
│ │                              │ │ ⚠ AR >90 days: $184K           │  │
│ │ Cash Forecast | Collections  │ │                                  │  │
│ │ Capacity Forecast            │ │ [View all risks →]              │  │
│ │ [Toggle: Optimistic/Base/    │ │                                  │  │
│ │  Conservative]               │ │                                  │  │
│ └──────────────────────────────┘ └──────────────────────────────────┘  │
│                                                                         │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ OFFICE COMPARISON TABLE                                           │  │
│ │ Office    | Revenue | Matters | Util% | Collect% | Pipeline     │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ Main St   | $480K   | 89      | 82%   | 93%      | 42 leads    │  │
│ │ Downtown  | $390K   | 67      | 76%   | 89%      | 31 leads    │  │
│ │ Suburban  | $330K   | 54      | 71%   | 91%      | 28 leads    │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ [Click row → Office Dashboard]                    [Export CSV]   │  │
│ └───────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

## Header
- Dashboard title with breadcrumb (Firmwide Dashboard)
- Filter bar: Office (multi-select), Practice Area (multi-select), Time Period (preset: Today, This Week, This Month, This Quarter, YTD, Custom)
- Refresh button (manual refresh; data auto-refreshes per interval — **[ASSUMPTION]** every 5 minutes)

## Primary Navigation
Global left nav (Screen 1 shell). Dashboard is the active/highlighted item.

## Secondary Navigation
None — filters serve as the dimensional navigation model.

## Main Content Areas
1. **KPI Summary Row** (top, full width, 5 cards)
2. **Capacity Heat Map** (left, 50%)
3. **Pipeline by Stage** (right, 50%)
4. **Forecasting Panel** (left, 50%)
5. **Risk & Compliance** (right, 50%)
6. **Office Comparison Table** (full width, bottom)

## Persistent Elements
- Filter bar is sticky when scrolling
- Alert count in global nav updates in real-time

---

## KEY COMPONENTS

### 1. KPI Summary Cards
- **Description:** Five key metric cards with current value, trend indicator, and period-over-period comparison
- **Data displayed:**
  - Revenue MTD/QTD (per selected period)
  - New Matters (count)
  - Average Fee per Case
  - Average Cycle Time (days, lead-to-close)
  - Collections Rate (%)
- **Actions available:** Click card → drill-down detail view (slide-over panel with chart + table breakdown by office/practice)
- **Trend indicators:** ▲ green (improvement), ▼ red (decline), ► gray (flat). Direction is value-aware (e.g., lower cycle time = ▲ green)

### 2. Capacity Heat Map
- **Description:** Visual representation of attorney utilization with capacity gap analysis
- **Data displayed:**
  - Attorney name, utilization %, status (Over 🔴, Near 🟡, Good 🟢, Under 🔵)
  - Open appointments this week vs. demand (consult requests)
  - Capacity gap indicator
- **Actions available:**
  - Click attorney row → Attorney detail (matters, capacity breakdown)
  - "Manage capacity →" → Screen 6 (Attorney Capacity Control Panel)
  - Sort by: utilization %, office, practice area
  - Filter to specific office or practice
- **CLO-specific:** Includes "What-if" button to simulate capacity changes (links to Screen 6)

### 3. Pipeline by Stage
- **Description:** Horizontal funnel visualization of matters across the universal stage model
- **Data displayed:**
  - Count of matters at each stage (Lead → Qualified → Consult Scheduled → Consult Completed → Engaged)
  - Overall conversion rate (Lead → Engaged)
  - Stage-over-stage drop-off percentages
- **Actions available:**
  - Click stage → list of matters at that stage (slide-over)
  - Toggle: by practice area
  - Link to Intake Console

### 4. Forecasting Panel
- **Description:** Multi-scenario forecast charts (revenue, cash, collections, capacity)
- **Data displayed:**
  - Line chart: actual vs. forecast vs. budget (90-day forward)
  - Toggle between: Revenue, Cash, Collections, Capacity forecasts
  - Scenario selector: Optimistic / Base / Conservative
- **Actions available:**
  - Hover data points for details
  - Toggle scenario/metric
  - Export chart as PNG/PDF
  - **[ASSUMPTION]** CFO can adjust forecast assumptions; CLO/CEO read-only on financial forecasts but CLO can adjust capacity forecast inputs

### 5. Risk & Compliance Panel
- **Description:** Aggregated risk indicators across all matters
- **Data displayed:**
  - Count and list of: missing engagement letters, approaching deadlines, overdue statutory deadlines, low retainers, high AR aging
  - Each item is a clickable link to the relevant matter
- **Actions available:** Click item → navigate to matter; "View all risks →" → filtered risk report

### 6. Office Comparison Table
- **Description:** Tabular comparison of all offices on key metrics
- **Data displayed:** Office name, Revenue, Matter count, Utilization %, Collections %, Pipeline count
- **Actions available:** Click row → Office Dashboard (Screen 3); Sort by any column; Export CSV

---

## INTERACTIONS & STATES

### Default State
All panels populated with data for the selected filters. Default: All offices, All practices, This Month.

### Empty State
- **New firm / no data:** "Welcome to your dashboard. Data will appear as matters and activities are created." Illustration with setup checklist.
- **Filtered to empty result:** "No data matches the selected filters. [Clear filters]"

### Loading State
- Each panel loads independently with skeleton shimmer
- KPI cards show placeholder "—" values with shimmer
- Charts show axis labels with shimmer in chart area

### Error State
- Per-panel error: "Unable to load [panel name]. [Retry]" — other panels unaffected
- Full dashboard error: Centered error with support link

### Blocked/Gated State
- Not applicable — this is a read-only analytics screen
- If a user without dashboard permission navigates here: "You don't have access to this dashboard. Contact your administrator."

### Success/Confirmation State
- Filter change: panels refresh with subtle fade transition
- Export: toast notification "Report exported successfully"

---

## AUTOMATIONS & AI ASSISTS

### What the system suggests
- **Anomaly detection:** AI highlights KPIs that deviate significantly from historical norm with a subtle glow and tooltip: "Revenue is 15% above typical for this period"
- **Capacity recommendation:** When demand exceeds capacity, system suggests: "Consider opening 3 additional appointment slots this week" (surfaced in capacity heat map)
- **Forecast confidence:** AI displays confidence interval bands on forecast charts

### What requires human approval
- All actions from this screen are navigation/viewing only — no automated actions triggered
- Capacity adjustments suggested here require CLO approval on Screen 6

### How suggestions are surfaced visually
- AI insights appear as a subtle blue "insight" chip (🤖) next to the relevant metric
- Tooltip on hover explains the insight
- **[ASSUMPTION]** AI insights can be collapsed/dismissed per session; they reset daily

---

## ROLE-BASED VISIBILITY

### CLO
- **Full access** to all panels
- Capacity heat map includes "What-if" simulation button
- Can see attorney-level detail in all drill-downs
- Sees pricing variance and margin data in KPI drill-downs
- **Editable:** Capacity forecast inputs, dashboard filter preferences (saved)

### CEO / Managing Partner
- **Full read access** to all panels
- Capacity heat map shows aggregated view (no individual attorney names by default; can drill in)
- Forecasting panel: read-only
- **Editable:** Dashboard filter preferences only

### COO
- **Full read access** to all panels
- Emphasis on: pipeline, SLA metrics, staffing, cycle time
- Capacity heat map: full detail
- **Editable:** Dashboard filter preferences only

### CFO
- **Full read access** to financial panels (Revenue, Collections, Forecasting, AR)
- Capacity heat map: visible but secondary
- Pipeline: visible but secondary
- Forecasting panel: **can adjust financial forecast assumptions**
- Trust balance summary visible
- **Editable:** Forecast assumptions, dashboard filter preferences

### Attorney
- **No access** to this screen. Attorneys see their personal dashboard on the Home screen.

### Intake Specialist
- **No access** to Firmwide Dashboard. Intake sees Office Dashboard (Screen 3).

### Paralegal / Billing / Marketing
- **No access** to this screen.

### Client
- **No access.** Clients use the Client Portal.

---

## EDGE CASES & GUARDRAILS

### Capacity exceeded
- Capacity heat map: over-utilized attorneys highlighted in red row with pulsing indicator
- Capacity gap section shows negative gap in red: "Gap: -8 (capacity constrained)"
- If gap is negative: system displays yellow banner at top of dashboard: "⚠ Intake demand exceeds available capacity across [X] practice areas. [Review capacity →]"

### Payment fails
- Not directly shown on this screen. AR aging in Risk panel reflects downstream payment issues.

### Required documents missing
- Risk & Compliance panel: "⚠ [N] matters: missing engagement letter" — each clickable to the matter
- If any matter has been in "engaged" status >48 hours without an engagement letter: escalated to 🔴

### Deadlines at risk
- Risk & Compliance panel: grouped by severity
  - 🔴 Statutory/filing deadlines overdue or <24 hours
  - ⚠ Internal deadlines <7 days
- Click through to matter workspace for resolution

### Data freshness
- **[ASSUMPTION]** Each panel shows a "Last updated: [timestamp]" in the footer
- If data is >15 minutes stale: subtle warning icon next to refresh button
- If data source is down: panel shows "Data unavailable — showing last known values from [timestamp]" with muted styling

### Large firm (many offices/attorneys)
- **[ASSUMPTION]** Capacity heat map defaults to top 20 attorneys by utilization concern (over > near > under)
- "Show all [N] attorneys" expandable link
- Office comparison table paginated at 10 rows (unlikely to exceed in v1 per 10-office assumption)
