# Screen 6: Attorney Capacity & Open Appointments Control Panel (CLO)

## Purpose
Enable the CLO to manage attorney capacity, control open appointment availability, run what-if simulations, and handle capacity expansion/throttling requests. This is the control center for the Attorney Capacity & Open Appointments Engine defined in the FSD.

## Primary Users
CLO (primary; full control), COO (read access + limited config), Managing Partner/CEO (read access)

## When This Screen Is Used (Workflow Stage)
Ongoing capacity management. Used when: reviewing weekly capacity, responding to expansion requests from intake, adjusting for PTO/OOO, running what-if scenarios for staffing decisions, addressing over/underutilization.

---

## LAYOUT (TEXTUAL WIREFRAME)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ [Global Nav Shell — see Screen 1]                                       │
├─────────────────────────────────────────────────────────────────────────┤
│ CAPACITY HEADER                                                         │
│ Attorney Capacity Control   [Office: All ▾] [Period: This Week ▾] [⟳] │
│ [Capacity Overview] [What-If Simulator] [Expansion Requests (2)]       │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│ TAB: CAPACITY OVERVIEW (default)                                        │
│                                                                         │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ FIRMWIDE CAPACITY SUMMARY                                         │  │
│ │                                                                   │  │
│ │ Total Attorneys: 12  │  Target Capacity: 480 hrs/wk              │  │
│ │ Active Capacity: 410 hrs/wk (85%)  │  Open Appts: 14            │  │
│ │ Consult Demand: 22 requests  │  Gap: -8 slots                    │  │
│ │                                                                   │  │
│ │ ┌─── Capacity by Practice Area ──────────────────────────────┐   │  │
│ │ │ Practice       │ Attys │ Util% │ Open │ Demand │ Gap      │   │  │
│ │ │ ─────────────────────────────────────────────────────────  │   │  │
│ │ │ Estate Plan.   │ 4     │ 84%   │ 6    │ 10     │ -4  🔴  │   │  │
│ │ │ Estate Admin.  │ 3     │ 91%   │ 2    │ 5      │ -3  🔴  │   │  │
│ │ │ Elder Law      │ 2     │ 72%   │ 4    │ 3      │ +1  🟢  │   │  │
│ │ │ Tax Planning   │ 2     │ 68%   │ 3    │ 2      │ +1  🟢  │   │  │
│ │ │ Lit./Fiduciary │ 1     │ 78%   │ 1    │ 2      │ -1  🟡  │   │  │
│ │ └────────────────────────────────────────────────────────────┘   │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ ATTORNEY CAPACITY DETAIL TABLE                                    │  │
│ │                                                                   │  │
│ │ Attorney    │Office│Prac │Tgt │Active│Util%│Open│Complex│Deadline│  │
│ │                    │Area │Hrs │Mtrs  │     │Apt │Weight │Pressure│  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ J. Smith    │Main │EP/EA│40  │ 18   │ 92% │ 0  │ High  │ 🔴 3  │  │
│ │ K. Park     │Main │EP/EL│40  │ 14   │ 78% │ 3  │ Med   │ 🟢 0  │  │
│ │ L. Chen     │Main │Tax  │40  │ 11   │ 45% │ 4  │ Low   │ 🟢 0  │  │
│ │ M. Jones    │Down │EP/EA│40  │ 16   │ 88% │ 1  │ High  │ 🟡 1  │  │
│ │ P. Williams │Down │EL   │32  │ 10   │ 72% │ 3  │ Med   │ 🟢 0  │  │
│ │ ...                                                               │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ [Click row for detail]  [Sort by any column]  [Export]           │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ATTORNEY DETAIL (slide-over on row click)                               │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ J. Smith — Capacity Detail                                        │  │
│ │                                                                   │  │
│ │ Target weekly hours: 40                                           │  │
│ │ Active matters: 18 (12 EP, 6 EA)                                 │  │
│ │ Complexity-weighted load: 47 hrs equivalent                      │  │
│ │ Utilization: 92% ── 🔴 OVER THRESHOLD                           │  │
│ │                                                                   │  │
│ │ ── Capacity Factors ──                                            │  │
│ │ Active matter load:         ████████████████████░░  18/20 cap    │  │
│ │ Complexity weighting:       ██████████████████████░  High         │  │
│ │ Deadline pressure:          █████████████████░░░░░  3 statutory  │  │
│ │ SLA commitments:            ██████████░░░░░░░░░░░░  Normal       │  │
│ │ PTO/OOO this period:        ░░░░░░░░░░░░░░░░░░░░░  None         │  │
│ │                                                                   │  │
│ │ ── Open Appointments ──                                           │  │
│ │ This week: 0 slots open (capacity engine closed all)             │  │
│ │ Next week: 1 slot open (projected capacity recovery)             │  │
│ │                                                                   │  │
│ │ ── Capacity Actions (CLO only) ──                                 │  │
│ │ [+ Add open appointment slot]  (override — logged)               │  │
│ │ [Reassign matters →]                                              │  │
│ │ [Adjust target hours]                                             │  │
│ │ [Temporarily close to new intake]                                 │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: WHAT-IF SIMULATOR                                                  │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Scenario Builder                                                  │  │
│ │                                                                   │  │
│ │ Base: Current state          [Reset to current]                  │  │
│ │                                                                   │  │
│ │ Adjustments:                                                      │  │
│ │ ┌─────────────────────────────────────────────────────────────┐  │  │
│ │ │ + Add adjustment                                            │  │  │
│ │ │                                                             │  │  │
│ │ │ 1. J. Smith: Reduce target to 32 hrs (partial caseload)   │  │  │
│ │ │ 2. New hire: Add attorney, EP, 40 hrs, start week of 4/7  │  │  │
│ │ │ 3. K. Park: PTO week of 3/31                               │  │  │
│ │ └─────────────────────────────────────────────────────────────┘  │  │
│ │                                                                   │  │
│ │ [Run Simulation]                                                  │  │
│ │                                                                   │  │
│ │ ── Simulation Results ──                                          │  │
│ │ ┌──────────────────────────────┬──────────────────────────────┐  │  │
│ │ │ CURRENT STATE               │ SIMULATED STATE               │  │  │
│ │ │ Total capacity: 480 hrs     │ Total capacity: 472 hrs      │  │  │
│ │ │ Utilization: 85%            │ Utilization: 79%             │  │  │
│ │ │ Open appts: 14              │ Open appts: 18              │  │  │
│ │ │ Demand gap: -8              │ Demand gap: -4              │  │  │
│ │ │ Revenue impact: —           │ Revenue impact: +$12K/mo    │  │  │
│ │ └──────────────────────────────┴──────────────────────────────┘  │  │
│ │                                                                   │  │
│ │ [Apply changes] [Save scenario] [Discard]                        │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: EXPANSION REQUESTS (2)                                             │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Pending Requests                                                  │  │
│ │                                                                   │  │
│ │ ┌─────────────────────────────────────────────────────────────┐  │  │
│ │ │ 🟡 Main Street — Estate Planning                            │  │  │
│ │ │ Requested by: A. Johnson (Intake Lead)                      │  │  │
│ │ │ Reason: Demand exceeds capacity by 4 consults this week    │  │  │
│ │ │ Requested: Mar 24, 10:15 AM                                │  │  │
│ │ │ Current gap: -4 slots                                      │  │  │
│ │ │                                                             │  │  │
│ │ │ Suggested resolution (system):                              │  │  │
│ │ │ • Open 2 additional slots for K. Park (current: 78% util) │  │  │
│ │ │ • Open 1 additional slot for L. Chen (current: 45% util)  │  │  │
│ │ │                                                             │  │  │
│ │ │ [Approve as suggested] [Modify & approve] [Deny] [Defer]  │  │  │
│ │ └─────────────────────────────────────────────────────────────┘  │  │
│ │                                                                   │  │
│ │ ┌─────────────────────────────────────────────────────────────┐  │  │
│ │ │ 🟡 Downtown — Estate Administration                         │  │  │
│ │ │ Requested by: C. Martinez (Intake)                          │  │  │
│ │ │ Reason: 6 new estate admin intakes, only 2 slots available │  │  │
│ │ │ Requested: Mar 24, 11:30 AM                                │  │  │
│ │ │ [Approve as suggested] [Modify & approve] [Deny] [Defer]  │  │  │
│ │ └─────────────────────────────────────────────────────────────┘  │  │
│ │                                                                   │  │
│ │ ── Request History ──                                             │  │
│ │ Mar 23: Main St EP — Approved (2 slots added to K. Park)        │  │
│ │ Mar 20: Suburban EL — Denied (PTO week, insufficient coverage)  │  │
│ │ [View all history →]                                              │  │
│ └───────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## KEY COMPONENTS

