# Navigation Model

## Global Navigation Map

### Staff Platform (Internal)

```
PERSISTENT SHELL (always visible)
├── Top Bar
│   ├── Logo → Home
│   ├── Global Search (clients, matters, contacts, documents, tasks)
│   ├── Quick Action (+) → New Lead / New Matter / Log Call / Create Task / Meeting Notes
│   ├── Notifications (🔔) → Slide-over notification panel
│   ├── UCaaS Softphone (📞) → Mini softphone widget / Full UCaaS Console
│   └── Profile (👤) → Settings / Office selector / Logout
│
├── Left Navigation (role-filtered)
│   ├── Home → Screen 1 (Role-Based Home / My Day)
│   ├── Dashboard → Screen 2 (Firmwide) or Screen 3 (Office) based on role
│   ├── Intake → Screen 4 (Intake Console)
│   ├── Calendar → Screen 5 (Appointment Scheduling)
│   ├── Matters → Matter list → Screen 7 (Matter Workspace)
│   ├── Communications → Screen 8 (UCaaS Console)
│   ├── Billing → Screen 9 (Billing & Collections)
│   ├── Reports → Screen 10 (Accounting) / Screen 11 (Trust Accounting)
│   ├── Marketing → Screen 12 (Marketing & Content Calendar)
│   └── Settings → Platform configuration (admin only)
│
└── Bottom Status Bar (conditional)
    ├── UCaaS status (shown when active)
    └── System status (shown when degraded)
```

### Client Portal (External)

```
CLIENT PORTAL SHELL
├── Top Bar
│   ├── Firm Logo
│   ├── Client name
│   ├── Message indicator (unread count)
│   └── Sign Out
│
└── Horizontal Navigation
    ├── My Matters → Screen 13 (Status Tracker)
    ├── Documents → Screen 14 (Documents section)
    ├── Messages → Screen 14 (Messages section)
    ├── Payments → Screen 14 (Payments section)
    └── Resources → Screen 14 (Resources section)
```

---

## How Users Move Between Screens

### Primary Navigation Patterns

| From | To | Trigger | Pattern |
|------|----|---------|---------|
| Any screen | Home | Click logo or Home nav | Direct navigation |
| Home | Any screen | Click left nav item | Direct navigation |
| Home | Matter Workspace | Click task in "My Work Today" | Deep link |
| Home | Relevant screen | Click alert item | Deep link |
| Dashboard (Firmwide) | Office Dashboard | Click office row in comparison table | Drill-down |
| Dashboard (Firmwide) | Capacity Control | Click "Manage capacity →" | Action link |
| Dashboard (Firmwide) | Intake Console | Click "View intake console →" | Action link |
| Office Dashboard | Intake Console | Click "Open intake console →" | Action link |
| Office Dashboard | UCaaS Console | Click "Live call monitor →" | Action link |
| Office Dashboard | Appointment Scheduling | Click today's schedule entry | Deep link |
| Intake Console | Appointment Scheduling | Schedule consultation (successful) | Workflow transition |
| Intake Console | Matter Workspace | Click post-engagement matter | Deep link |
| Appointment Scheduling | Intake Console | Click appointment client | Cross-reference |
| Matter Workspace | Billing Workspace | Click "View full billing →" | Tab / deep link |
| Matter Workspace | UCaaS (softphone) | Click "Call client" | Widget activation |
| Billing Workspace | Trust Accounting | Click trust-related invoice | Cross-reference |
| Billing Workspace | Matter Workspace | Click matter number | Deep link |
| Any screen | Global Search results | Use search bar | Overlay / new view |

### Slide-Over Panel Pattern
Many screens use slide-over panels (right-side drawer, 50% width) for detail views without losing list context:
- Intake Console: lead detail
- Matter Workspace: document preview, task detail
- Billing: invoice detail
- Trust Accounting: matter ledger
- Scheduling: appointment detail
- Capacity Control: attorney detail

**Interaction:** Click list item → slide-over opens. Click outside or ✕ → closes. Click "Open full view" → navigates to full screen.

### Modal Pattern
Used for focused, blocking interactions:
- Payment collection (Intake Console)
- Confirmation dialogs (capacity override, write-off approval)
- Transfer requests (Trust Accounting)
- File upload (Client Portal)

---

## Persistent vs. Contextual Elements

