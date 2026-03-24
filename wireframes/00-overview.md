# Legal Operations Platform — Wireframes & Interaction Models

## Document Version
- **Version:** 1.0
- **Date:** 2026-03-24
- **Source:** FSD v0.2
- **Purpose:** Build-ready wireframes for engineering, product, and UX teams

## Document Index

| # | Screen | File |
|---|--------|------|
| 1 | Global Navigation & Role-Based Home | `01-global-navigation.md` |
| 2 | Firmwide Executive Dashboard | `02-executive-dashboard.md` |
| 3 | Office Dashboard | `03-office-dashboard.md` |
| 4 | Intake & Qualification Console | `04-intake-console.md` |
| 5 | Appointment Scheduling & Confirmation Flow | `05-appointment-scheduling.md` |
| 6 | Attorney Capacity & Open Appointments Control Panel | `06-capacity-control.md` |
| 7 | Matter Workspace | `07-matter-workspace.md` |
| 8 | Embedded UCaaS Console | `08-ucaas-console.md` |
| 9 | Billing & Collections Workspace | `09-billing-collections.md` |
| 10 | Accounting & Forecasting Dashboard | `10-accounting-forecasting.md` |
| 11 | Trust Accounting Screen | `11-trust-accounting.md` |
| 12 | Marketing & Content Calendar | `12-marketing-calendar.md` |
| 13 | Client Portal – Matter Status Tracker | `13-client-status-tracker.md` |
| 14 | Client Portal – Documents, Messages & Payments | `14-client-portal.md` |
| Nav | Navigation Model | `15-navigation-model.md` |
| UJ | User Journeys | `16-user-journeys.md` |

## Design Principles (Non-Negotiable)

1. **Single pane of glass** — every role sees one unified interface, scoped to their permissions
2. **Compliance-first** — unsafe actions are blocked visually and functionally; no workarounds
3. **Role-aware simplicity** — hide unnecessary complexity; show only what the persona needs
4. **Status clarity** — matter posture understandable in <60 seconds
5. **Automation with human control** — AI suggests, humans approve
6. **Minimize clicks and context switching** — deep linking, persistent context, slide-over panels

## Universal Stage Model (Reference)

```
Lead → Qualified → Consult Scheduled → Consult Completed → Engaged → In Progress → Client Review → Delivery → Closed
```

## Assumptions Log

All assumptions are labeled **[ASSUMPTION]** inline throughout the wireframes. Key global assumptions:

- **[ASSUMPTION]** Responsive web application; mobile-adaptive but desktop-first for staff screens
- **[ASSUMPTION]** Client portal is fully responsive / mobile-first
- **[ASSUMPTION]** Dark mode is NOT in scope for v1
- **[ASSUMPTION]** Maximum of 10 offices per firm in v1 (affects dropdown vs. search patterns)
- **[ASSUMPTION]** Real-time updates via WebSocket for queues, capacity, and alerts
- **[ASSUMPTION]** All timestamps displayed in the user's local timezone with office timezone available on hover
- **[ASSUMPTION]** 508/WCAG 2.1 AA accessibility compliance required
