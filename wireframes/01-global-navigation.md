# Screen 1: Global Navigation & Role-Based Home

## Purpose
Serve as the universal entry point and persistent navigation shell for all platform users. Delivers a role-specific landing page ("My Day") with prioritized tasks, urgent alerts, and quick-action shortcuts. Every subsequent screen renders inside this shell.

## Primary Users
All personas: CLO, Managing Partner/CEO, COO, CFO, Intake Specialist, Attorney, Paralegal/Case Manager, Billing/Collections Specialist, Marketing Coordinator

## When This Screen Is Used (Workflow Stage)
Every session, every workflow stage. This is the persistent chrome and the default landing after login.

---

## LAYOUT (TEXTUAL WIREFRAME)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ TOP BAR (persistent, 48px)                                              │
│ [Logo/Firm Mark]  [Global Search ________________🔍]  [+ Quick Action ▾]│
│                          [🔔 Alerts (3)] [📞 UCaaS] [👤 Profile ▾]     │
├────────┬────────────────────────────────────────────────────────────────┤
│ LEFT   │  MAIN CONTENT AREA                                            │
│ NAV    │                                                                │
│ (220px)│  ┌──────────────────────────────────────────────────────────┐  │
│        │  │  GREETING BAR                                            │  │
│ [🏠 Home│  │  "Good morning, [Name]" | [Role badge] | [Office]       │  │
│  ──────]│  │  Last login: Mar 24, 9:02 AM                            │  │
│ [📊 Dash│  └──────────────────────────────────────────────────────────┘  │
│  board] │                                                                │
│ [📥 Int-│  ┌─────────────────────┐  ┌────────────────────────────────┐  │
│  ake]   │  │  ALERT BANNER       │  │  MY WORK TODAY                 │  │
│ [📅 Cal-│  │  (critical items)   │  │  ┌────────────────────────┐    │  │
│  endar] │  │  🔴 2 deadlines at  │  │  │ Priority task 1        │    │  │
│ [📁 Mat-│  │     risk            │  │  │ [Matter] Due: Today    │    │  │
│  ters]  │  │  🟡 1 retainer low  │  │  │ [Dependency: ✅ Ready] │    │  │
│ [💬 Com-│  │  🔴 1 consult fee   │  │  ├────────────────────────┤    │  │
│  ms]    │  │     unpaid          │  │  │ Priority task 2        │    │  │
│ [💰 Bil-│  │                     │  │  │ [Matter] Due: Tomorrow │    │  │
│  ling]  │  │  [View All →]       │  │  │ [Dependency: ⏳ Pend.] │    │  │
│ [📈 Rep-│  └─────────────────────┘  │  ├────────────────────────┤    │  │
│  orts]  │                            │  │ ...more tasks...       │    │  │
│ [📣 Mar-│  ┌─────────────────────┐  │  └────────────────────────┘    │  │
│  keting]│  │  RECENT ACTIVITY    │  │  [Show completed ▾]           │  │
│ [⚙ Set- │  │  Feed of recent     │  └────────────────────────────────┘  │
│  tings] │  │  events relevant    │                                       │
│        │  │  to this user       │  ┌────────────────────────────────┐  │
│ ───────│  │  • Call logged...   │  │  QUICK STATS (role-specific)   │  │
│ [Coll- │  │  • Doc uploaded...  │  │  Varies by persona — see below │  │
│  apsed │  │  • Task completed.. │  └────────────────────────────────┘  │
│  secti-│  └─────────────────────┘                                       │
│  ons   │                                                                │
│  for   │                                                                │
│  unused│                                                                │
│  nav]  │                                                                │
├────────┴────────────────────────────────────────────────────────────────┤
│ BOTTOM STATUS BAR (optional, 32px)                                      │
│ [UCaaS status: Online] [Active call: —] [System status: ✅ Operational] │
└─────────────────────────────────────────────────────────────────────────┘
```

## Header
- Firm logo/mark (links to home)
- Global search bar: searches across clients, matters, contacts, documents, tasks (typeahead with category grouping)
- Quick Action button (+): dropdown with role-filtered actions — New Lead, New Matter, Log Call, Create Task, Meeting Notes
- Notification bell with unread count; opens slide-over notification panel
- UCaaS softphone toggle (opens embedded UCaaS — see Screen 8)
- Profile avatar/dropdown: profile settings, role display, office, logout

## Primary Navigation (Left Sidebar, persistent)
Role-filtered. Items not relevant to the user's role are hidden entirely (not grayed out).

| Nav Item | CLO | CEO | COO | CFO | Intake | Attorney | Paralegal | Billing | Marketing |
|----------|-----|-----|-----|-----|--------|----------|-----------|---------|-----------|
| Home | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Dashboard | ✅ | ✅ | ✅ | ✅ | ✅* | — | — | — | — |
| Intake | ✅ | — | ✅ | — | ✅ | — | — | — | — |
| Calendar | ✅ | — | ✅ | — | ✅ | ✅ | ✅ | — | ✅ |
| Matters | ✅ | — | ✅ | — | — | ✅ | ✅ | ✅ | — |
| Communications | ✅ | — | ✅ | — | ✅ | ✅ | ✅ | ✅ | — |
| Billing | ✅ | — | — | ✅ | — | — | — | ✅ | — |
| Reports | ✅ | ✅ | ✅ | ✅ | — | — | — | ✅ | ✅ |
| Marketing | ✅ | — | — | — | — | — | — | — | ✅ |
| Settings | ✅ | ✅ | ✅ | ✅ | — | — | — | — | — |

*Intake sees Office Dashboard, not Firmwide Dashboard

## Secondary Navigation
None at the shell level. Each screen provides its own tab/sub-nav as needed.

## Main Content Area
The role-based home screen renders here. Divided into:
- **Greeting Bar** (top, full width)
- **Alert Banner** (left column, 40%)
- **My Work Today** (right column, 60%)
- **Recent Activity Feed** (left column, below alerts)
- **Quick Stats** (right column, below tasks)

## Persistent Elements
- Left nav: always visible (collapsible to icons on narrow screens)
- Top bar: always visible
- Notification bell: real-time badge updates
- UCaaS status indicator: bottom bar or top bar
- **[ASSUMPTION]** Bottom status bar shown only when UCaaS is active or system has degraded status

---

## KEY COMPONENTS

### 1. Global Search
- **Description:** Unified search across all platform entities
- **Data displayed:** Typeahead results grouped by category (Clients, Matters, Contacts, Documents, Tasks) — max 5 per category
- **Actions available:** Select result → navigate to entity; press Enter → full search results page
- **[ASSUMPTION]** Search respects RBAC — users only see results they have permission to view

### 2. Quick Action Menu
- **Description:** Role-filtered shortcut to create new entities
- **Data displayed:** Action labels with icons
- **Actions available:**
  - New Lead (Intake, CLO, COO)
  - New Matter (Attorney, Paralegal, CLO)
  - Log Call (Intake, Attorney, Paralegal, Billing)
  - Create Task (All staff roles)
  - Meeting Notes (Attorney, Paralegal, CLO)

### 3. Alert Banner
- **Description:** Prioritized list of items requiring attention, sorted by severity (red → yellow → blue)
- **Data displayed:**
  - Alert type icon and color
  - Brief description with linked entity
  - Time remaining (for deadline alerts)
- **Actions available:** Click alert → navigate to relevant screen; Dismiss (for informational alerts only; compliance alerts cannot be dismissed)
- **Alert types per FSD:**
  - 🔴 Deadline at risk (statutory or filing)
  - 🔴 Missing required artifacts (engagement letter, ID, etc.)
  - 🟡 AR/retainer issues (low retainer, overdue invoice)
  - 🟡 Responsiveness breach (SLA timer exceeded)
  - 🔵 Informational (new assignment, status change)

### 4. My Work Today
- **Description:** Prioritized task list for the current day, with dependency awareness
- **Data displayed:**
  - Task name
  - Linked matter/client
  - Due date/time
  - Dependency status (Ready ✅, Pending ⏳, Blocked 🚫)
  - Owner (if delegated)
- **Actions available:** Mark complete, Reassign, Snooze (with reason), Open matter workspace
- **Sorting:** System-prioritized: blocked dependencies surface but are visually muted; ready items sort to top; overdue items pinned at top with red indicator

### 5. Recent Activity Feed
- **Description:** Chronological feed of events relevant to the user's matters and team
- **Data displayed:** Event type, actor, entity, timestamp
- **Actions available:** Click → navigate to entity; Filter by type

### 6. Quick Stats (Role-Specific Widgets)
- **CLO:** Open capacity %, matters at risk, revenue forecast vs. target
- **CEO:** Firm revenue MTD, new matters this week, client satisfaction score
- **COO:** Intake SLA compliance %, queue depth, staffing utilization
- **CFO:** Collections rate, AR aging summary, trust balance total
- **Intake:** Leads today, consults scheduled, conversion rate (trailing 7d)
- **Attorney:** Active matters, hours logged today, next consult time
- **Paralegal:** Open tasks, approaching deadlines (7d), documents pending
- **Billing:** Invoices pending, AR >90 days, payments received today
- **Marketing:** Active campaigns, leads attributed this month, content pending approval

---

## INTERACTIONS & STATES

### Default State
Full layout as described. Tasks populated, alerts populated, activity feed streaming.

### Empty State
- **My Work Today (no tasks):** "You're all caught up! No tasks due today." with illustration. Show "View upcoming tasks →" link.
- **Alert Banner (no alerts):** Section collapses; green checkmark with "No urgent items" shown as single line.
- **Activity Feed (no events):** "No recent activity." with prompt to log a call or create a task.

### Loading State
- Skeleton loaders for each section (shimmer animation)
- Top bar and left nav render immediately (static chrome)
- Each widget loads independently — no full-page spinner

### Error State
- **Search failure:** "Search is temporarily unavailable. Try again in a moment." inline in search dropdown
- **Widget failure:** Individual widget shows "Unable to load [section]. [Retry]" — other widgets unaffected
- **Full page error:** Centered error message with support contact link; left nav still functional

### Blocked/Gated State
Not applicable to home screen directly. Alerts surface blocked states from other screens (e.g., "Engagement letter missing for [Matter]" links to Matter Workspace).

### Success/Confirmation State
- Task completion: Task row animates out with green checkmark, count updates
- Quick Action creation: Toast notification "Lead created successfully" with [View] link

---

## AUTOMATIONS & AI ASSISTS

### What the system suggests
- **Task prioritization:** AI reorders My Work Today based on deadline proximity, dependency chains, matter value, and risk score. Visual indicator: "🤖 Suggested priority" label on reordered items.
- **Smart search:** Typeahead uses fuzzy matching and recent history to predict intent.

### What requires human approval
- No automated actions on the home screen — this is a read/navigate screen
- Task completion always requires explicit user action

### How suggestions are surfaced visually
- AI-suggested priority order: subtle "AI" chip next to reordered tasks; tooltip explains reasoning
- If the user manually reorders, AI suggestion is suppressed for that session

---

## ROLE-BASED VISIBILITY

### CLO
- Full left nav (all items)
- Quick Stats: capacity utilization, matters at risk, revenue forecast
- Alerts: includes capacity overload warnings, pricing override requests
- Quick Actions: all available

### Attorney
- Left nav: Home, Calendar, Matters, Communications
- Quick Stats: active matters, hours, next consult
- Alerts: deadline risk, missing artifacts for their matters only
- Quick Actions: Log Call, Create Task, Meeting Notes

### Intake Specialist
- Left nav: Home, Dashboard (Office), Intake, Calendar, Communications
- Quick Stats: leads today, consults scheduled, conversion rate
- Alerts: intake SLA breaches, unpaid consultation fees, capacity warnings
- Quick Actions: New Lead, Log Call

### Billing/Collections Specialist
- Left nav: Home, Matters, Communications, Billing, Reports
- Quick Stats: invoices pending, AR aging, payments today
- Alerts: AR/retainer issues, payment failures
- Quick Actions: Log Call, Create Task

### Marketing Coordinator
- Left nav: Home, Calendar, Reports, Marketing
- Quick Stats: campaigns, leads attributed, content queue
- Alerts: content approval pending, campaign deadlines
- Quick Actions: Create Task

### Client
- **Does NOT see this screen.** Clients access the Client Portal (Screens 13–14) which has its own navigation.

---

## EDGE CASES & GUARDRAILS

### Capacity exceeded
- CLO and COO see an orange banner: "⚠ [Office/Attorney] capacity exceeded. [Review capacity →]"
- Intake sees: available appointment slots already removed by the capacity engine — no direct alert needed

### Payment fails
- Not applicable to home screen. Billing alerts surface as yellow alert items linking to Billing Workspace.

### Required documents missing
- Surfaced in Alert Banner: "🔴 Missing [document type] for [Matter]. Engagement blocked." Links to Matter Workspace documents panel.

### Deadlines at risk
- Surfaced in Alert Banner with countdown: "🔴 [Deadline type] for [Matter] due in [X hours]. [Open matter →]"
- If deadline is <24 hours: alert is pinned and cannot be dismissed
- If deadline is past due: alert turns dark red with "OVERDUE" label

### Session timeout
- **[ASSUMPTION]** Session timeout at 30 minutes of inactivity
- Warning modal at 25 minutes: "Your session will expire in 5 minutes. [Stay logged in]"
- On timeout: redirect to login with "Session expired" message; return to same URL after re-authentication

### Multi-office users
- **[ASSUMPTION]** Users assigned to multiple offices see an office selector in the top bar (next to their name)
- Home screen aggregates across all assigned offices; office-specific screens filter by selected office
