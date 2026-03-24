# Screen 5: Appointment Scheduling & Confirmation Flow

## Purpose
Manage the end-to-end consultation appointment lifecycle: booking on capacity-approved slots, automated confirmations (SMS + email), reminder sequences, reschedule/cancel handling, and no-show tracking. Ensures no appointment is confirmed until payment (when required) is complete.

## Primary Users
Intake Specialist (primary), Attorney (view own schedule), CLO (oversight), Client (receives confirmations; can reschedule via link)

## When This Screen Is Used (Workflow Stage)
Consult Scheduled stage. Triggered after qualification and fee payment on the Intake Console (Screen 4).

---

## LAYOUT (TEXTUAL WIREFRAME)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ [Global Nav Shell — see Screen 1]                                       │
├─────────────────────────────────────────────────────────────────────────┤
│ SCHEDULING HEADER                                                       │
│ Appointment Scheduling    [Office: Main St ▾]  [Week ◀ Mar 24–28 ▶]   │
│ [Calendar View] [List View] [No-Show Analytics]                        │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│ CALENDAR VIEW (default)                                                 │
│ ┌───────┬───────────┬───────────┬───────────┬───────────┬───────────┐  │
│ │       │ Mon 3/24  │ Tue 3/25  │ Wed 3/26  │ Thu 3/27  │ Fri 3/28 │  │
│ │       ├───────────┼───────────┼───────────┼───────────┼──────────┤  │
│ │K.Park │ 9:00 ✅   │ 10:00 ✅  │ ░░░░░░░░ │ 9:00 🔲  │ 10:00 🔲│  │
│ │       │ Garcia    │ Lee       │ [OOO]    │ [OPEN]   │ [OPEN]  │  │
│ │       │ 10:00 ✅  │ 2:00 ⏳   │          │ 11:00 🔲 │         │  │
│ │       │ Thompson  │ Martinez  │          │ [OPEN]   │         │  │
│ │       ├───────────┼───────────┼───────────┼───────────┼──────────┤  │
│ │L.Chen │ 10:30 ✅  │ 9:00 🔲  │ 9:00 🔲  │ ░░░░░░░░ │ 9:00 🔲 │  │
│ │       │ Williams  │ [OPEN]   │ [OPEN]   │ [PTO]    │ [OPEN]  │  │
│ │       │ 2:30 ✅   │ 1:00 🔲  │ 2:00 🔲  │          │ 1:00 🔲 │  │
│ │       │ Roberts   │ [OPEN]   │ [OPEN]   │          │ [OPEN]  │  │
│ │       ├───────────┼───────────┼───────────┼───────────┼──────────┤  │
│ │J.Smith│ 1:00 🔴   │ ░░░░░░░░ │ 10:00 🔲 │ 9:00 🔲  │ ░░░░░░░ │  │
│ │       │ Davis     │ [FULL]   │ [OPEN]   │ [OPEN]   │ [FULL]  │  │
│ │       │ ⚠ Fee!   │          │          │          │         │  │
│ └───────┴───────────┴───────────┴───────────┴───────────┴──────────┘  │
│                                                                         │
│ Legend: ✅ Confirmed  ⏳ Awaiting confirm  🔴 Fee pending  🔲 Open     │
│         ░░ Unavailable (OOO/PTO/Full)                                  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ APPOINTMENT DETAIL (slide-over on click)                                │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Consultation: Maria Garcia                                        │  │
│ │ Date: Mon, Mar 24, 2026 at 9:00 AM                               │  │
│ │ Attorney: K. Park                                                 │  │
│ │ Practice: Estate Planning                                         │  │
│ │ Location: Main Street Office, Room 2                              │  │
│ │ Duration: 60 min                                                  │  │
│ │                                                                   │  │
│ │ ── Fee Status ──                                                  │  │
│ │ Consultation fee: $250.00 — ✅ PAID (Mar 24, 9:05 AM, Visa *4521)│  │
│ │ Credit to engagement: Yes (if client engages)                     │  │
│ │                                                                   │  │
│ │ ── Confirmation Status ──                                         │  │
│ │ ✅ Email confirmation sent: Mar 24, 9:06 AM                       │  │
│ │ ✅ SMS confirmation sent: Mar 24, 9:06 AM                         │  │
│ │ ⏳ 24-hr reminder: Scheduled for Mar 23, 9:00 AM [NOT APPLICABLE]│  │
│ │ ⏳ Same-day reminder: Scheduled for Mar 24, 7:00 AM              │  │
│ │                                                                   │  │
│ │ ── Consult Prep (for attorney) ──                                 │  │
│ │ Underwriting: HIGH (7/10)                                         │  │
│ │ Key concerns: Guardianship, tax minimization                      │  │
│ │ Suggested strategies: Comprehensive estate plan w/ trust          │  │
│ │ Suggested pricing: Fixed $3,500–$5,500                            │  │
│ │ [View full intake record →]                                       │  │
│ │                                                                   │  │
│ │ [Reschedule] [Cancel Appointment] [Mark No-Show] [Mark Complete] │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ CONFIRMATION & REMINDER AUTOMATION LOG                                  │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Today's Outbound Communications                                   │  │
│ │                                                                   │  │
│ │ 9:06 AM  📧 Confirmation → Garcia (m.garcia@email.com) ✅ Sent   │  │
│ │ 9:06 AM  📱 Confirmation → Garcia (+15551234567) ✅ Delivered    │  │
│ │ 7:00 AM  📧 Reminder → Roberts (+15559876543) ✅ Delivered       │  │
│ │ 7:00 AM  📱 Reminder → Roberts (r.roberts@email.com) ✅ Sent     │  │
│ │ 7:01 AM  📧 Reminder → Martinez (p.martinez@...) ⚠ Bounced      │  │
│ │                                                                   │  │
│ │ [View all communications →]                                       │  │
│ └───────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## KEY COMPONENTS

