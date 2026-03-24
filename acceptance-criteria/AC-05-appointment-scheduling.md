# AC-05: Appointment Scheduling & Confirmation Flow

## Feature Overview
End-to-end consultation appointment lifecycle management: booking on capacity-approved slots, automated confirmations (SMS + email), reminder sequences, reschedule/cancel handling, and no-show tracking.

---

## AC-05.1 — Access Control

**Given** a user with role Intake Specialist navigates to Appointment Scheduling
**Then** they have full read/write access for their assigned office

**Given** a user with role CLO or COO navigates to Appointment Scheduling
**Then** they have full read access across all offices

**Given** a user with role Attorney navigates to their Calendar
**Then** they see a read-only view of their own schedule (personal calendar view, not this screen directly)

**Given** a user with role Paralegal, Billing, or Marketing
**Then** they have no access to this screen

---

## AC-05.2 — Calendar View (Default)

**Given** the Appointment Scheduling screen loads
**Then** the calendar view displays the current week with attorney rows × day columns

**Given** the calendar is displayed
**Then** each cell shows one of:
- ✅ Confirmed appointment (with client name)
- ⏳ Awaiting confirmation
- 🔴 Fee pending
- 🔲 Open capacity-approved slot
- ░░ Unavailable (OOO, PTO, or Full)

**Given** a user clicks the week navigation arrows (◀ ▶)
**Then** the calendar advances or retreats by one week

**Given** a user applies the attorney filter
**Then** only the selected attorney's rows are shown

---

## AC-05.3 — Booking a Slot

**Given** a user clicks an open 🔲 slot
**Then** a booking modal opens pre-filled with the practice area from the intake record

**Given** an open slot is clicked but the consultation fee has not been collected
**Then** the modal shows: "Consultation fee must be collected before booking. [Collect on Intake Console →]"

**Given** an open slot is clicked but the conflict check is unresolved
**Then** booking is blocked: "Conflict check must be resolved before scheduling."

**Given** a valid slot is selected and the booking is confirmed
**Then**:
1. The calendar updates in real-time
2. A toast displays: "Appointment confirmed. Confirmations sending..."
3. Automated email and SMS confirmations are triggered immediately
4. Reminder schedule is configured per office settings

**Given** two intake specialists try to book the same slot simultaneously
**Then** the first commit wins and the second user sees: "This slot was just booked by another user. Please select a different time."

---

## AC-05.4 — Automated Confirmations (No Approval Required)

**Given** an appointment is confirmed
**Then** the following are sent automatically and logged in the intake/matter timeline:
- Email confirmation (immediately)
- SMS confirmation (immediately, where permitted)
- 24–48 hour reminder (configurable per office)
- Same-day reminder (configurable per office, optional)

**Given** the confirmation message is sent
**Then** it includes: date, time, location or video link, attorney name, purpose/practice area, and a cancellation/reschedule link

---

## AC-05.5 — Appointment Detail Slide-Over

**Given** a user clicks an appointment on the calendar
**Then** a slide-over opens showing:
- Client name, date/time, attorney, practice area, location/video link, duration
- Fee status with payment details (amount, method, timestamp)
- Confirmation and reminder status with delivery timestamps
- Consult prep summary (underwriting score, key concerns, suggested strategies/pricing)

**Given** the appointment detail is shown
**Then** the available actions are: [Reschedule] [Cancel Appointment] [Mark No-Show] [Mark Complete] [View full intake record]

**Given** the consult prep is shown
**Then** it is read-only for intake specialists; attorneys can add notes from their own view

---

## AC-05.6 — Confirmation & Reminder Automation Log

**Given** the automation log is displayed
**Then** it shows each outbound communication with: timestamp, type (email/SMS), recipient, content summary, and delivery status (✅ Sent, ✅ Delivered, ⚠ Bounced, 🔴 Failed)

**Given** a message delivery has failed
**Then** a "Resend" action is available

**Given** an SMS fails
**Then** the system retries once after 5 minutes; if still failed, it is flagged in the log and intake is notified

**Given** an email bounces
**Then** it is flagged in the log with the message: "intake should call client to confirm"

**Given** both SMS and email delivery fail
**Then** the appointment detail shows: "🔴 Unable to confirm — manual outreach required"

---

## AC-05.7 — No-Show Marking

**Given** a user marks an appointment as "No-Show"
**Then** the status updates and the no-show data is recorded for analytics (this action requires explicit user action; it cannot be automated)

---

## AC-05.8 — Reschedule & Cancel

**Given** an intake specialist reschedules an appointment
**Then** the system checks capacity availability and only shows approved slots

**Given** a client cancels via the reschedule link in their confirmation
**Then**:
- The slot reopens immediately in the calendar
- The intake specialist receives a notification: "Client [name] cancelled appointment for [date/time]"

**Given** the cancellation is within 24 hours of the appointment
**Then** it is flagged as a late cancellation in analytics

---

## AC-05.9 — Payment Hold on Unconfirmed Appointments

**Given** an appointment is created but the consultation fee has not been collected
**Then**:
- The appointment shows status: ⏳ Awaiting confirmation
- The appointment detail shows: "⚠ Confirmation held — consultation fee payment pending"
- Confirmation email and SMS are NOT sent

**Given** the consultation fee is not collected within 24 hours of booking
**Then** the appointment auto-cancels and the slot reopens (threshold configurable per office; default 24 hours)

---

## AC-05.10 — Capacity Enforcement

**Given** an attorney has no remaining capacity-approved slots
**Then** their calendar shows ░░ [FULL] — no 🔲 slots are displayed

**Given** ALL attorneys in a practice area are at capacity
**Then** an alert banner displays: "No available capacity for [practice]. [Request expansion → CLO]"

---

## AC-05.11 — No-Show Analytics Tab

**Given** a user navigates to the No-Show Analytics tab
**Then** they see no-show rate broken down by: client type (new/returning), channel (referral/web/ad), practice area, attorney, time of day

**Given** analytics are shown
**Then** trend charts (weekly/monthly) and impact metrics (lost revenue, wasted capacity hours) are displayed

**Given** a user clicks "Export"
**Then** a data export is triggered

---

## AC-05.12 — List View

**Given** a user switches to List View
**Then** all appointments for the selected period display in chronological order with: date/time, attorney, client, practice, fee status, confirmation status

**Given** a user sorts by a column
**Then** the list re-sorts by that column

---

## AC-05.13 — AI Suggestions

**Given** an available slot is shown
**Then** the system highlights "recommended" slots based on attorney expertise match and schedule density

**Given** a client has a high no-show risk pattern
**Then** their appointment cell shows a subtle ⚠ indicator on the calendar

---

## AC-05.14 — Empty States

**Given** no appointments are scheduled for the current week
**Then** the message displays: "No consultations scheduled for this week. Open slots are available — [go to Intake Console to schedule]."

**Given** no open slots are available for the week
**Then** the message displays: "No available capacity this week. [Request capacity expansion →]"

---

## AC-05.15 — Client Reschedule Link

**Given** a client clicks the reschedule link in their confirmation
**Then** they see a simple client-facing form showing available slots for the same attorney/practice area

**Given** no slots are available within 2 weeks
**Then** the client is prompted: "No available slots within 2 weeks. Please call the office."

**Given** a client reschedules
**Then** this is subject to capacity availability — only capacity-approved slots are shown to the client
