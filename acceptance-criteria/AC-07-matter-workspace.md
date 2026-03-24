# AC-07: Matter Workspace

## Feature Overview
Central workspace for managing all aspects of a legal matter: posture tracking, unified timeline, task management, document handling, billing overview, risk monitoring, and communications.

---

## AC-07.1 — Access Control

| Role | Access Level |
|------|-------------|
| CLO | Full read access across all matters; can reassign attorneys, override stage transitions |
| Attorney | Full read/write for their assigned matters |
| Paralegal/Case Manager | Full read/write for assigned matters |
| Billing Specialist | Billing tab only; read-only all other tabs |
| Intake Specialist | Read-only for matters they originated (post-handoff) |
| Marketing | No access |
| Client | Does not access this screen; sees Client Portal (Screens 13–14) |

**Given** a matter has an ethical wall configured
**Then** restricted users see: "Access denied — ethical wall in effect for this matter" and the matter does not appear in their search results or matter list

---

## AC-07.2 — Matter Header (Persistent)

**Given** any tab of the Matter Workspace is open
**Then** the matter header is always visible and shows:
- Matter number, name, practice area, office
- Client, attorney, paralegal names
- Stage progress bar (visual, showing current step number and total steps)
- Open date, estimated completion date, matter value
- Risk/alert badges (e.g., "🔴 1 Risk", "⚠ 2 Alerts")

**Given** the Actions dropdown is opened
**Then** available actions are: Edit matter details, Reassign attorney/paralegal, Change stage, Close matter, Add note, Log time entry

**Given** the stage progress bar is displayed
**Then** completed steps are filled, the current step is highlighted, and blocked steps show a lock icon

---

## AC-07.3 — Overview Tab — Posture Tracker

**Given** the Overview tab is active
**Then** the Posture Tracker shows each phase with: phase name, status (Done/Active/Pending/Blocked), owner, and sub-tasks within the active phase

**Given** a phase is clicked
**Then** sub-tasks within that phase expand

**Given** a phase has unmet prerequisites
**Then** it cannot be entered; the phase shows a Blocked status with a lock indicator

**Given** the matter type determines the posture template
**Then** the template adapts:
- Estate Planning: Intake → Consult → Scope/Price → Drafting → Review → Execution → Delivery → Close
- Estate & Trust Administration: Pathway Decision → Statutory Filings → Asset Collection → Distributions → Close
- Other practice types per FSD

**Given** the user is CLO
**Then** they can override stage transitions (audit logged)

---

## AC-07.4 — Overview Tab — Risk Panel

**Given** the risk panel loads
**Then** it shows risk items with actions:
- Missing engagement letter → [Upload now →]
- Retainer below threshold → [Send replenishment req →]
- Client responsiveness > 48 hrs → [Send reminder →]
- Statutory deadline at risk → countdown timer with color coding

**Given** a statutory deadline is < 7 days away
**Then** it shows 🟡 with the number of days remaining

**Given** a statutory deadline is < 48 hours away
**Then** it shows 🔴 with countdown

**Given** a statutory deadline is past due
**Then** it shows 🔴 OVERDUE with a pulsing indicator

**Given** the engagement letter is missing and the matter has been in "Engaged" status > 48 hours
**Then** the matter header shows a 🔴 BLOCKED badge and a red banner: "🔴 Engagement letter required. Drafting tasks are blocked until uploaded. [Upload engagement letter]"

**Given** the engagement letter is missing and the matter is in blocked state
**Then** drafting tasks cannot be started (functionally enforced, not just visual)

**Given** a statutory deadline is present
**Then** only CLO-level or above can delete or mark it as non-required (compliance guardrail)

---

## AC-07.5 — Overview Tab — Next Actions Panel

**Given** the next actions panel loads
**Then** it shows both AI-suggested and manually assigned tasks

**Given** AI-suggested tasks are shown
**Then** they are labeled "System-suggested" with a light blue background and a 🤖 chip; each suggestion has a tooltip explaining the basis

**Given** manually assigned tasks are shown
**Then** they display on a white background

**Given** a user accepts an AI suggestion
**Then** the task is created and assigned

**Given** a user modifies or defers an AI suggestion
**Then** the suggestion updates accordingly

**Given** no tasks are assigned
**Then** the panel shows: "No tasks assigned yet. [Create task] or accept a suggestion."

