# AC-01: Global Navigation & Role-Based Home

## Feature Overview
Persistent navigation shell and role-specific landing page ("My Day") for all platform users.

---

## AC-01.1 — Top Bar (Persistent)

**Given** any authenticated staff user is on any screen
**Then** the top bar (48px) is always visible and contains:
- Firm logo/mark that links to Home
- Global search bar with placeholder text and search icon
- Quick Action (+) button
- Notification bell with unread badge count
- UCaaS softphone toggle
- Profile avatar/dropdown

---

## AC-01.2 — Global Search

**Given** a user types in the global search bar
**Then**:
- Typeahead results appear grouped by category: Clients, Matters, Contacts, Documents, Tasks
- Maximum 5 results per category are shown
- Results respect RBAC — users only see entities they have permission to view
- Pressing Enter navigates to a full search results page
- Selecting a result navigates directly to that entity

**Given** the search service is unavailable
**Then** an inline message displays: "Search is temporarily unavailable. Try again in a moment."

---

## AC-01.3 — Quick Action Menu

**Given** a user clicks the Quick Action (+) button
**Then** a dropdown appears with role-filtered actions:

| Action | Visible to |
|--------|-----------|
| New Lead | Intake, CLO, COO |
| New Matter | Attorney, Paralegal, CLO |
| Log Call | Intake, Attorney, Paralegal, Billing |
| Create Task | All staff roles |
| Meeting Notes | Attorney, Paralegal, CLO |

**Given** a quick action is successfully completed
**Then** a toast notification appears: "[Entity] created successfully" with a [View] link

---

## AC-01.4 — Left Navigation (Role-Filtered)

**Given** any authenticated user
**Then** only nav items relevant to their role are visible (items are hidden, not grayed out):

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

*Intake sees Office Dashboard (Screen 3), not Firmwide Dashboard (Screen 2)

**Given** the user is on a narrow screen
**Then** the left nav collapses to icon-only mode

---

## AC-01.5 — Greeting Bar

**Given** a user lands on the Home screen
**Then** the Greeting Bar displays:
- "Good [morning/afternoon/evening], [First Name]"
- Role badge
- Office name
- Last login timestamp

---

## AC-01.6 — Alert Banner

**Given** there are active alerts for the user
**Then** the Alert Banner displays alerts sorted by severity: 🔴 red → 🟡 yellow → 🔵 blue

**Given** an alert is a compliance alert (deadline at risk, missing artifact)
**Then** the Dismiss button is NOT shown — compliance alerts cannot be dismissed

**Given** an alert is informational
**Then** a Dismiss button is shown

**Given** the user clicks an alert
**Then** they are navigated to the relevant screen/entity

**Given** there are no alerts
**Then** the Alert Banner collapses and shows a single green line: "No urgent items"

---

## AC-01.7 — Deadline Alerts

**Given** a statutory/filing deadline is at risk
**Then** an alert displays: "🔴 [Deadline type] for [Matter] due in [X hours]. [Open matter →]"

**Given** a deadline is < 24 hours away
**Then** the alert is pinned and cannot be dismissed

**Given** a deadline is past due
**Then** the alert displays in dark red with an "OVERDUE" label

---

## AC-01.8 — My Work Today

**Given** a user has tasks due today
**Then** the "My Work Today" panel displays:
- Task name
- Linked matter/client
- Due date/time
- Dependency status: Ready ✅, Pending ⏳, Blocked 🚫
- Owner (if delegated)

**Given** tasks are displayed
**Then** the sort order is:
- Overdue items pinned at top with red indicator
- Ready items sorted above pending/blocked items
- Blocked items visible but visually muted

**Given** a task is completed
**Then** the task row animates out with a green checkmark and the count updates

**Given** no tasks are due today
**Then** the panel shows: "You're all caught up! No tasks due today." with a "View upcoming tasks →" link

---

## AC-01.9 — AI Task Prioritization

**Given** AI has reordered tasks based on deadline proximity, dependency chains, matter value, and risk score
**Then** reordered items show a subtle "🤖 Suggested priority" label with a tooltip explaining the reasoning

**Given** the user manually reorders tasks
**Then** AI suggestions are suppressed for the remainder of that session

---

## AC-01.10 — Recent Activity Feed

**Given** a user views the Home screen
**Then** the Recent Activity Feed shows chronological events relevant to the user's matters and team

**Given** there are no events
**Then** the feed shows: "No recent activity." with a prompt to log a call or create a task

---

## AC-01.11 — Quick Stats (Role-Specific Widgets)

**Given** the user's role
**Then** the Quick Stats section shows the following metrics:

| Role | Metrics |
|------|---------|
| CLO | Open capacity %, matters at risk, revenue forecast vs. target |
| CEO | Firm revenue MTD, new matters this week, client satisfaction score |
| COO | Intake SLA compliance %, queue depth, staffing utilization |
| CFO | Collections rate, AR aging summary, trust balance total |
| Intake | Leads today, consults scheduled, conversion rate (trailing 7d) |
| Attorney | Active matters, hours logged today, next consult time |
| Paralegal | Open tasks, approaching deadlines (7d), documents pending |
| Billing | Invoices pending, AR >90 days, payments received today |
| Marketing | Active campaigns, leads attributed this month, content pending approval |

---

## AC-01.12 — Loading States

**Given** the page is loading
**Then**:
- Top bar and left nav render immediately (static chrome)
- Each widget shows a shimmer skeleton loader independently
- No full-page spinner is displayed

---

## AC-01.13 — Error States

**Given** an individual widget fails to load
**Then** it shows: "Unable to load [section]. [Retry]" without affecting other widgets

**Given** a full-page error occurs
**Then** a centered error message with support contact link is shown; left nav remains functional

---

## AC-01.14 — Session Timeout

**Given** 25 minutes of inactivity have elapsed
**Then** a warning modal displays: "Your session will expire in 5 minutes. [Stay logged in]"

**Given** 30 minutes of inactivity have elapsed
**Then** the user is redirected to login with "Session expired" message

**Given** the user re-authenticates after a session timeout
**Then** they are returned to the URL they were on before the timeout

---

## AC-01.15 — Multi-Office Users

**Given** a user is assigned to multiple offices
**Then** an office selector appears in the top bar next to their name

**Given** the user selects a specific office
**Then** office-specific screens filter to the selected office while the Home screen aggregates all assigned offices

---

## AC-01.16 — Capacity Warnings (CLO / COO)

**Given** an office or attorney's capacity is exceeded
**Then** CLO and COO see an orange banner: "⚠ [Office/Attorney] capacity exceeded. [Review capacity →]"

---

## AC-01.17 — Bottom Status Bar

**Given** UCaaS is active or system status is degraded
**Then** the bottom status bar (32px) is visible showing: UCaaS status, active call indicator, and system status

**Given** UCaaS is inactive and system is operational
**Then** the bottom status bar is hidden

---

## AC-01.18 — Client Access

**Given** the user is a client
**Then** they do NOT see the Global Navigation Shell or Home screen — clients access the Client Portal only (Screens 13–14)
