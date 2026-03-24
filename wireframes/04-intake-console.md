# Screen 4: Intake & Qualification Console

## Purpose
Central workspace for intake specialists to manage the full intake lifecycle: capture leads, run conflict checks, underwrite clients, schedule capacity-approved consultations, collect fees, and prepare consult briefs for attorneys. This screen enforces the gating logic that ensures no consultation is booked without proper qualification and (when required) fee payment.

## Primary Users
Intake Specialist (primary), CLO (oversight), COO (oversight)

## When This Screen Is Used (Workflow Stage)
Lead → Qualified → Consult Scheduled → Consult Completed stages of the universal stage model.

---

## LAYOUT (TEXTUAL WIREFRAME)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ [Global Nav Shell — see Screen 1]                                       │
├─────────────────────────────────────────────────────────────────────────┤
│ CONSOLE HEADER                                                          │
│ Intake Console     [Office: Main St ▾]  [+ New Lead]  [🔍 Search]      │
│ [All Leads] [Qualified] [Scheduled] [Fee Pending] [Consult Done]       │
├────────────────────────────┬────────────────────────────────────────────┤
│ LEAD LIST (left, 35%)      │ LEAD DETAIL (right, 65%)                   │
│                            │                                            │
│ Sort: [Newest ▾] Filter:🔽│ ┌────────────────────────────────────────┐ │
│                            │ │ CONTACT INFORMATION                    │ │
│ ┌────────────────────────┐ │ │ Name: Maria Garcia                    │ │
│ │ 🔵 Maria Garcia        │ │ │ Phone: (555) 123-4567                 │ │
│ │ Estate Planning        │ │ │ Email: m.garcia@email.com             │ │
│ │ Lead · 12 min ago      │ │ │ Source: Website form                  │ │
│ │ Source: Website         │ │ │ Referral: None                        │ │
│ │ Score: —               │ │ │ Prior client: No (not in 12 months)   │ │
│ ├────────────────────────┤ │ │ [Edit] [View full CRM record →]       │ │
│ │ 🟢 James Thompson      │ │ └────────────────────────────────────────┘ │
│ │ Elder Law              │ │                                            │
│ │ Qualified · 2 hrs ago  │ │ ┌────────────────────────────────────────┐ │
│ │ Source: Referral-Atty   │ │ │ QUESTIONNAIRE RESPONSES               │ │
│ │ Score: High            │ │ │ (Digital intake form answers)          │ │
│ ├────────────────────────┤ │ │                                        │ │
│ │ 🟡 Robert Davis        │ │ │ Practice area: Estate Planning        │ │
│ │ Estate Admin           │ │ │ Urgency: Moderate                     │ │
│ │ Fee Pending · 1 day    │ │ │ Assets: $500K–$1M                     │ │
│ │ Source: Google Ad       │ │ │ Family: Married, 2 children           │ │
│ │ Score: Medium          │ │ │ Prior estate plan: No                 │ │
│ ├────────────────────────┤ │ │ Specific concerns: Guardianship for   │ │
│ │ 🔴 Sarah Williams      │ │ │   minor children, tax minimization    │ │
│ │ Tax Planning           │ │ │                                        │ │
│ │ Stale · 3 days         │ │ │ [View full questionnaire →]           │ │
│ │ Source: Existing ref    │ │ └────────────────────────────────────────┘ │
│ │ Score: Low             │ │                                            │
│ └────────────────────────┘ │ ┌────────────────────────────────────────┐ │
│                            │ │ CONFLICT CHECK                         │ │
│ Showing 23 leads           │ │ Status: ✅ Clear (auto-checked)        │ │
│ [Load more]                │ │ Checked: Mar 24, 9:15 AM              │ │
│                            │ │ [Re-run conflict check]                │ │
│                            │ └────────────────────────────────────────┘ │
│                            │                                            │
│                            │ ┌────────────────────────────────────────┐ │
│                            │ │ UNDERWRITING & SCORING       🤖 AI    │ │
│                            │ │                                        │ │
│                            │ │ Client Value Score: ● HIGH             │ │
│                            │ │                                        │ │
│                            │ │ Factors:                               │ │
│                            │ │ + Asset range ($500K–$1M)       +3    │ │
│                            │ │ + Family complexity (minors)    +2    │ │
│                            │ │ + Multiple concerns identified  +1    │ │
│                            │ │ - No referral source            -1    │ │
│                            │ │ - No prior engagement           -1    │ │
│                            │ │ = Score: 7/10 → HIGH                  │ │
│                            │ │                                        │ │
│                            │ │ ┌──────────────────────────────────┐   │ │
│                            │ │ │ SUGGESTED STRATEGIES  🤖 AI     │   │ │
│                            │ │ │ ⚠ Decision support only —       │   │ │
│                            │ │ │   not legal advice               │   │ │
│                            │ │ │                                  │   │ │
│                            │ │ │ 1. Comprehensive estate plan     │   │ │
│                            │ │ │    with trust (recommended)     │   │ │
│                            │ │ │ 2. Basic will + guardianship     │   │ │
│                            │ │ │    designation                  │   │ │
│                            │ │ └──────────────────────────────────┘   │ │
│                            │ │                                        │ │
│                            │ │ ┌──────────────────────────────────┐   │ │
│                            │ │ │ SUGGESTED PRICING     🤖 AI     │   │ │
│                            │ │ │ ⚠ Suggested ranges — attorney   │   │ │
│                            │ │ │   sets final pricing             │   │ │
│                            │ │ │                                  │   │ │
│                            │ │ │ Model: Fixed fee (recommended)  │   │ │
│                            │ │ │ Range: $3,500 – $5,500          │   │ │
│                            │ │ │ Alt: Tiered ($2,800 basic /     │   │ │
│                            │ │ │      $5,500 comprehensive)      │   │ │
│                            │ │ │ Basis: Cost-to-serve + capacity │   │ │
│                            │ │ │        + margin targets          │   │ │
│                            │ │ └──────────────────────────────────┘   │ │
│                            │ └────────────────────────────────────────┘ │
│                            │                                            │
│                            │ ┌────────────────────────────────────────┐ │
│                            │ │ CONSULTATION FEE                       │ │
│                            │ │                                        │ │
│                            │ │ Fee required: YES                      │ │
│                            │ │ Reason: No professional referral AND   │ │
│                            │ │   not engaged client in prior 12 mo.  │ │
│                            │ │ Amount: $250.00                        │ │
│                            │ │ Status: ⏳ NOT COLLECTED               │ │
│                            │ │                                        │ │
│                            │ │ [Collect Payment →] [Request Override] │ │
│                            │ └────────────────────────────────────────┘ │
│                            │                                            │
│                            │ ┌────────────────────────────────────────┐ │
│                            │ │ SCHEDULE CONSULTATION                  │ │
│                            │ │                                        │ │
│                            │ │ ⚠ Payment required before scheduling  │ │
│                            │ │                                        │ │
│                            │ │ Practice: Estate Planning              │ │
│                            │ │ Available slots (capacity-approved):   │ │
│                            │ │ ┌──────────────────────────────────┐   │ │
│                            │ │ │ [LOCKED — Collect fee first]     │   │ │
│                            │ │ └──────────────────────────────────┘   │ │
│                            │ │                                        │ │
│                            │ │ [Mark Qualified] [Disqualify] [Hold]  │ │
│                            │ └────────────────────────────────────────┘ │
├────────────────────────────┴────────────────────────────────────────────┤
│ ACTIVITY TIMELINE (collapsible bottom panel, 200px)                     │
│ Mar 24 9:14 AM — Lead created from website form                        │
│ Mar 24 9:15 AM — Conflict check: clear                                 │
│ Mar 24 9:15 AM — Underwriting score calculated: HIGH (7/10)            │
│ Mar 24 9:16 AM — Intake specialist viewed: A. Johnson                  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## KEY COMPONENTS

