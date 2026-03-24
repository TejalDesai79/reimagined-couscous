# Screen 7: Matter Workspace (Core Screen)

## Purpose
The central workspace for managing all aspects of a legal matter: posture tracking, unified timeline, task management, document handling, billing overview, risk monitoring, and communications. This is the single pane of glass for anyone working on a matter.

## Primary Users
Attorney (primary), Paralegal/Case Manager (primary), CLO (oversight), Billing Specialist (billing panel), Intake Specialist (post-engagement handoff)

## When This Screen Is Used (Workflow Stage)
Engaged → In Progress → Client Review → Delivery → Closed. Accessed throughout the entire matter lifecycle after engagement.

---

## LAYOUT (TEXTUAL WIREFRAME)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ [Global Nav Shell — see Screen 1]                                       │
├─────────────────────────────────────────────────────────────────────────┤
│ MATTER HEADER                                                           │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Garcia Estate Plan #2026-EP-0147     [Estate Planning]  [Main St] │  │
│ │ Client: Maria Garcia  │  Attorney: K. Park  │  Paralegal: S. Lee │  │
│ │ Stage: ████████░░░░░░░░░░░░ In Progress (Step 3 of 7)           │  │
│ │ Opened: Mar 24  │  Est. completion: Apr 28  │  Value: $4,200     │  │
│ │                                                                   │  │
│ │ [🔴 1 Risk] [⚠ 2 Alerts]                      [Actions ▾]       │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ [Overview] [Timeline] [Tasks] [Documents] [Billing] [Communications]   │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│ TAB: OVERVIEW (default)                                                 │
│                                                                         │
│ ┌─────────────────────────────────┐ ┌────────────────────────────────┐ │
│ │ POSTURE TRACKER                  │ │ RISK PANEL                     │ │
│ │                                  │ │                                │ │
│ │ Phase        │ Status   │ Owner │ │ 🔴 Missing engagement letter  │ │
│ │ ─────────────────────────────── │ │    Required to proceed past   │ │
│ │ 1. Intake    │ ✅ Done  │ Intake│ │    drafting phase.            │ │
│ │ 2. Consult   │ ✅ Done  │ K.Park│ │    [Upload now →]             │ │
│ │ 3. Scope &   │ ✅ Done  │ K.Park│ │                                │ │
│ │    Price     │          │       │ │ ⚠ Retainer balance: $800     │ │
│ │ 4. Drafting  │ 🔵 Active│ S.Lee │ │   (threshold: $1,000)        │ │
│ │    ├ Will    │ ✅ Done  │       │ │   [Send replenishment req →]  │ │
│ │    ├ Trust   │ 🔵 In Prg│       │ │                                │ │
│ │    └ POA     │ ⬜ Pend. │       │ │ ⚠ Client response pending    │ │
│ │ 5. Review    │ ⬜ Pend. │ K.Park│ │   >48 hrs on doc request      │ │
│ │ 6. Execution │ ⬜ Pend. │ S.Lee │ │   [Send reminder →]           │ │
│ │ 7. Delivery  │ ⬜ Pend. │ S.Lee │ │                                │ │
│ │              │          │       │ │                                │ │
│ │ Next action: Complete trust     │ │                                │ │
│ │   draft (S. Lee, due Mar 28)   │ │                                │ │
│ │ ETA: On track ✅                │ │                                │ │
│ └─────────────────────────────────┘ └────────────────────────────────┘ │
│                                                                         │
│ ┌─────────────────────────────────┐ ┌────────────────────────────────┐ │
│ │ NEXT ACTIONS                     │ │ DOCUMENTS SNAPSHOT             │ │
│ │                                  │ │                                │ │
│ │ 🤖 System-suggested:            │ │ Required    │ Received │ Pend.│ │
│ │ ┌──────────────────────────┐   │ │ ──────────────────────────── │ │
│ │ │ Complete trust instrument │   │ │ Engagement  │ ⬜       │ 🔴  │ │
│ │ │ Owner: S. Lee            │   │ │ Letter      │          │      │ │
│ │ │ Due: Mar 28              │   │ │ Photo ID    │ ✅       │      │ │
│ │ │ Dep: Will draft ✅       │   │ │ Asset list  │ ✅       │      │ │
│ │ │ [Accept] [Modify] [Defer]│   │ │ Deed copies │ ⬜       │ ⏳  │ │
│ │ └──────────────────────────┘   │ │ Ben. forms  │ ⬜       │ ⏳  │ │
│ │                                  │ │                                │ │
│ │ Assigned tasks:                  │ │ Drafts      │ Status         │ │
│ │ ┌──────────────────────────┐   │ │ ──────────────────────────── │ │
│ │ │ Draft POA document       │   │ │ Will        │ ✅ Complete    │ │
│ │ │ Owner: S. Lee            │   │ │ Trust       │ 🔵 Drafting   │ │
│ │ │ Due: Apr 2               │   │ │ POA         │ ⬜ Not started │ │
│ │ │ Dep: Trust draft         │   │ │                                │ │
│ │ │ [Start] [Reassign]       │   │ │ [View all documents →]        │ │
│ │ └──────────────────────────┘   │ │                                │ │
│ │ [View all tasks →]              │ │ Signature: 0/3 executed       │ │
│ └─────────────────────────────────┘ │ Retention: Standard (7 years) │ │
│                                      └────────────────────────────────┘ │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: TIMELINE (unified)                                                 │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Filter: [All ▾] [Calls] [Emails] [Tasks] [Docs] [Billing] [Notes]│  │
│ │                                                                   │  │
│ │ Mar 24, 2:15 PM — 📞 Call with client (K. Park, 12 min)         │  │
│ │   Summary: Discussed trust provisions for minor children.         │  │
│ │   🤖 AI summary — [Edit] [Approve for record]                    │  │
│ │                                                                   │  │
│ │ Mar 24, 10:00 AM — 📄 Document uploaded: Asset inventory        │  │
│ │   By: Client (portal upload)                                      │  │
│ │                                                                   │  │
│ │ Mar 24, 9:30 AM — ✅ Task completed: Draft will                  │  │
│ │   By: S. Lee                                                      │  │
│ │                                                                   │  │
│ │ Mar 23, 4:00 PM — 📧 Email to client: Document request          │  │
│ │   Status: Sent, no response (>48 hrs)                             │  │
│ │                                                                   │  │
│ │ Mar 22, 2:00 PM — 💰 Payment received: $2,000 retainer          │  │
│ │                                                                   │  │
│ │ [Load more...]                                                    │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: BILLING (summary)                                                  │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Fee arrangement: Fixed fee — $4,200                               │  │
│ │ Consult fee: $250 (credited to engagement) ✅                     │  │
│ │ Retainer: $2,000 collected │ Balance: $800 │ Threshold: $1,000   │  │
│ │ Invoiced: $0  │  Collected: $2,250  │  Remaining: $1,950         │  │
│ │                                                                   │  │
│ │ [View full billing →] [Generate invoice] [Request retainer top-up]│  │
│ └───────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## KEY COMPONENTS

