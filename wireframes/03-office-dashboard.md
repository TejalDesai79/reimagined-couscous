# Screen 3: Office Dashboard (Intake Lead / Ops)

## Purpose
Provide office-level operational visibility into call volume, intake pipeline, staffing load, and SLA performance. This is the daily command center for office managers and intake leads to ensure smooth operations and identify bottlenecks before they impact client experience.

## Primary Users
Intake Specialist (Intake Lead), COO, CLO (drill-down from Firmwide Dashboard)

## When This Screen Is Used (Workflow Stage)
Continuous throughout the workday. Primary screen for intake leads. COO and CLO access via drill-down from Firmwide Dashboard or direct navigation.

---

## LAYOUT (TEXTUAL WIREFRAME)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ [Global Nav Shell — see Screen 1]                                       │
├─────────────────────────────────────────────────────────────────────────┤
│ DASHBOARD HEADER                                                        │
│ Office Dashboard: [Main Street ▾]          [Today ▾]          [⟳]      │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐     │
│ │CALLS TODAY│ │QUEUE NOW │ │LEADS     │ │CONSULTS  │ │INTAKE SLA│     │
│ │ 34       │ │ 3 waiting│ │ 12 new   │ │ 8 sched  │ │ 94%      │     │
│ │ Ans: 89% │ │ Avg: 42s │ │ 7 qual'd │ │ 3 today  │ │ Target:95│     │
│ └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘     │
│                                                                         │
│ ┌──────────────────────────────────┐ ┌──────────────────────────────┐  │
│ │ CALL QUEUE & SLA METRICS         │ │ INTAKE PIPELINE              │  │
│ │                                  │ │                              │  │
│ │ IVR Branch   | Queue | Avg Wait │ │ Stage       | Count | Trend  │  │
│ │ ─────────────────────────────── │ │ ────────────────────────────│  │
│ │ 1-New Client | 2     | 0:38    │ │ New Leads   | 12    | ▲     │  │
│ │ 2-Existing   | 1     | 0:22    │ │ Qualified   | 7     | ►     │  │
│ │ 3-Billing    | 0     | —       │ │ Scheduled   | 8     | ▲     │  │
│ │                                  │ │ Fee Pending | 3     | ▼     │  │
│ │ 🟢 SLA: 80% <30s (target 80%)  │ │ Consult Done| 5     | ►     │  │
│ │ 📞 Abandoned: 4% (target <5%)  │ │ Engaged     | 4     | ▲     │  │
│ │                                  │ │                              │  │
│ │ [Live call monitor →]           │ │ [Open intake console →]      │  │
│ └──────────────────────────────────┘ └──────────────────────────────┘  │
│                                                                         │
│ ┌──────────────────────────────────┐ ┌──────────────────────────────┐  │
│ │ STAFFING & CAPACITY SNAPSHOT     │ │ BOTTLENECK WARNINGS          │  │
│ │                                  │ │                              │  │
│ │ Role         | In | Avail | OOO │ │ 🟡 J. Smith: 0 open appts  │  │
│ │ ─────────────────────────────── │ │    this week (92% utilized) │  │
│ │ Attorneys    | 4  | 3     | 1   │ │ 🟡 3 consult fees unpaid   │  │
│ │ Paralegals   | 3  | 2     | 0   │ │    >24hrs — appts not      │  │
│ │ Intake Staff | 2  | 2     | 0   │ │    confirmed                │  │
│ │                                  │ │ 🔴 Estate admin queue: 6   │  │
│ │ Atty Capacity This Week:        │ │    matters waiting assign.  │  │
│ │ Open Appts: 6 | Demand: 11     │ │                              │  │
│ │ ⚠ Gap: -5                       │ │ [View all →]                │  │
│ │ [Request capacity expansion →]  │ │                              │  │
│ └──────────────────────────────────┘ └──────────────────────────────┘  │
│                                                                         │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ TODAY'S SCHEDULE                                                   │  │
│ │ Time  | Attorney    | Client        | Type     | Fee   | Status  │  │
│ │ ────────────────────────────────────────────────────────────────  │  │
│ │ 9:00  | K. Park     | New: Garcia   | Estate P | $250  | ✅ Paid │  │
│ │ 10:00 | K. Park     | New: Thompson | Elder L  | $250  | ⏳ Pend │  │
│ │ 10:30 | L. Chen     | Ret: Williams | Tax      | Waived| ✅ N/A  │  │
│ │ 1:00  | J. Smith    | New: Davis    | Estate A | $250  | 🔴 Fail│  │
│ │ 2:30  | L. Chen     | New: Roberts  | Estate P | $250  | ✅ Paid │  │
│ │ ────────────────────────────────────────────────────────────────  │  │
│ │ [View full calendar →]                                            │  │
│ └───────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

## Header
- Dashboard title with office selector dropdown
- Date/period selector (default: Today; options: Today, This Week, This Month)
- Refresh button

## Main Content Areas
1. **KPI Summary Row** (5 cards, full width)
2. **Call Queue & SLA Metrics** (left, 50%)
3. **Intake Pipeline** (right, 50%)
4. **Staffing & Capacity Snapshot** (left, 50%)
5. **Bottleneck Warnings** (right, 50%)
6. **Today's Schedule** (full width, bottom)

---

## KEY COMPONENTS