### 1. Lead List (Left Panel)
- **Description:** Filterable, sortable list of all intake leads for the selected office
- **Data displayed:** Status indicator (color-coded by stage), name, practice area, stage, time in stage, source, underwriting score
- **Actions available:** Click to select (loads detail in right panel); Sort by: newest, oldest, score, stage; Filter by: stage, practice area, source, score, assigned intake specialist
- **Color coding:** 🔵 New Lead, 🟢 Qualified, 🟡 Fee Pending / Awaiting Action, 🔴 Stale (>48hrs without progression)
- **[ASSUMPTION]** "Stale" threshold configurable per office; default 48 hours

### 2. Contact Information
- **Description:** Client contact details and referral/engagement history
- **Data displayed:** Name, phone, email, source/channel, referral details, prior client flag (checked against 12-month window per FSD)
- **Actions available:** Edit contact info, View full CRM record, Flag duplicate
- **Editable by:** Intake Specialist, CLO

### 3. Questionnaire Responses
- **Description:** Answers from digital intake questionnaire
- **Data displayed:** All questionnaire fields grouped by category (practice area, personal info, situation details, urgency indicators)
- **Actions available:** View full questionnaire (expanded view), Add intake notes
- **[ASSUMPTION]** Questionnaire templates vary by practice area; system selects template based on lead's indicated practice area