### 1. Firmwide Capacity Summary
- **Description:** Aggregated capacity metrics across the firm, broken down by practice area
- **Data displayed:** Total attorneys, target capacity hours, active capacity, utilization %, open appointments, consult demand, gap; per practice area breakdown
- **Actions available:** Filter by office; click practice area row for attorney-level detail

### 2. Attorney Capacity Detail Table
- **Description:** Per-attorney capacity breakdown with all factors from the FSD capacity engine
- **Data displayed:** Attorney name, office, practice areas, target hours, active matter count, utilization %, open appointment slots, complexity weighting, deadline pressure indicator
- **Actions available:** Click row → slide-over with full detail; Sort by any column; Export
- **Capacity factors per FSD:** Target weekly capacity, active matter load, complexity weighting, deadline pressure, SLA commitments, PTO/OOO

### 3. Attorney Detail Slide-Over
- **Description:** Full capacity breakdown for a single attorney with action controls
- **Data displayed:** All capacity factors visualized as progress bars; open appointment status; projected capacity for next period
- **Actions available (CLO only):**
  - Add open appointment slot (override, audit logged)
  - Reassign matters to another attorney
  - Adjust target hours
  - Temporarily close attorney to new intake
- **All overrides create audit trail entries per FSD**

### 4. What-If Simulator
- **Description:** Scenario planning tool for CLO to model capacity changes before committing
- **Data displayed:** Current state vs. simulated state side-by-side; adjustable parameters (target hours, new hires, PTO, matter reassignments)
- **Actions available:**
  - Add adjustments (multiple per scenario)
  - Run simulation → compare current vs. projected metrics
  - Apply changes (commits adjustments to the live system)
  - Save scenario (for future reference / meeting prep)
  - Discard (no changes)
