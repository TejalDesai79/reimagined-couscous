# AC-04: Intake & Qualification Console

## Feature Overview
Central workspace for intake specialists to manage the full intake lifecycle: lead capture, conflict checks, underwriting, consultation scheduling, fee collection, and consult brief preparation.

---

## AC-04.1 — Access Control

**Given** a user with role Intake Specialist navigates to Intake Console
**Then** they can view and manage leads for their assigned office only

**Given** a user with role CLO or COO navigates to Intake Console
**Then** they can view all leads across all offices

**Given** a user with role Attorney, Paralegal, Billing, Marketing, or Client
**Then** they have no direct access to the Intake Console

---

## AC-04.2 — Lead List (Left Panel)

**Given** the Intake Console loads
**Then** the lead list displays each lead with: status indicator (color-coded), name, practice area, stage, time in stage, source, and underwriting score

**Given** lead color coding is applied
**Then**:
- 🔵 = New Lead
- 🟢 = Qualified
- 🟡 = Fee Pending / Awaiting Action
- 🔴 = Stale (> 48 hours without progression, configurable per office)

**Given** no lead is selected
**Then** the right panel shows: "Select a lead to view details."

**Given** a user clicks a lead
**Then** the lead detail loads in the right panel

**Given** sorting is applied
**Then** available sort options are: Newest, Oldest, Score, Stage

**Given** filtering is applied
**Then** available filters are: Stage, Practice Area, Source, Score, Assigned Intake Specialist

**Given** no leads match the current filters
**Then** the message displays: "No leads match current filters. [Clear filters] or [Create new lead]"

---

## AC-04.3 — New Lead Creation

**Given** a user clicks "+ New Lead"
**Then** a lead creation form opens

**Given** a new lead is submitted
**Then**:
1. Lead is created and appears in the list
2. Conflict check runs automatically (results within seconds)
3. Underwriting score is calculated automatically
4. Consultation fee requirement is determined automatically

**Given** a duplicate contact is detected (matching name + phone/email)
**Then** a modal displays: "This contact may already exist: [Name, prior matter]. [Merge] [Create new] [Cancel]"

---

## AC-04.4 — Contact Information Panel

**Given** a lead is selected
**Then** the contact information panel shows: name, phone, email, source/channel, referral details, and prior client flag (checked against 12-month window)

**Given** a user with role Intake Specialist or CLO is viewing
**Then** the [Edit] button is visible to update contact information

---

## AC-04.5 — Questionnaire Responses

**Given** a lead is selected
**Then** questionnaire responses are displayed grouped by category

**Given** the questionnaire is incomplete
**Then** the underwriting panel shows: "Insufficient data for scoring. Request client to complete questionnaire. [Send questionnaire link]"

---

## AC-04.6 — Conflict Check

**Given** a lead is created
**Then** a conflict check runs automatically and results are displayed with: status (Clear ✅, Potential conflict ⚠, Conflict found 🔴) and timestamp

**Given** a conflict is found
**Then** the Scheduling section is blocked with: "🔴 Conflict detected — resolve before scheduling"

**Given** the conflict check service is unavailable
**Then** the error message displays: "Conflict check service unavailable. [Retry] You may not schedule until conflict check completes."

**Given** the user clicks "Re-run conflict check"
**Then** the check runs again and results update

**Given** the user is CLO and a conflict exists
**Then** an "Override conflict" action is available (with required reason, audit logged)

---

## AC-04.7 — Underwriting & Scoring Panel

**Given** a lead has sufficient questionnaire data
**Then** the AI-generated underwriting score displays: score tier (High/Medium/Low), numeric value (e.g., 7/10), and positive/negative factors with point values

**Given** the score is displayed
**Then** a 🤖 chip labels it as AI-generated

**Given** strategy suggestions are shown
**Then** they are labeled: "⚠ Decision support only — not legal advice"

**Given** pricing suggestions are shown
**Then** they are labeled: "⚠ Suggested ranges — attorney sets final pricing"