### 4. Conflict Check
- **Description:** Automated conflict check results
- **Data displayed:** Status (Clear ✅, Potential conflict ⚠, Conflict found 🔴), timestamp, matched records (if any)
- **Actions available:** Re-run conflict check, Override with reason (CLO only), Escalate conflict
- **If conflict found:** Scheduling section is blocked with message "Conflict detected — resolve before scheduling"

### 5. Underwriting & Scoring Panel
- **Description:** AI-generated client value assessment with explainable factors
- **Data displayed:**
  - Score: High (green) / Medium (yellow) / Low (gray) with numeric value
  - Factor breakdown: positive and negative factors with point values
  - Suggested legal strategies (labeled as decision support, not legal advice)
  - Suggested pricing models and fee ranges
- **Actions available:** Accept score, Override score (with reason — audit logged), Expand factor detail
- **Critical labeling:** "⚠ Decision support only — not legal advice" on strategy suggestions; "⚠ Suggested ranges — attorney sets final pricing" on pricing suggestions

### 6. Consultation Fee Panel
- **Description:** Fee requirement determination and payment capture
- **Data displayed:**
  - Fee required: Yes/No with reason
  - Amount (configurable by office and practice area)
  - Payment status: Not Collected, Processing, Paid, Failed, Waived, Credited
- **Actions available:**
  - "Collect Payment →" opens payment modal (card/ACH entry)
  - "Request Override" sends waiver request to CLO (audit logged)
- **Fee required rules per FSD:**
  - Required when: (a) no professional referral OR (b) not engaged client within prior 12 months
  - Waived when: professional referral source AND/OR engaged client within 12 months
  - Override requires CLO approval

### 7. Schedule Consultation
- **Description:** Appointment booking using only capacity-approved slots
- **Data displayed:**
  - Available time slots (only capacity-approved per Attorney Capacity Engine)
  - Attorney name, date, time, duration
  - Location (office address or video link)
- **Actions available:** Select slot, Confirm booking (triggers confirmation flow — Screen 5)
- **Gating:** Slot selector is LOCKED if consultation fee is required but not paid. Shows: "⚠ Payment required before scheduling"
- **[ASSUMPTION]** Slots show attorney name; intake cannot override attorney assignment for capacity-approved slots

### 8. Activity Timeline
- **Description:** Chronological audit log of all actions on this lead
- **Data displayed:** Timestamp, event description, actor
- **Actions available:** Scroll, Filter by event type; Collapsible panel

---

## INTERACTIONS & STATES

### Default State
Lead list populated; no lead selected. Right panel shows: "Select a lead to view details."

### Empty State
- **No leads:** "No leads match current filters. [Clear filters] or [Create new lead]"
- **New office:** "No intake activity yet. Leads will appear here as they are submitted."

### Loading State
- Lead list: skeleton rows
- Detail panel: skeleton blocks for each section
- Underwriting score: "Calculating score..." with spinner (may take 2–3 seconds for AI processing)

### Error State
- Conflict check failure: "Conflict check service unavailable. [Retry] You may not schedule until conflict check completes."
- Payment processing error: "Payment could not be processed. [Try again] [Use different method]" — scheduling remains locked
- Underwriting failure: "Scoring unavailable. You may proceed manually — enter score override."

### Blocked/Gated States
1. **Conflict detected:** Scheduling section blocked. Message: "🔴 Potential conflict detected with [entity]. Resolve before scheduling. [Escalate to CLO]"
2. **Fee unpaid:** Scheduling section shows available slots but they are not selectable. Overlay: "Collect consultation fee to unlock scheduling."
3. **No capacity:** Scheduling section shows: "No available appointment slots for [practice area]. [Request capacity expansion →]"
4. **Questionnaire incomplete:** Underwriting shows: "Insufficient data for scoring. Request client to complete questionnaire. [Send questionnaire link]"