- **Revenue impact:** Estimated based on average fee per consult × additional slots opened
- **[ASSUMPTION]** Simulations use a simplified model; CLO understands these are estimates

### 5. Expansion Request Queue
- **Description:** Pending capacity expansion requests from intake leads
- **Data displayed:** Office, practice area, requester, reason, timestamp, current gap, system-suggested resolution
- **Actions available:**
  - Approve as suggested (applies system recommendation)
  - Modify & approve (CLO adjusts which attorneys/slots, then approves)
  - Deny (with reason, sent back to requester)
  - Defer (acknowledge but delay decision)
- **Notification:** Intake requester receives real-time notification of decision

---

## INTERACTIONS & STATES

### Default State
Capacity Overview tab active. All attorneys listed. Summary metrics populated.

### Empty State
- **No attorneys configured:** "No attorneys configured for capacity management. [Add attorneys in Settings →]"
- **No expansion requests:** Tab shows "(0)" and message "No pending requests."

### Loading State
Table renders with skeleton rows. Summary cards show shimmer. Simulator shows "Ready to build scenario."

### Error State
- Capacity engine unavailable: "Capacity calculations are temporarily unavailable. Showing last known state from [timestamp]. [Retry]"
- Simulation failure: "Unable to run simulation. [Retry]" — does not affect live capacity view

