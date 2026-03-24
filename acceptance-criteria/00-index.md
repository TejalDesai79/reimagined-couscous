# Acceptance Criteria — Legal Operations Platform

## Document Version
- **Version:** 1.0
- **Date:** 2026-03-24
- **Source:** Wireframes v1.0 (FSD v0.2)
- **Purpose:** Build-ready acceptance criteria for QA, engineering, and product teams

---

## Document Index

| File | Screen | Description |
|------|--------|-------------|
| `AC-01-global-navigation.md` | Screen 1 | Global Navigation & Role-Based Home ("My Day") |
| `AC-02-executive-dashboard.md` | Screen 2 | Firmwide Executive Dashboard (CLO/CEO/C-Suite) |
| `AC-03-office-dashboard.md` | Screen 3 | Office Dashboard (Intake Lead / Ops) |
| `AC-04-intake-console.md` | Screen 4 | Intake & Qualification Console |
| `AC-05-appointment-scheduling.md` | Screen 5 | Appointment Scheduling & Confirmation Flow |
| `AC-06-capacity-control.md` | Screen 6 | Attorney Capacity & Open Appointments Control Panel |
| `AC-07-matter-workspace.md` | Screen 7 | Matter Workspace (Core Screen) |
| `AC-08-ucaas-console.md` | Screen 8 | Embedded UCaaS Console |
| `AC-09-billing-collections.md` | Screen 9 | Billing & Collections Workspace |
| `AC-10-accounting-forecasting.md` | Screen 10 | Accounting & Forecasting Dashboard |
| `AC-11-trust-accounting.md` | Screen 11 | Trust Accounting |
| `AC-12-marketing-calendar.md` | Screen 12 | Marketing & Content Calendar |
| `AC-13-client-status-tracker.md` | Screen 13 | Client Portal — Matter Status Tracker |
| `AC-14-client-portal.md` | Screen 14 | Client Portal — Documents, Messages & Payments |
| `AC-15-cross-cutting.md` | All Screens | Cross-Cutting: Navigation, Alerts, RBAC, Accessibility, AI Disclosure |

---

## How to Read These Documents

Each AC file follows this structure:
- **AC-[NN].[N]** — numbered acceptance criterion
- **Given / Then** — behavior-driven format describing the condition and expected outcome
- **Tables** — used for role-based access matrices and multi-value comparisons

---

## Key Platform-Wide Standards

All acceptance criteria in this document are governed by the following non-negotiable constraints:

- **RBAC enforced everywhere** — users see only what their role permits; items are hidden (not grayed out)
- **AI always labeled** — 🤖 chip on all AI-generated content; strategy suggestions labeled "decision support only"
- **Human-in-the-loop required** — AI suggestions for calls, tasks, content, and pricing require explicit human approval
- **Audit trail on overrides** — all compliance-relevant overrides generate immutable audit entries
- **Compliance blocks are hard** — conflict detected, fee unpaid, trust balance insufficient = hard system blocks with no intake-level bypass
- **WCAG 2.1 AA** — accessibility compliance required across all screens
- **Mobile-first for client portal** — staff screens are desktop-first, responsive; client portal is mobile-first
- **Dark mode not in v1 scope**
- **Max 10 offices per firm in v1**

---

## Assumptions Referenced Throughout

All wireframe assumptions labeled **[ASSUMPTION]** are carried forward into these acceptance criteria. Key global assumptions:

- Responsive web application; desktop-first for staff; mobile-first for client portal
- Dark mode is NOT in scope for v1
- Maximum 10 offices per firm in v1
- Real-time updates via WebSocket for queues, capacity, and alerts
- Timestamps in user's local timezone; office timezone available on hover
- WCAG 2.1 AA accessibility compliance required
- Session timeout: 30 minutes inactivity (5-minute warning at 25 minutes)
- Stale lead threshold: 48 hours (configurable per office)
- Auto-cancel of unconfirmed appointments: 24 hours (configurable per office)
- IVR: per-office main number with branches (New Client, Existing Client, Billing) — no per-user DDI
- No conference calling in v1
- Bank statement import via CSV/OFX upload or direct bank feed
- Trust large transfer alert threshold: $10,000 (configurable)
- Client portal MFA required; session timeout 30 minutes
- File upload: PDF, JPG, PNG; max 25MB per file; max 5 files per batch
- Message SLA default: 1 business day (general); 4 hours (urgent, internally determined)