### Success/Confirmation State
- Lead qualified: Stage badge updates, lead moves in list, toast "Lead qualified successfully"
- Payment collected: Fee panel updates to "✅ Paid", scheduling section unlocks with animation
- Appointment scheduled: Redirect to confirmation flow (Screen 5), toast "Consultation scheduled"

---

## AUTOMATIONS & AI ASSISTS

### What the system suggests
- **Underwriting score:** Auto-calculated from intake data; explainable factors shown
- **Legal strategies:** AI suggests 1–3 strategies based on intake answers; clearly labeled as decision support
- **Pricing models/ranges:** AI suggests pricing based on cost-to-serve, capacity, margin targets
- **Conflict check:** Auto-runs on lead creation; results within seconds
- **Consultation fee determination:** Auto-determined based on referral source and engagement history

### What requires human approval
- Conflict override (CLO only)
- Fee waiver/override (CLO only)
- Final scheduling confirmation (intake specialist)
- Underwriting score override (intake specialist, audit logged)
- All strategy and pricing suggestions are advisory — attorney makes final decisions

### How suggestions are surfaced visually
- 🤖 AI chip on panels containing AI-generated content
- "⚠ Decision support only" label on strategy suggestions
- "⚠ Suggested ranges" label on pricing suggestions
- Explainable factors listed with point values (no black-box scores)

---

## ROLE-BASED VISIBILITY

### CLO
- **Full read access** to all leads across all offices
- **Can:** Approve fee overrides and waivers, override conflict checks, adjust underwriting thresholds (via Settings), view all underwriting details
- **Editable:** Fee override approval, conflict override, scoring thresholds (in Settings)

### COO
- **Full read access** to all leads across all offices
- **Cannot:** Override fees or conflicts (those are CLO functions)
- **Editable:** None on this screen

### Intake Specialist
- **Full read/write access** to leads for their assigned office
- **Can:** Create leads, edit contact info, mark qualified/disqualified, collect payments, schedule consultations, override underwriting score (with reason)
- **Cannot:** Override conflicts, waive fees, see other offices' leads
- **Editable:** Contact info, intake notes, stage transitions, score override (with reason)

### Attorney
- **Read-only** access to consultation prep summary for their scheduled consults (shown in Matter Workspace, not this screen)
- **No direct access** to Intake Console

### Paralegal / Billing / Marketing / Client
- **No access** to this screen

---

## EDGE CASES & GUARDRAILS

### Capacity exceeded
- Scheduling section: "No available slots for [practice area] in [office]. Next available: [date]."
- "Request capacity expansion →" button visible to intake
- **[ASSUMPTION]** Intake cannot book outside capacity-approved slots under any circumstances — system enforces this; there is no override for intake

### Payment fails
- Fee panel shows: "🔴 Payment failed: [reason]. [Try again] [Use different method]"
- Scheduling remains locked
- If payment fails 3 times: "Multiple payment failures. Contact client directly. [Log call →]"
- Failed payment attempts logged in timeline

### Required documents missing
- If digital questionnaire is incomplete: scoring shows partial results with "Insufficient data" warning
- System can still proceed to scheduling if conflict is clear and fee is paid, but attorney consult prep will note incomplete intake

### Deadlines at risk
- If a lead has been in "Lead" or "Qualified" stage beyond SLA threshold (configurable): lead card shows 🔴 and moves to "Stale" status
- Intake SLA timer visible on lead card (time since creation)

### Duplicate lead detection
- **[ASSUMPTION]** On lead creation, system checks for duplicate contacts (name + phone/email match)
- If potential duplicate found: modal "This contact may already exist: [Name, prior matter]. [Merge] [Create new] [Cancel]"

### Fee override request flow
- Intake clicks "Request Override" → Modal: "Reason for fee waiver request: [text field]" → Submit
- CLO sees notification: "Fee waiver request from [Intake] for [Client]. Reason: [text]. [Approve] [Deny]"
- Approval: fee status updates to "Waived (CLO approved)", scheduling unlocks
- Denial: fee status remains "Not Collected", intake notified "Fee waiver denied by CLO. Collect fee to proceed."