### 1. Calendar View
- **Description:** Weekly calendar grid showing attorney schedules with capacity-approved open slots
- **Data displayed:** Attorney rows × day columns; each cell shows appointment (with client name, status) or slot status (open, unavailable)
- **Actions available:**
  - Click open slot → opens booking modal (pre-filled with practice area from intake)
  - Click appointment → opens appointment detail slide-over
  - Navigate weeks with ◀ ▶ arrows
  - Filter by attorney, practice area

### 2. List View (alternate tab)
- **Description:** Chronological list of all appointments for the selected period
- **Data displayed:** Date/time, attorney, client, practice, fee status, confirmation status
- **Actions available:** Sort by any column, filter, click to open detail

### 3. Appointment Detail (Slide-Over)
- **Description:** Full detail view of a specific appointment
- **Data displayed:**
  - Client name, date/time, attorney, practice area, location/video link, duration
  - Fee status with payment details
  - Confirmation/reminder status with timestamps and delivery status
  - Consult prep summary (underwriting score, key concerns, suggested strategies/pricing)
- **Actions available:** Reschedule, Cancel, Mark No-Show, Mark Complete, View full intake record
- **Consult prep is read-only for intake; attorney can add notes after consult**

### 4. Confirmation & Reminder Automation Log
- **Description:** Audit trail of all automated confirmations and reminders
- **Data displayed:** Timestamp, type (email/SMS), recipient, content summary, delivery status
- **Actions available:** Resend failed message, View message content

### 5. No-Show Analytics (tab)
- **Description:** Analytics view of no-show patterns per FSD requirement
- **Data displayed:**
  - No-show rate by: client type (new/returning), channel (referral/web/ad), practice area, attorney, time of day
  - Trend charts (weekly/monthly)
  - Impact: lost revenue, wasted capacity hours
- **Actions available:** Filter by dimensions, export data
- **Feeds into:** Underwriting scoring and capacity logic

---

## INTERACTIONS & STATES

### Default State
Calendar view showing current week. Open (capacity-approved) slots clearly visible. Confirmed appointments show client names.

### Empty State
- **No appointments this week:** "No consultations scheduled for this week. Open slots are available — [go to Intake Console to schedule]."
- **No open slots:** "No available capacity this week. [Request capacity expansion →]"

### Loading State
Calendar grid renders immediately with shimmer in appointment cells. Slide-over loads with skeleton blocks.

### Error State
- Booking conflict (race condition): "This slot was just booked by another user. Please select a different time."
- Confirmation send failure: "⚠ SMS delivery failed for [client]. [Retry] [Send email only]"
- Calendar sync failure: "Unable to sync attorney calendar. Showing last known availability."

### Blocked/Gated States
1. **Fee unpaid:** Open slot click → modal shows "Consultation fee must be collected before booking. [Collect on Intake Console →]"
2. **Conflict unresolved:** Booking blocked. "Conflict check must be resolved before scheduling."
3. **Attorney at capacity:** No open slots shown for that attorney. ░░ [FULL] displayed.
4. **Outside business hours:** Slots outside configured business hours not shown. **[ASSUMPTION]** Business hours configurable per office per FSD.

