# AC-03: Office Dashboard

## Feature Overview
Office-level operational dashboard for Intake Leads and COO/CLO to monitor call volume, intake pipeline, staffing load, and SLA performance.

---

## AC-03.1 — Access Control

**Given** a user with role Intake Specialist (Intake Lead) navigates to Dashboard
**Then** the Office Dashboard for their assigned office is displayed

**Given** a user with role CLO or COO navigates to Office Dashboard
**Then** they can view any office via the office selector dropdown

**Given** an Intake Specialist is assigned to a single office
**Then** the office selector is not available to them — they see their assigned office only

**Given** a user with role Attorney, Paralegal, Billing, or Marketing
**Then** they have no access to this screen

**Given** a user has no office assignment
**Then** the message displays: "Please contact your administrator to be assigned to an office."

---

## AC-03.2 — Header & Filters

**Given** the Office Dashboard loads
**Then** the header shows: office selector (CLO/COO only), period selector (default: Today; options: Today, This Week, This Month), and a refresh button

---

## AC-03.3 — KPI Summary Cards

**Given** the dashboard loads
**Then** five KPI cards display with real-time data:
- Calls today (total count, answer rate %)
- Queue now (callers waiting, average wait time)
- Leads today (new count, qualified count)
- Consults (total scheduled, scheduled today)
- Intake SLA (% within target, target value)

**Given** a user clicks a KPI card
**Then** a drill-down opens showing an hourly breakdown chart

**Given** the dashboard is open
**Then** KPI cards update in real-time via WebSocket

---

## AC-03.4 — Call Queue & SLA Metrics

**Given** the call queue panel loads
**Then** it shows per IVR branch: branch name (New Client, Existing Client Updates, Billing), current queue depth, average wait time

**Given** the SLA metric is displayed
**Then** it shows: % of calls answered within target threshold (e.g., 80% < 30s) and abandoned call rate (target < 5%)

**Given** a user clicks "Live call monitor →"
**Then** they are navigated to Screen 8 (UCaaS Console)

**Given** the UCaaS integration is unavailable
**Then** the queue panel shows: "Phone system connection unavailable. [Retry] [View system status]"

---

## AC-03.5 — Intake Pipeline

**Given** the pipeline panel loads
**Then** it shows per stage: stage name, count, and 7-day trend arrow

**Given** stages are listed
**Then** the following stages are shown: New Leads, Qualified, Scheduled, Fee Pending, Consult Done, Engaged

**Given** a user clicks a stage
**Then** they are navigated to Screen 4 (Intake Console) filtered to that stage

**Given** a user clicks "Open intake console →"
**Then** they are navigated to Screen 4

---

## AC-03.6 — Staffing & Capacity Snapshot

**Given** the staffing panel loads
**Then** it shows per role: role name, count in-office, count available, count OOO

**Given** the capacity section is displayed
**Then** it shows: open appointment slots this week, demand (consult requests), and gap indicator

**Given** the gap is negative (demand > slots)
**Then** the gap displays in red: "⚠ Gap: -[N]"

**Given** an Intake Lead clicks "Request capacity expansion →"
**Then** a notification is sent to CLO for approval (not self-service)

**Given** CLO has already been notified about a capacity expansion request but hasn't responded
**Then** the button shows: "Expansion request pending (sent [X]h ago)"

**Given** a user hovers over an attorney name
**Then** a tooltip shows their open slots and next availability

---

## AC-03.7 — Bottleneck Warnings

**Given** operational issues exist
**Then** the Bottleneck Warnings panel shows each issue with a severity icon (🔴/🟡) and a description linked to an impacted entity

**Given** a user clicks a warning
**Then** they are navigated to the relevant screen (capacity control, intake record, or matter)

**Given** there are no bottlenecks
**Then** the panel collapses to a single green line: "✅ No current bottlenecks"

---

## AC-03.8 — Today's Schedule

**Given** the schedule panel loads
**Then** it shows each appointment in chronological order with: time, attorney name, client name (New/Returning indicator), matter type, consultation fee, and payment status

**Given** payment statuses are displayed
**Then** they are: ✅ Paid, ⏳ Pending, 🔴 Failed, ✅ N/A (waived/returning client)

**Given** an appointment has a failed payment
**Then** the 🔴 Failed status is shown and the appointment is not confirmed

**Given** a user clicks an appointment row
**Then** they are navigated to the intake record or matter workspace for that appointment

**Given** a user clicks "View full calendar →"
**Then** they are navigated to Screen 5 (Appointment Scheduling)

---

## AC-03.9 — Capacity Expansion (CLO Role)

**Given** the user is CLO and viewing an Office Dashboard
**Then** they can approve capacity expansion requests directly from the staffing panel

---

## AC-03.10 — AI Suggestions

**Given** the system detects a pattern (e.g., high call volume on Tuesday afternoons)
**Then** an AI insight row appears in the Bottleneck Warnings panel with a blue "insight" style: "Based on historical patterns: [suggestion]"

**Given** upcoming consultations have clients matching high no-show risk patterns
**Then** the relevant appointments are flagged with a ⚠ indicator

---

## AC-03.11 — Loading & Error States

**Given** the dashboard is loading
**Then** all panels show skeleton shimmer; KPI cards show "—"; real-time elements show "Connecting..."

**Given** a panel fails to load
**Then** it shows: "Unable to load [panel]. [Retry]" without affecting other panels

---

## AC-03.12 — Empty States

**Given** no calls have been received yet today
**Then** the call panel shows: "No calls received yet. Queue monitoring is active."

**Given** no consultations are scheduled today
**Then** the schedule panel shows: "No consultations scheduled for today. [View this week →]"

**Given** all queues are empty
**Then** the queue panel shows: "All queues clear. 0 callers waiting." with a green indicator

---

## AC-03.13 — Payment Failure in Today's Schedule

**Given** a consultation's payment has failed
**Then** the appointment row shows 🔴 Failed status

**Given** N consultations have unpaid fees > 24 hours
**Then** a bottleneck warning shows: "[N] consult fees unpaid >24hrs — appointments not confirmed"
