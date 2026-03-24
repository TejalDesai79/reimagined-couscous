# AC-15: Cross-Cutting Acceptance Criteria

## Feature Overview
Platform-wide standards covering navigation patterns, alert flows, notification behavior, accessibility, and technical compliance requirements that apply across all screens.

---

## AC-15.1 — Navigation Patterns

### Primary Navigation

**Given** a user is on any screen and clicks the firm logo
**Then** they are navigated to their Home screen (Screen 1)

**Given** a user clicks any left nav item
**Then** they are navigated directly to that screen

**Given** a user clicks a task in "My Work Today"
**Then** they deep-link to the relevant matter workspace or screen

**Given** a user clicks an alert item
**Then** they deep-link to the relevant screen and entity

### Drill-Down Navigation

**Given** a user clicks an office row in the Firmwide Dashboard comparison table
**Then** they navigate to the Office Dashboard filtered to that office

**Given** a user clicks "Manage capacity →" on the Firmwide Dashboard
**Then** they navigate to Screen 6 (Capacity Control)

**Given** a user clicks a matter number in any screen
**Then** they deep-link to the Matter Workspace for that matter

**Given** a user clicks "View full billing →" in the Matter Workspace
**Then** they navigate to Screen 9 (Billing & Collections) filtered to that matter

**Given** a billing invoice is trust-related
**Then** clicking it navigates to Screen 11 (Trust Accounting)

### Slide-Over Panel Pattern

**Given** a user clicks a list item on any of the following screens
**Then** a right-side slide-over panel (50% width) opens without losing the list context:
- Intake Console (lead detail)
- Matter Workspace (document preview, task detail)
- Billing Workspace (invoice detail)
- Trust Accounting (matter ledger)
- Appointment Scheduling (appointment detail)
- Capacity Control (attorney detail)

**Given** a slide-over is open and the user clicks outside it or the ✕ button
**Then** the slide-over closes without navigating away

**Given** a slide-over has an "Open full view" link
**Then** clicking it navigates to the full screen for that entity

### Modal Pattern

**Given** the system requires a blocking interaction
**Then** a modal (center overlay) is used for:
- Payment collection (Intake Console)
- Confirmation dialogs (capacity override, write-off approval)
- Transfer requests (Trust Accounting)
- File upload (Client Portal)

---

## AC-15.2 — Alert and Risk Interrupt Flow

### Critical Interruptions (Block Workflow)

| Condition | Blocked Action | Resolution |
|-----------|----------------|------------|
| Missing engagement letter (> 48 hrs in Engaged) | Drafting tasks in Matter Workspace | Upload engagement letter |
| Unpaid consultation fee | Scheduling in Intake Console | Collect payment via payment modal |
| Conflict detected | Scheduling in Intake Console | CLO escalation and resolution |
| Trust balance insufficient | Trust transfer in Screen 11 | Deposit additional funds |
| Retainer depleted ($0) | New tasks in Matter Workspace | Billing replenishment request |
| Ethical wall active | Access to matter | N/A — access denied |

### Warning Interruptions (Non-Blocking)

| Condition | Surfaces Where | Display |
|-----------|---------------|---------|
| Deadline < 7 days | Home alerts, Matter Workspace risk panel | 🟡 yellow badge |
| Deadline < 24 hours | Home alerts, Matter Workspace risk panel | 🔴 red badge, pinned, cannot dismiss |
| Deadline overdue | Home alerts, Matter Workspace risk panel | Dark red, pulsing, pinned |
| Retainer below threshold | Home alerts, Matter Workspace, Billing dashboard | 🟡 yellow with action |
| AR > 90 days | Home alerts, Billing dashboard, Exec dashboard | Aging bucket highlight |
| Capacity gap (negative) | Office Dashboard, Firmwide Dashboard, Capacity Control | Banner + gap indicator |
| Intake SLA breach | Office Dashboard, Home intake alerts | 🟡/🔴 badge |
| Client responsiveness > 48 hrs | Matter Workspace risk panel | 🟡 yellow with "Send reminder" |

---

## AC-15.3 — Notification Flow

**Given** a critical event occurs
**Then**:
1. Inline block appears on the relevant screen
2. A banner shows on the relevant screen
3. The notification bell count increments in real-time (WebSocket)
4. An email notification is sent (if configured)

**Given** a warning event occurs
**Then**:
1. Alert appears in the relevant panel
2. The notification bell count increments

**Given** an informational event occurs
**Then**:
1. The notification bell count increments
2. The event appears in the activity feed

**Given** a user clicks the notification bell
**Then** a slide-over notification panel opens

**Given** a user clicks a notification
**Then** they are deep-linked to the relevant screen/entity

**Given** toast notifications are displayed
**Then** they appear in the bottom-right corner and auto-dismiss after 5 seconds