### Success/Confirmation State
- Booking confirmed: Calendar updates in real-time; toast "Appointment confirmed. Confirmations sending..."
- Confirmation sent: Log entry appears with ✅
- Appointment completed: Cell updates to show completed indicator; matter transitions to "Consult Completed" stage

---

## AUTOMATIONS & AI ASSISTS

### What the system suggests
- **Optimal slot suggestion:** When booking from Intake Console, system highlights "recommended" slots based on attorney's expertise match and schedule density
- **No-show risk flag:** Appointments with clients matching high no-show risk patterns show subtle ⚠ indicator on calendar cell
- **Reminder timing:** System auto-configures reminder schedule per office settings

### What requires human approval
- Appointment booking (intake specialist confirms)
- Cancellation (intake specialist or client via link)
- No-show marking (intake specialist; cannot be automated per FSD human-in-the-loop requirements)
- Rescheduling (intake specialist or client via link; subject to capacity availability)

### Automated without approval (per FSD)
- SMS confirmation sent immediately on booking (where permitted)
- Email confirmation sent immediately on booking
- 24–48 hour reminder (configurable per office)
- Optional same-day reminder (configurable per office)
- All confirmations/reminders logged in intake/matter timeline

### Confirmation message content (per FSD)
- Date, time, location or video link
- Attorney name
- Purpose / practice area
- Cancellation/reschedule link
- **[ASSUMPTION]** Message templates configurable by office; system provides defaults

---

## ROLE-BASED VISIBILITY

### CLO
- **Full read access** across all offices
- Can view no-show analytics for strategic decisions
- Can see all attorney calendars and capacity
- **Editable:** None directly; capacity managed on Screen 6

### Intake Specialist
- **Full read/write** for their office
- Can book, reschedule, cancel appointments
- Can mark no-shows and appointment completion
- Can view consult prep summaries
- Cannot see open slots outside capacity-approved windows
- **Editable:** Appointment booking, reschedule, cancel, status updates

### Attorney
- **Read-only** view of their own schedule (via Calendar nav item, not this screen directly)
- Sees consult prep summary for upcoming appointments on their Matter Workspace
- Cannot reschedule or cancel from this view — must request via intake
- **[ASSUMPTION]** Attorneys see a simplified personal calendar view; Intake Console calendar is the system-of-record

### COO
- **Full read access** across all offices
- Emphasis on SLA and utilization metrics in analytics
- **Editable:** None

### Paralegal / Billing / Marketing
- **No access** to this screen

### Client
- **Does not see this screen.** Client receives:
  - Confirmation email/SMS with appointment details
  - Reminder email/SMS at configured intervals
  - Cancellation/reschedule link (opens a simple client-facing form, not this screen)
  - **[ASSUMPTION]** Client reschedule link shows available slots for the same attorney/practice; if none available within 2 weeks, prompts to call the office

---

## EDGE CASES & GUARDRAILS

### Capacity exceeded
- Calendar shows ░░ [FULL] for attorney-days with no remaining capacity
- No open 🔲 slots displayed = no way to book (system enforced)
- If ALL attorneys in a practice area are full: alert banner "No available capacity for [practice]. [Request expansion → CLO]"

### Payment fails
- Appointment is created in "Pending" status but confirmation is NOT sent
- Appointment detail shows: "⚠ Confirmation held — consultation fee payment pending"
- If fee not collected within 24 hours of booking: appointment auto-cancels and slot reopens
- **[ASSUMPTION]** Auto-cancel threshold configurable per office; default 24 hours

### Required documents missing
- Not a gating factor for scheduling (consultation occurs before documents)
- Consult prep notes any incomplete questionnaire fields

### Deadlines at risk
- Not directly applicable to scheduling screen
- If consult is related to a time-sensitive matter (e.g., probate filing), urgency flag shows on appointment card

### Double-booking prevention
- System enforces: no two appointments in the same attorney slot
- Optimistic locking: if two intake specialists try to book the same slot simultaneously, first commit wins; second sees error "Slot no longer available"

### Client cancels via link
- Slot reopens immediately in the calendar
- Intake specialist receives notification: "Client [name] cancelled appointment for [date/time]"
- If within 24 hours of appointment: flagged as late cancellation in analytics
- **[ASSUMPTION]** Cancellation fee policy is outside system scope for v1; system logs the cancellation but does not charge

### Confirmation delivery failures
- SMS bounce: system retries once after 5 minutes; if still failed, flags in automation log and notifies intake
- Email bounce: flagged in automation log; intake should call client to confirm
- If both channels fail: appointment detail shows "🔴 Unable to confirm — manual outreach required"