### 1. KPI Summary Cards
- **Description:** Five real-time operational metrics
- **Data displayed:**
  - Calls today (total, answer rate)
  - Queue now (count waiting, average wait time)
  - Leads (new today, qualified today)
  - Consults (scheduled total, scheduled today)
  - Intake SLA (% within target, target value)
- **Actions available:** Click card → drill-down with hourly breakdown chart
- Real-time updates via WebSocket

### 2. Call Queue & SLA Metrics
- **Description:** Live view of phone queues by IVR branch
- **Data displayed:** IVR branch name, current queue depth, average wait time, SLA percentage, abandoned call rate
- **Actions available:**
  - "Live call monitor →" links to UCaaS Console (Screen 8)
  - Supervisor can see agent status (on-call, available, away)
- **[ASSUMPTION]** IVR branches follow FSD: New Client, Existing Client Updates, Billing

### 3. Intake Pipeline
- **Description:** Stage-by-stage count of intake items for this office
- **Data displayed:** Stage name, count, 7-day trend arrow
- **Actions available:** Click stage → filtered view in Intake Console (Screen 4); "Open intake console →" navigates to Screen 4
- Includes "Fee Pending" as a stage — matters with required but unpaid consultation fees

### 4. Staffing & Capacity Snapshot
- **Description:** Current staffing status and capacity gap analysis
- **Data displayed:** Role, count in-office, count available, count OOO; open appointment slots vs. demand; capacity gap
- **Actions available:**
  - "Request capacity expansion →" sends notification to CLO for approval (not self-service for intake leads)
  - Hover on attorney → tooltip with their open slots and next availability

### 5. Bottleneck Warnings
- **Description:** Actionable alerts about operational issues
- **Data displayed:** Severity icon, description, impacted entity
- **Actions available:** Click warning → navigate to relevant screen (capacity control, intake record, matter)

### 6. Today's Schedule
- **Description:** Chronological list of today's consultations and appointments
- **Data displayed:** Time, attorney, client name (New/Returning indicator), matter type, consultation fee, payment status
- **Actions available:**
  - Click row → open intake record or matter workspace
  - Payment status indicators: ✅ Paid, ⏳ Pending, 🔴 Failed, ✅ N/A (waived/returning client)
  - "View full calendar →" navigates to calendar view

---

## INTERACTIONS & STATES

### Default State
All panels populated with live data. Today's schedule sorted chronologically.

### Empty State
- **No calls yet today:** "No calls received yet. Queue monitoring is active."
- **No consults scheduled:** "No consultations scheduled for today. [View this week →]"
- **No bottlenecks:** Section shows green "✅ No current bottlenecks" and collapses to single line

### Loading State
Skeleton shimmer per panel; KPI cards show "—" with shimmer. Real-time elements (queue) show "Connecting..." state.

### Error State
Per-panel: "Unable to load [panel]. [Retry]"
UCaaS integration down: Queue panel shows "Phone system connection unavailable. [Retry] [View system status]"

### Blocked/Gated State
- User without office assignment: "Please contact your administrator to be assigned to an office."

### Success/Confirmation State
- Capacity expansion request sent: toast "Capacity expansion request sent to CLO"

---

## AUTOMATIONS & AI ASSISTS

### What the system suggests
- **Bottleneck prediction:** AI identifies patterns (e.g., "Tuesdays typically see 40% more calls after 2pm — consider scheduling additional intake staff")
- **No-show risk:** Flags upcoming consultations with high no-show probability based on client type, channel, payment timing

### What requires human approval
- Capacity expansion requests require CLO approval
- No automated staffing changes from this screen

### How suggestions are surfaced visually
- AI insights shown as a blue "insight" row in the Bottleneck Warnings panel, visually distinct from operational warnings
- Tooltip: "Based on historical patterns"

---

## ROLE-BASED VISIBILITY

### CLO
- **Full read access** to all panels for any office
- Capacity section: can approve expansion requests directly from this screen
- **Editable:** Can approve/deny capacity requests inline

### COO
- **Full read access** to all panels for any office
- Emphasis on SLA and staffing metrics
- **Editable:** None from this screen (operational changes happen in Settings)

### Intake Specialist / Intake Lead
- **Full read access** for their assigned office only
- Cannot switch office selector (single office view)
- Capacity section: can request expansion but not approve
- **Editable:** None directly; actions link to Intake Console (Screen 4)

### Attorney / Paralegal / Billing / Marketing / Client
- **No access** to this screen

---

## EDGE CASES & GUARDRAILS

### Capacity exceeded
- Capacity snapshot shows red gap indicator: "⚠ Gap: -5"
- "Request capacity expansion →" button is prominent
- If CLO has already been notified and hasn't responded: shows "Expansion request pending (sent 2h ago)"

### Payment fails
- Today's Schedule: consultation with failed payment shows 🔴 status
- Cannot proceed with appointment confirmation until payment resolved
- Bottleneck warning: "[N] consult fees unpaid >24hrs — appointments not confirmed"

### Required documents missing
- Not primary on this screen; surfaced if intake questionnaire is incomplete in pipeline view

### Deadlines at risk
- Not primary on this screen; office-level deadline risk is shown as a bottleneck warning if it impacts capacity

### All queues empty / low volume day
- Queue panel: "All queues clear. 0 callers waiting." with green indicator
- **[ASSUMPTION]** System does not generate false urgency on low-volume days

### Multi-office intake leads
- **[ASSUMPTION]** Intake leads assigned to multiple offices see the office selector dropdown and can switch between offices. KPI cards reflect selected office only.
