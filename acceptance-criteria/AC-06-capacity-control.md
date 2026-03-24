# AC-06: Attorney Capacity & Open Appointments Control Panel

## Feature Overview
CLO control center for managing attorney capacity, open appointment availability, what-if simulations, and capacity expansion requests.

---

## AC-06.1 — Access Control

**Given** a user with role CLO navigates to Capacity Control
**Then** they have full read/write access to all tabs and all action controls

**Given** a user with role CEO
**Then** they have read-only access to the Capacity Overview tab only; What-If Simulator and Expansion Requests tabs are not visible

**Given** a user with role COO
**Then** they have read access to all tabs including Expansion Requests and can view but not action What-If Simulator; they cannot approve or deny expansion requests

**Given** a user with role Intake, Attorney, Paralegal, Billing, or Marketing
**Then** they have no access to this screen

---

## AC-06.2 — Header & Filters

**Given** the Capacity Control screen loads
**Then** the header shows: office filter (All or specific office), period filter (This Week, etc.), and a refresh button

**Given** the Expansion Requests tab has pending requests
**Then** the tab label shows the count: "Expansion Requests ([N])"

---

## AC-06.3 — Firmwide Capacity Summary

**Given** the Capacity Overview tab is active
**Then** the summary shows: total attorneys, target capacity hours/week, active capacity, utilization %, open appointment count, consult demand count, and gap

**Given** the practice area breakdown is shown
**Then** each practice area shows: attorney count, utilization %, open appointments, demand, and gap with a color indicator (🔴 negative gap, 🟡 near threshold, 🟢 positive gap)

**Given** a user clicks a practice area row
**Then** the view drills down to attorney-level detail for that practice area

---

## AC-06.4 — Attorney Capacity Detail Table

**Given** the capacity table loads
**Then** each attorney row shows: name, office, practice areas, target hours, active matter count, utilization %, open appointment slots, complexity weighting, and deadline pressure indicator

**Given** an attorney's utilization is at/above 90% (over threshold)
**Then** their row is highlighted in red

**Given** a user clicks a column header
**Then** the table sorts by that column

**Given** a user clicks an attorney row
**Then** an attorney detail slide-over opens

**Given** the user is CLO and clicks "Export"
**Then** a data export is triggered

---

## AC-06.5 — Attorney Detail Slide-Over

**Given** an attorney detail slide-over opens
**Then** it shows all capacity factors as visual progress bars:
- Active matter load (count vs. cap)
- Complexity weighting
- Deadline pressure (statutory deadline count)
- SLA commitments
- PTO/OOO this period

**Given** the slide-over is displayed
**Then** it also shows: open appointment status for this week and projected next week

**Given** the user is CLO
**Then** the following action buttons are visible:
- [+ Add open appointment slot] (override — logged in audit trail)
- [Reassign matters →]
- [Adjust target hours]
- [Temporarily close to new intake]

**Given** the CLO adds an open appointment slot for an over-capacity attorney
**Then** a confirmation modal displays: "Opening slots for an over-capacity attorney. This may impact service delivery quality. [Confirm override]"

**Given** an override is confirmed
**Then** an audit trail entry is created with: timestamp, actor, action, previous value, new value, and any reason provided

**Given** all override and configuration changes
**Then** they are logged in an append-only audit trail that cannot be modified or deleted

---

## AC-06.6 — What-If Simulator

**Given** the CLO opens the What-If Simulator tab
**Then** they see the current state metrics and a "Scenario Builder" panel

**Given** the CLO adds adjustments to the scenario
**Then** available adjustment types include: reduce/increase attorney target hours, add new hire (practice, hours, start date), add PTO period, reassign matters

**Given** the CLO clicks "Run Simulation"
**Then** a side-by-side comparison displays: current state vs. simulated state for: total capacity, utilization %, open appointments, demand gap, and estimated revenue impact

**Given** the CLO clicks "Apply changes"
**Then** a confirmation modal displays: "Apply these changes? This will update attorney capacity and open appointment availability." with [Confirm] [Cancel]

**Given** the CLO applies a simulation that would put an attorney above 90% utilization
**Then** the system shows a warning before allowing confirmation: "This change will put [Attorney] above 90% utilization. Proceed?" [Confirm] [Adjust]

**Given** the CLO clicks "Save scenario"
**Then** the scenario is saved with a name for future reference/meeting prep

**Given** the CLO clicks "Discard"
**Then** no changes are applied to the live system

**Given** the simulation fails to run
**Then** the error message displays: "Unable to run simulation. [Retry]" without affecting the live capacity view

---

## AC-06.7 — Expansion Request Queue

**Given** the Expansion Requests tab is open
**Then** each pending request shows: office, practice area, requester name, reason, timestamp, current gap, and system-suggested resolution

**Given** the system-suggested resolution is displayed
**Then** it includes specific attorneys and slot counts with a 🤖 indicator

**Given** the CLO clicks "Approve as suggested"
**Then** the system recommendation is applied, slots are opened, and the requesting intake lead receives a real-time notification

**Given** the CLO clicks "Modify & approve"
**Then** the CLO can adjust which attorneys/slots are opened before approving

**Given** the CLO clicks "Deny"
**Then** a reason can be provided, and the requesting intake lead is notified of the denial with the reason

**Given** the CLO clicks "Defer"
**Then** the request remains pending with a deferral note; the requester is acknowledged

**Given** an approval is executed
**Then** a toast displays: "Expansion approved. [N] slots opened. Intake notified."

**Given** a denial is executed
**Then** a toast displays: "Expansion request denied. Intake notified."

---

## AC-06.8 — Concurrent Expansion Requests

**Given** multiple offices request expansion for the same practice area
**Then** the system groups them and shows an aggregate view: "Total unmet demand across offices: [N] slots for [practice area]"

---

## AC-06.9 — Automated Capacity Engine

**Given** the system is running
**Then** it continuously computes per-attorney utilization using: target hours, active matter load, complexity weighting, deadline pressure, SLA commitments, and PTO/OOO

**Given** the capacity engine determines an attorney is over capacity
**Then** their appointment slots are automatically closed — this is the default engine behavior (not a suggestion)

**Given** the CLO overrides a capacity engine decision
**Then** this creates an audit trail entry and the override is visible in the attorney detail

---

## AC-06.10 — Deadline Pressure Indicators

**Given** an attorney has both high utilization and high deadline pressure
**Then** a compound risk indicator 🔴🔴 is shown with tooltip: "This attorney has [N] statutory deadlines and [X]% utilization. Consider reassigning non-urgent matters."

---

## AC-06.11 — Loading & Error States

**Given** the screen is loading
**Then** the table renders with skeleton rows and summary cards shimmer

**Given** the capacity engine is unavailable
**Then** the message displays: "Capacity calculations are temporarily unavailable. Showing last known state from [timestamp]. [Retry]"

**Given** no attorneys are configured
**Then** the message displays: "No attorneys configured for capacity management. [Add attorneys in Settings →]"

**Given** no expansion requests are pending
**Then** the Expansion Requests tab shows "(0)" and the message: "No pending requests."