### 1. Matter Header (Persistent)
- **Description:** Always-visible context bar with matter identity, assignment, stage progress, and risk indicators
- **Data displayed:** Matter number, name, practice area, office, client, attorney, paralegal, stage progress bar (visual), open date, estimated completion, value, risk/alert badges
- **Actions available (dropdown):**
  - Edit matter details
  - Reassign attorney/paralegal
  - Change stage (with gating checks)
  - Close matter (triggers close workflow)
  - Add note
  - Log time entry
- **Stage progress bar:** Visual representation of the practice-specific workflow (e.g., 7 steps for estate planning per FSD). Current step highlighted. Completed steps filled. Blocked steps have lock icon.

### 2. Posture Tracker (Overview Tab)
- **Description:** Internal phase-by-phase tracker showing completed steps, current step, next steps, owners, and ETAs per FSD
- **Data displayed:** Phase name, status (Done/Active/Pending/Blocked), owner, sub-tasks within active phase
- **Actions available:** Click phase → expand sub-tasks; Update status; Reassign owner
- **Gating:** Certain phases cannot be entered until prerequisites are met (e.g., cannot enter Execution until all documents in Review status)

### 3. Risk Panel (Overview Tab)
- **Description:** Aggregated risk indicators for this matter per FSD risk types
- **Data displayed:**
  - Missing artifacts (engagement letter, ID, etc.) — with severity
  - Deadline risk (statutory vs. internal, time remaining)
  - AR/retainer risk (balance vs. threshold)
  - Responsiveness risk (client response time exceeding SLA)