---

## AC-15.4 — Real-Time Updates

**Given** screens with live data (queues, capacity, notifications, alerts)
**Then** they use WebSocket connections to update in real-time without page refresh

**Given** a WebSocket connection drops
**Then** the system attempts to reconnect automatically

---

## AC-15.5 — Role-Based Access (RBAC) — General Principles

**Given** a user navigates to a screen they do not have access to
**Then** they see: "You don't have access to [screen]. Contact your administrator." — NOT a 404 error

**Given** nav items are hidden for a role
**Then** they are completely hidden — NOT grayed out or disabled

**Given** a user has partial access to a screen (e.g., Billing Specialist on Matter Workspace)
**Then** only the permitted tabs/sections are visible; other tabs are hidden

**Given** any search result
**Then** it respects RBAC — users only see entities they are permitted to access

---

## AC-15.6 — Accessibility (WCAG 2.1 AA)

**Given** any screen in the platform
**Then** it meets 508/WCAG 2.1 AA accessibility standards, including:
- All interactive elements are keyboard navigable
- All images and icons have appropriate alt text or ARIA labels
- Color is not the sole means of conveying information (status indicators also use icons/text)
- Contrast ratios meet AA standards
- Focus states are clearly visible

---

## AC-15.7 — Responsive Design

**Given** staff screens are accessed on a narrow viewport
**Then** the left navigation collapses to icon-only mode

**Given** the platform is classified as responsive
**Then** it is desktop-first for staff screens; mobile-adaptive but NOT mobile-first

**Given** the client portal is accessed
**Then** it is fully responsive and mobile-first

**Given** the client portal is on a mobile device
**Then** the status tracker converts to a vertical timeline layout

---

## AC-15.8 — Dark Mode

**Given** dark mode is requested
**Then** dark mode is NOT in scope for v1 — the platform uses light mode only

---

## AC-15.9 — Timezone Display

**Given** timestamps are displayed throughout the platform
**Then** they display in the user's local timezone

**Given** a user hovers over a timestamp
**Then** the office timezone equivalent is shown as a tooltip

---

## AC-15.10 — Multi-Office Support

**Given** the platform is configured for multiple offices
**Then** a maximum of 10 offices per firm is supported in v1

**Given** a user is assigned to multiple offices
**Then** an office selector appears in the top bar

**Given** office-specific screens are viewed
**Then** they filter to the selected office

---

## AC-15.11 — AI Disclosure Standards

**Given** any AI-generated content is displayed
**Then** it is labeled with a 🤖 chip

**Given** AI strategy suggestions are shown
**Then** they are labeled: "⚠ Decision support only — not legal advice"

**Given** AI pricing suggestions are shown
**Then** they are labeled: "⚠ Suggested ranges — attorney sets final pricing"

**Given** AI-generated call summaries are shown
**Then** they require human approval before being added to the official matter record

**Given** AI task suggestions are shown
**Then** they require explicit human action to create (no auto-creation)

**Given** AI reorders tasks
**Then** a visible "🤖 Suggested priority" label shows with a tooltip explaining the reasoning

**Given** a user manually overrides AI ordering
**Then** AI suggestions are suppressed for that session

---

## AC-15.12 — Audit Trail Standards

**Given** any override, approval, denial, or configuration change is made
**Then** an audit log entry is created with: timestamp, actor, action, previous value, new value, and reason (if applicable)

**Given** any audit trail
**Then** it is append-only and immutable — no entry can be modified or deleted by any user

---

## AC-15.13 — Universal Stage Model

**Given** stages are referenced throughout the platform
**Then** the universal stage model is: Lead → Qualified → Consult Scheduled → Consult Completed → Engaged → In Progress → Client Review → Delivery → Closed

**Given** matter posture is displayed internally
**Then** the full stage names are used

**Given** matter progress is shown to clients
**Then** client-friendly labels are used (per Screen 13 milestone mapping)

---

## AC-15.14 — Performance & Data Loading

**Given** any screen loads
**Then** the top bar and left nav render immediately (static chrome); content panels load independently with skeleton shimmer

**Given** individual panels load independently
**Then** a failure in one panel does not prevent other panels from loading

**Given** the client portal loads
**Then** content displays within 2 seconds

---

## AC-15.15 — Design Principles (Non-Negotiable)

All screens must comply with the following platform design principles:

1. **Single pane of glass** — every role sees one unified interface, scoped to their permissions
2. **Compliance-first** — unsafe actions are blocked visually and functionally; no workarounds
3. **Role-aware simplicity** — only show what the persona needs; hide unnecessary complexity
4. **Status clarity** — matter posture must be understandable in < 60 seconds
5. **Automation with human control** — AI suggests, humans approve
6. **Minimize clicks and context switching** — deep linking, persistent context, slide-over panels