### Blocked/Gated State
- Non-CLO user accessing actions: action buttons hidden; read-only view rendered
- **[ASSUMPTION]** COO can view all data but cannot execute capacity changes

### Success/Confirmation State
- Override applied: toast "Open appointment added for [Attorney] on [date]. Audit trail updated."
- Expansion approved: toast "Expansion approved. [N] slots opened. Intake notified."
- Expansion denied: toast "Expansion request denied. Intake notified."
- Simulation applied: modal confirmation "Apply these changes? This will update attorney capacity and open appointment availability." [Confirm] [Cancel]

---

## AUTOMATIONS & AI ASSISTS

### What the system suggests
- **Auto-calculated capacity:** System continuously computes per-attorney utilization using the FSD formula (target hours, matter load, complexity, deadline pressure, SLA, PTO)
- **Open appointment management:** System automatically opens/closes appointment slots based on capacity — this is the core engine behavior
- **Expansion request recommendations:** System suggests which attorneys to expand and by how many slots based on underutilization and practice area match
- **Forecasting:** Capacity panel shows projected utilization for next 4 weeks based on scheduled events, PTO, and pipeline

### What requires human approval
- All overrides to capacity-approved slots (CLO only; audit logged)
- Expansion request approval/denial (CLO only)
- Applying what-if simulation results to live system (CLO only)
- Target hour adjustments (CLO only)
- Matter reassignments (CLO only)

### How suggestions are surfaced visually
- System suggestions in expansion requests are displayed in a distinct "Suggested resolution" block with 🤖 indicator
- Capacity engine decisions (opening/closing slots) are shown as system actions in the audit log but not as "suggestions" — they are the default behavior that CLO can override

---

## ROLE-BASED VISIBILITY

### CLO
- **Full read/write access**
- All action buttons visible (add slots, reassign, adjust hours, close to intake)
- What-if simulator fully interactive
- Expansion request queue with approve/deny/modify controls
- **Editable:** All capacity parameters, overrides, expansion decisions

### CEO / Managing Partner
- **Read-only** access to Capacity Overview tab
- Cannot access What-If Simulator or Expansion Requests
- **Editable:** None

### COO
- **Read access** to all tabs including Expansion Requests
- Can view but not action expansion requests
- Can view What-If Simulator in read-only mode (can build scenarios but not apply them)
- **[ASSUMPTION]** COO can escalate observations to CLO but cannot directly modify capacity
- **Editable:** None

### Intake / Attorney / Paralegal / Billing / Marketing / Client
- **No access** to this screen

---

## EDGE CASES & GUARDRAILS

### Capacity exceeded
- This IS the screen for resolving capacity issues
- Over-threshold attorneys highlighted in red rows
- System has already closed their appointment slots
- CLO can override and force-open slots — this creates a prominent audit trail entry and shows a confirmation: "Opening slots for an over-capacity attorney. This may impact service delivery quality. [Confirm override]"

### Payment fails
- Not applicable to this screen

### Required documents missing
- Not applicable to this screen

### Deadlines at risk
- Deadline pressure column shows statutory deadline count per attorney
- If an attorney has high deadline pressure AND high utilization: compound risk indicator 🔴🔴
- Tooltip: "This attorney has [N] statutory deadlines and [X]% utilization. Consider reassigning non-urgent matters."

### CLO applies simulation that causes issues
- Before applying: system shows impact summary including any attorneys that would go over threshold
- Warning: "This change will put [Attorney] above 90% utilization. Proceed?" [Confirm] [Adjust]

### Concurrent expansion requests
- If multiple offices request expansion for the same practice area: system groups them and shows aggregate demand
- CLO sees combined view: "Total unmet demand across offices: [N] slots for [practice area]"

### Audit trail
- Every override, approval, denial, and configuration change is logged with: timestamp, actor (CLO), action, previous value, new value, reason (if provided)
- Audit log accessible from this screen via "View audit trail →" link in each detail panel
- **[ASSUMPTION]** Audit trail is append-only and cannot be modified or deleted