- **Actions available:** Each risk item has a contextual action button (Upload, Send reminder, Request payment, etc.)
- **Blocked matters:** If engagement letter is missing after 48 hours in "Engaged" status, matter header shows 🔴 BLOCKED badge and drafting tasks cannot be started

### 4. Next Actions Panel (Overview Tab)
- **Description:** Combined view of system-suggested and manually assigned tasks
- **Data displayed:**
  - AI-suggested next actions with dependency awareness
  - Assigned tasks with owner, due date, dependency status
- **Actions available:** Accept/modify/defer AI suggestions; Start/complete/reassign tasks; Create new task

### 5. Documents Snapshot (Overview Tab)
- **Description:** Quick view of document status — required vs. received, drafts, signatures
- **Data displayed:** Required documents checklist, draft status, signature status, retention policy
- **Actions available:** Upload document, Request from client (via portal), View full documents tab; Click document → preview in slide-over

### 6. Unified Timeline (Timeline Tab)
- **Description:** Chronological feed of ALL events on this matter per FSD: communications, tasks, filings, documents, invoices, payments
- **Data displayed:** Timestamp, event type icon, description, actor, linked entities
- **Actions available:** Filter by type, search within timeline, click event to navigate, add manual note
- **AI summaries:** Call summaries and meeting notes generated by AI shown with 🤖 indicator; require human approval before being added to official record

### 7. Billing Summary (Billing Tab)
- **Description:** Financial overview for the matter
- **Data displayed:** Fee arrangement, retainer status, invoice history, payment history, outstanding balance
- **Actions available:** View full billing workspace (Screen 9), Generate invoice, Request retainer top-up

---

## INTERACTIONS & STATES

### Default State
Overview tab active. All panels populated. Posture tracker reflects current stage.

### Empty State
- **New matter (just engaged):** Posture tracker shows Step 1 active, all others pending. Documents panel shows required checklist with no items received. Timeline shows engagement event.
- **No tasks assigned:** Next Actions shows AI suggestions only with prompt "No tasks assigned yet. [Create task] or accept a suggestion."

### Loading State
Matter header loads first (cached from list navigation). Tab content loads with skeleton shimmer. Timeline loads most recent 20 events, with "Load more" for history.

### Error State
- Timeline load failure: "Unable to load timeline. [Retry]" — other tabs unaffected
- Document upload failure: "Upload failed for [filename]. [Retry] [Try smaller file]"

### Blocked/Gated States
1. **Missing engagement letter:** Red banner below header: "🔴 Engagement letter required. Drafting tasks are blocked until uploaded. [Upload engagement letter]"
2. **Retainer depleted:** Yellow banner: "⚠ Retainer below threshold ($800/$1,000). New work may be paused. [Request replenishment]"
3. **Stage transition blocked:** When clicking "Advance to next phase": modal "Cannot advance — prerequisites not met: [list of missing items]"
4. **Ethical wall:** If matter has conflict restrictions: "⚠ Ethical wall active. [Restricted users] cannot access this matter." — access denied for restricted users with explanation

### Success/Confirmation State
- Task completed: checkmark animation, posture tracker updates if phase advances
- Document uploaded: appears in documents panel with "New" badge, timeline entry created
- Stage advanced: progress bar animation, toast "Matter advanced to [phase]"

---

## AUTOMATIONS & AI ASSISTS

### What the system suggests
- **Next actions:** AI suggests next tasks based on practice-specific workflow templates, current progress, and dependency chains
- **Call/meeting summaries:** AI generates summaries of calls and meetings; displayed with 🤖 indicator
- **Task suggestions post-call:** After a logged call, AI suggests follow-up tasks based on summary content
- **Deadline prediction:** AI estimates completion dates based on historical cycle times for similar matters
- **Document extraction:** AI extracts key data from uploaded documents (names, dates, asset values) for review