### Persistent (Always Visible)
| Element | Where | Purpose |
|---------|-------|---------|
| Top bar | All staff screens | Identity, search, quick actions, notifications |
| Left nav | All staff screens | Primary navigation |
| UCaaS softphone widget | All staff screens (when active) | Call controls without leaving current screen |
| Client portal top bar | All portal screens | Identity, messages, sign out |
| Client portal nav | All portal screens | Section navigation |

### Contextual (Appears Based on State)
| Element | Trigger | Location |
|---------|---------|----------|
| Bottom status bar | UCaaS active or system degraded | Bottom of staff shell |
| Notification slide-over | Click bell icon | Right side overlay |
| Slide-over detail panels | Click list item | Right side drawer |
| Alert banners | Risk/compliance events | Below header on relevant screens |
| Gating modals | Missing prerequisites | Center overlay |
| Toast notifications | Action completion | Bottom-right corner (auto-dismiss 5s) |
| AI insight chips | AI-generated content present | Inline with relevant data |

---

## Where Alerts and Risks Interrupt Normal Flow

### Critical Interruptions (Block Workflow)
| Alert | Interrupts | Resolution Screen |
|-------|-----------|-------------------|
| Missing engagement letter | Matter Workspace — blocks drafting tasks | Matter Workspace → Document upload |
| Unpaid consultation fee | Intake Console — blocks scheduling | Intake Console → Payment modal |
| Conflict detected | Intake Console — blocks scheduling | Intake Console → CLO escalation |
| Trust balance insufficient | Trust Accounting — blocks transfer | Trust Accounting → deposit required |
| Retainer depleted ($0) | Matter Workspace — blocks new tasks | Billing Workspace → replenishment |
| Ethical wall | Matter Workspace — denies access | N/A — access denied screen shown |

### Warning Interruptions (Surface but Don't Block)
| Alert | Where Surfaced | How |
|-------|---------------|-----|
| Deadline <7 days | Home (alerts), Matter Workspace (risk panel) | Yellow badge, pinned alert |
| Deadline <24 hours | Home (alerts), Matter Workspace (risk panel) | Red badge, pinned alert, cannot dismiss |
| Deadline overdue | Home (alerts), Matter Workspace (risk panel) | Dark red badge, pulsing, pinned |
| Retainer below threshold | Home (alerts), Matter Workspace (risk panel), Billing dashboard | Yellow alert with action |
| AR >90 days | Home (alerts), Billing dashboard, Exec dashboard | Aging bucket highlight |
| Capacity gap (negative) | Office Dashboard, Firmwide Dashboard, Capacity Control | Banner, gap indicator |
| Intake SLA breach | Office Dashboard, Home (intake alerts) | Yellow/red badge |
| Client responsiveness >48hrs | Matter Workspace (risk panel) | Yellow alert with "Send reminder" action |

### Notification Flow
```
Event occurs
  → System evaluates severity and relevance
    → Critical: Inline block + banner on relevant screen + notification bell + email (configurable)
    → Warning: Alert in relevant panels + notification bell
    → Informational: Notification bell + activity feed
  → Notification bell count updates in real-time (WebSocket)
  → Click notification → deep link to relevant screen/entity
```

---

## Navigation by Role (Default Landing + Available Screens)

| Role | Default Landing | Available Screens |
|------|----------------|-------------------|
| CLO | Firmwide Dashboard (2) | All screens (1–12) |
| CEO | Firmwide Dashboard (2) | 1, 2, 10, 12 (limited) |
| COO | Firmwide Dashboard (2) | 1, 2, 3, 4 (read), 5 (read), 6 (read), 8 (read), 9 (read) |
| CFO | Accounting Dashboard (10) | 1, 2 (financial), 9, 10, 11 |
| Intake | Office Dashboard (3) | 1, 3, 4, 5, 8 (queues) |
| Attorney | Home / My Day (1) | 1, 5 (own cal), 7 (own matters), 8 (softphone) |
| Paralegal | Home / My Day (1) | 1, 5 (own cal), 7 (assigned matters), 8 (softphone) |
| Billing | Home / My Day (1) | 1, 7 (billing tab), 8 (billing queue), 9, 11 (limited) |
| Marketing | Home / My Day (1) | 1, 12 |
| Client | Status Tracker (13) | 13, 14 |