**Given** an intake specialist overrides the underwriting score
**Then** the override is logged in the audit trail with the reason provided

**Given** the underwriting service is unavailable
**Then** the panel shows: "Scoring unavailable. You may proceed manually — enter score override."

---

## AC-04.8 — Consultation Fee Panel

**Given** a lead is displayed
**Then** the fee requirement is determined automatically based on these rules:
- **Fee required:** No professional referral AND not an engaged client within prior 12 months
- **Fee waived:** Professional referral source AND/OR engaged client within 12 months

**Given** a fee is required
**Then** the panel shows: Fee required: YES, reason, amount ($250 or configured), and status

**Given** a fee is waived automatically
**Then** the panel shows: Fee required: NO with the waiver reason

**Given** a user clicks "Collect Payment →"
**Then** a payment modal opens with card/ACH options

**Given** payment is successfully collected
**Then** the fee status updates to "✅ Paid" and the scheduling section unlocks with an animation

**Given** payment fails
**Then** the panel shows: "🔴 Payment failed: [reason]. [Try again] [Use different method]" and scheduling remains locked

**Given** payment fails 3 times
**Then** the message displays: "Multiple payment failures. Contact client directly. [Log call →]"

**Given** all failed payment attempts
**Then** they are logged in the activity timeline

---

## AC-04.9 — Fee Override Request Flow

**Given** an intake specialist clicks "Request Override"
**Then** a modal opens with: "Reason for fee waiver request: [text field]" and Submit button

**Given** the override is submitted
**Then** CLO receives a notification: "Fee waiver request from [Intake] for [Client]. Reason: [text]. [Approve] [Deny]"

**Given** CLO approves the override
**Then** the fee status updates to "Waived (CLO approved)" and the scheduling section unlocks

**Given** CLO denies the override
**Then** the fee status remains "Not Collected" and intake is notified: "Fee waiver denied by CLO. Collect fee to proceed."

---

## AC-04.10 — Schedule Consultation (Gating)

**Given** the fee status is "Not Collected" and fee is required
**Then** the slot selector is LOCKED with the message: "⚠ Payment required before scheduling"

**Given** a conflict is unresolved
**Then** the slot selector is LOCKED with: "Conflict check must be resolved before scheduling"

**Given** payment is collected and conflict is clear
**Then** the slot selector shows only capacity-approved slots from the Attorney Capacity Engine

**Given** no capacity-approved slots exist for the practice area
**Then** the message displays: "No available appointment slots for [practice area]. [Request capacity expansion →]"

**Given** intake cannot override capacity-approved slots under any circumstances
**Then** the system enforces this hard constraint — no bypass exists for intake

**Given** a slot is selected and the user confirms
**Then** the system transitions to the confirmation flow (Screen 5) and shows a toast: "Consultation scheduled"

---

## AC-04.11 — Stage Actions

**Given** a lead is displayed
**Then** the following stage action buttons are available (role-appropriate): [Mark Qualified], [Disqualify], [Hold]

**Given** a lead is marked Qualified
**Then** the stage badge updates and the lead card updates to green (🟢) with a toast: "Lead qualified successfully"

---

## AC-04.12 — Activity Timeline

**Given** a lead is displayed
**Then** the collapsible activity timeline shows chronological audit log entries with: timestamp, event description, and actor

**Given** the timeline is shown
**Then** filter options by event type are available

---

## AC-04.13 — Stale Lead Tracking

**Given** a lead has been in "Lead" or "Qualified" stage beyond the SLA threshold (default 48 hours, configurable per office)
**Then** the lead card shows 🔴 and moves to "Stale" status

**Given** a stale lead is displayed
**Then** an SLA timer shows the time since creation

---

## AC-04.14 — Loading States

**Given** a lead is selected and the detail panel is loading
**Then** skeleton blocks display for each section

**Given** the underwriting score is calculating
**Then** the panel shows: "Calculating score..." with a spinner (AI processing may take 2–3 seconds)