### What requires human approval
- Call summaries must be approved before being added to the official matter record ("Approve for record" button)
- Task suggestions must be accepted by attorney or paralegal
- Stage transitions require explicit user action (system suggests but doesn't auto-advance)
- Any content asserting legal conclusions must be labeled "Decision support" and reviewed
- **Client-facing messages** from this screen require approval before sending

### How suggestions are surfaced visually
- 🤖 chip on AI-generated content
- "System-suggested" label in Next Actions panel
- AI suggestions are visually distinct (light blue background) from assigned tasks (white background)
- Tooltip on each suggestion explains the basis (e.g., "Based on estate planning workflow template, Step 4 requires trust drafting after will completion")

---

## ROLE-BASED VISIBILITY

### CLO
- **Full read access** to all matters across the firm
- Can reassign attorneys, override stage transitions
- Sees pricing variance (agreed fee vs. suggested range) in billing tab
- **Editable:** Matter reassignment, stage override, fee arrangement (with audit trail)

### Attorney
- **Full read/write** for matters assigned to them
- Can complete tasks, advance stages, approve AI summaries, log time, update posture
- Cannot modify fee arrangement (CLO function)
- **Editable:** Tasks, stage transitions, notes, time entries, document approvals, AI summary approvals

### Paralegal / Case Manager
- **Full read/write** for matters assigned to them
- Primary operator: manages documents, tasks, deadlines, client communications
- Cannot advance certain stages (attorney approval required for Review → Execution)
- **Editable:** Tasks, documents, communications, notes, deadline tracking

### Billing Specialist
- **Billing tab only** — can view billing summary and generate invoices
- Read-only on all other tabs
- Cannot see attorney work product or AI strategy suggestions
- **Editable:** Invoices, payment records, retainer requests (on Billing tab)

### Intake Specialist
- **Read-only** access to matters they originated (for post-handoff visibility)
- Sees: posture tracker, timeline (limited), documents status
- Cannot edit any matter data post-engagement

### Marketing
- **No access** to individual matters

### Client
- **Does NOT see this screen.** Client sees the simplified Client Portal Status Tracker (Screen 13) which shows a curated subset of posture information.

---

## EDGE CASES & GUARDRAILS

### Capacity exceeded
- Not directly surfaced on Matter Workspace. If attorney is over capacity, their home screen alerts them; matter work is not blocked by capacity.

### Payment fails
- Retainer top-up request declined or failed: billing tab shows "⚠ Retainer replenishment failed. [Retry] [Escalate to Billing]"
- If retainer is at $0: matter header shows "🔴 Retainer depleted. Billing review required." New tasks cannot be created until resolved (can be overridden by attorney with reason logged)

### Required documents missing
- Engagement letter: Most critical. Matter blocked from drafting if missing (enforced visually and functionally)
- Other required docs: Warning indicators but not blocking (paralegal follows up)
- If client hasn't uploaded required docs after 2 reminders: risk panel escalates to 🔴

### Deadlines at risk
- Statutory deadlines: shown with countdown timer in risk panel. If <7 days: 🟡. If <48 hours: 🔴. If overdue: 🔴 OVERDUE with pulsing indicator.
- Internal deadlines: shown with due date; no pulsing but red if past due.
- **[ASSUMPTION]** Statutory deadlines cannot be deleted or marked as non-required by anyone below CLO level. This is a compliance guardrail per FSD.

### Practice-specific posture variations
- **Estate Planning:** Intake → Consult → Scope/Price → Drafting → Review → Execution → Delivery → Close (per FSD)
- **Estate & Trust Administration:** Pathway Decision → Statutory Filings → Asset Collection → Distributions → Close (per FSD)
- **Elder Law:** Benefits/Planning workflows, document collection, deadlines
- **Fiduciary Litigation:** Pleadings, court dates, discovery, ethical walls, posture summaries
- **Tax Planning:** Return workflow, document requests, due dates, deliverables
- Posture tracker adapts template based on matter type. **[ASSUMPTION]** Templates configured in Settings by CLO/COO.

### Ethical walls (Fiduciary Litigation)
- If an ethical wall is configured for a matter: restricted users see "Access denied — ethical wall in effect for this matter" when attempting to navigate to it
- Matter does not appear in their search results or matter list
- **[ASSUMPTION]** Ethical walls configured by CLO or designated compliance officer; audit trail required for wall creation and any access attempts