---

## AC-07.6 — Overview Tab — Documents Snapshot

**Given** the documents snapshot loads
**Then** it shows: required documents checklist (received/pending status), draft status per document, signature status, and retention policy

**Given** a user clicks a document
**Then** a preview opens in a slide-over

**Given** a user clicks "Upload document"
**Then** a file upload modal opens

**Given** a user clicks "Request from client (via portal)"
**Then** a document request is sent to the client via the portal

---

## AC-07.7 — Timeline Tab

**Given** the Timeline tab is active
**Then** the unified timeline shows ALL events in reverse chronological order: communications, tasks, filings, documents, invoices, payments

**Given** each timeline event is displayed
**Then** it shows: timestamp, event type icon, description, and actor

**Given** filter options are used
**Then** available filters are: All, Calls, Emails, Tasks, Docs, Billing, Notes

**Given** an AI-generated call or meeting summary is present in the timeline
**Then** it shows a 🤖 indicator and two action buttons: [Edit] and [Approve for record]

**Given** the user has not yet approved an AI summary
**Then** it is NOT added to the official matter record

**Given** the timeline is initially loaded
**Then** the 20 most recent events are shown with a "Load more" option

---

## AC-07.8 — Billing Tab (Summary)

**Given** the Billing tab is active
**Then** it shows: fee arrangement, retainer status (balance vs. threshold), invoice summary, payment history, and outstanding balance

**Given** the user clicks "View full billing →"
**Then** they are navigated to Screen 9 (Billing & Collections Workspace)

**Given** the user clicks "Generate invoice"
**Then** an invoice generation flow begins

**Given** the retainer balance reaches $0
**Then** the matter header shows: "🔴 Retainer depleted. Billing review required." and new tasks cannot be created (attorney can override with logged reason)

**Given** the retainer is below the threshold (but > $0)
**Then** a yellow banner shows: "⚠ Retainer below threshold ($[balance]/$[threshold]). New work may be paused. [Request replenishment]"

---

## AC-07.9 — AI Assists on Matter Workspace

**Given** the system suggests next actions
**Then** they are based on practice-specific workflow templates, current progress, and dependency chains

**Given** a call is logged
**Then** AI generates a post-call summary and suggests follow-up tasks; both require human approval before saving to the official record

**Given** a document is uploaded
**Then** AI extracts key data (names, dates, asset values) for review; all extractions are labeled for human review

**Given** client-facing messages are initiated from this screen
**Then** they require human approval before sending

---

## AC-07.10 — Stage Transition Gating

**Given** a user attempts to advance the matter stage
**Then** the system checks prerequisites; if unmet, a modal displays: "Cannot advance — prerequisites not met: [list of missing items]"

**Given** a stage transition is successful
**Then** the progress bar animates and a toast displays: "Matter advanced to [phase]"

---

## AC-07.11 — Ethical Wall (Fiduciary Litigation)

**Given** an ethical wall is configured for a matter
**Then** restricted users see "Access denied — ethical wall in effect" and the matter does not appear in their search or matter lists

**Given** an ethical wall is created or modified
**Then** an audit trail entry is required (configured by CLO or designated compliance officer)

**Given** a restricted user attempts to access an ethical-wall matter
**Then** the access attempt is logged in the audit trail

---

## AC-07.12 — Loading & Error States

**Given** the matter workspace is loading
**Then** the matter header loads first (cached), then tab content loads with skeleton shimmer

**Given** the timeline fails to load
**Then** it shows: "Unable to load timeline. [Retry]" without affecting other tabs

**Given** a document upload fails
**Then** the error shows: "Upload failed for [filename]. [Retry] [Try smaller file]"

---

## AC-07.13 — Document Upload Success

**Given** a document is uploaded successfully
**Then** it appears in the documents panel with a "New" badge, and a timeline entry is created automatically

---

## AC-07.14 — Task Completion

**Given** a task is completed
**Then** a checkmark animation plays, and the posture tracker updates if completing the task advances the phase

---

## AC-07.15 — Intake Specialist Post-Handoff Access

**Given** an Intake Specialist who originated the matter views the Matter Workspace
**Then** they see (read-only): posture tracker, timeline (limited view), document status

**Given** the matter is in engagement or later stages
**Then** the Intake Specialist cannot edit any matter data
