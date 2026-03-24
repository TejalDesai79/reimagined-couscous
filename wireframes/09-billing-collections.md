# Screen 9: Billing & Collections Workspace

## Purpose
Centralized workspace for managing invoices, accounts receivable, payment plans, automated dunning sequences, and retainer replenishment. Supports the full billing lifecycle from invoice generation through collections, with approval-gated automation.

## Primary Users
Billing/Collections Specialist (primary), CFO (oversight), CLO (pricing governance), Attorney (time entry review), Paralegal (time entry)

## When This Screen Is Used (Workflow Stage)
Throughout engagement — from initial retainer collection through final invoice and matter close. Heavily used during File Completion + Billing/Collections + Close workflow.

---

## LAYOUT (TEXTUAL WIREFRAME)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ [Global Nav Shell — see Screen 1]                                       │
├─────────────────────────────────────────────────────────────────────────┤
│ BILLING HEADER                                                          │
│ Billing & Collections   [Office: All ▾] [Period: This Month ▾] [🔍]   │
│ [Dashboard] [Invoices] [AR Aging] [Payment Plans] [Retainers] [Dunning]│
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│ TAB: DASHBOARD (default)                                                │
│                                                                         │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐     │
│ │INVOICED  │ │COLLECTED │ │AR OUTST. │ │COLLECT % │ │RETAINER  │     │
│ │MTD       │ │MTD       │ │TOTAL     │ │          │ │BALANCE   │     │
│ │$285K     │ │$242K     │ │$184K     │ │91%       │ │$312K     │     │
│ │▲ 5%      │ │▲ 8%      │ │▼ 3%     │ │▲ 2pp    │ │► stable  │     │
│ └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘     │
│                                                                         │
│ ┌──────────────────────────────┐ ┌──────────────────────────────────┐  │
│ │ AR AGING SUMMARY             │ │ ACTION ITEMS                     │  │
│ │                              │ │                                  │  │
│ │ Current    │ $42K   │ 23%   │ │ 🔴 4 invoices >90 days          │  │
│ │ 1–30 days  │ $58K   │ 32%   │ │ 🟡 3 retainers below threshold │  │
│ │ 31–60 days │ $38K   │ 21%   │ │ 🟡 2 payment plans past due    │  │
│ │ 61–90 days │ $22K   │ 12%   │ │ ⚠ 5 dunning sequences paused   │  │
│ │ >90 days   │ $24K   │ 13%   │ │   (awaiting approval)           │  │
│ │            │        │       │ │ 🔵 12 invoices ready to send    │  │
│ │ [View full aging →]         │ │                                  │  │
│ └──────────────────────────────┘ │ [View all →]                    │  │
│                                  └──────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: INVOICES                                                           │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ [+ Generate Invoice]  [Filter: Status ▾] [Practice ▾] [Attorney ▾]│  │
│ │                                                                   │  │
│ │ Invoice # │ Client      │ Matter   │ Amount │ Status  │ Sent    │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ INV-0892 │ Garcia, M.  │ EP-0147 │ $2,100 │ Draft   │ —       │  │
│ │ INV-0891 │ Williams, R │ TX-0089 │ $1,800 │ Sent    │ Mar 20  │  │
│ │ INV-0890 │ Thompson, J │ EL-0072 │ $3,200 │ Partial │ Mar 15  │  │
│ │ INV-0889 │ Davis, R    │ EA-0045 │ $4,500 │ Overdue │ Feb 28  │  │
│ │ INV-0888 │ Roberts, K  │ EP-0138 │ $2,800 │ Paid    │ Mar 10  │  │
│ │                                                                   │  │
│ │ [Click row to open invoice detail]                                │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ INVOICE DETAIL (slide-over)                                             │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ INV-0892 — Garcia, Maria     Status: DRAFT                       │  │
│ │ Matter: #2026-EP-0147 (Estate Planning)                          │  │
│ │ Attorney: K. Park                                                │  │
│ │                                                                   │  │
│ │ Fee arrangement: Fixed fee — $4,200                               │  │
│ │ Consult fee applied: -$250                                       │  │
│ │ This invoice: $2,100 (50% milestone — drafting complete)         │  │
│ │                                                                   │  │
│ │ Line Items:                                                       │  │
│ │ ─────────────────────────────────────────────────────            │  │
│ │ Estate planning services — Phase 1 (Scope + Draft)  │ $2,100    │  │
│ │ Less: Consultation fee credit                       │ ($250)    │  │
│ │                                              Total: │ $1,850    │  │
│ │                                                                   │  │
│ │ Retainer application:                                             │  │
│ │ Available retainer balance: $2,000                               │  │
│ │ Apply from retainer: [$1,850  ] ← auto-suggested                │  │
│ │ Client payment due: $0.00                                        │  │
│ │                                                                   │  │
│ │ [Edit] [Apply retainer] [Send to client] [Write off ⚠]          │  │
│ │                                                                   │  │
│ │ ⚠ Write-offs require CLO approval                                │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: PAYMENT PLANS                                                      │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Active Plans: 8                                                   │  │
│ │                                                                   │  │
│ │ Client      │ Total  │ Paid    │ Next Due  │ Amount │ Status    │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ Thompson, J │ $3,200 │ $1,200  │ Apr 1     │ $400   │ 🟢 Current│  │
│ │ Davis, R    │ $4,500 │ $1,500  │ Mar 15 !! │ $500   │ 🔴 Late  │  │
│ │ ...                                                               │  │
│ │                                                                   │  │
│ │ [+ Create payment plan]                                          │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: DUNNING                                                            │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Active Dunning Sequences: 6                                       │  │
│ │                                                                   │  │
│ │ Client      │ Invoice │ Amount │ Step      │ Next Action │Status │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ Davis, R    │ INV-889 │ $4,500 │ 3/5 Final │ Call (Mar 26│ ⏳    │  │
│ │             │         │        │ Notice    │ )           │ Await │  │
│ │             │         │        │           │             │ Appr. │  │
│ │ Williams, R │ INV-870 │ $1,200 │ 1/5 Rem.  │ Email (Auto)│ 🟢   │  │
│ │ ...                                                               │  │
│ │                                                                   │  │
│ │ Dunning Steps:                                                    │  │
│ │ 1. Friendly reminder (email, auto at 7 days past due)            │  │
│ │ 2. Second notice (email, auto at 21 days)                        │  │
│ │ 3. Final notice (email + call, requires approval at 45 days)     │  │
│ │ 4. Collections escalation (requires approval at 60 days)         │  │
│ │ 5. Write-off review (requires CLO + CFO approval at 90 days)    │  │
│ │                                                                   │  │
│ │ [Configure dunning rules →] [Approve pending actions]            │  │
│ └───────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## KEY COMPONENTS

### 1. Billing Dashboard KPIs
- **Data displayed:** Invoiced MTD, Collected MTD, AR outstanding total, Collection rate %, Retainer balance total
- **Actions:** Click to drill down by office, practice, attorney

### 2. AR Aging Summary
- **Data displayed:** Aging buckets (Current, 1–30, 31–60, 61–90, >90) with amounts and percentages
- **Actions:** Click bucket → filtered invoice list; View full aging report with export

### 3. Invoice Management
- **Data displayed:** Invoice list with #, client, matter, amount, status (Draft/Sent/Partial/Paid/Overdue/Written off), date sent
- **Actions:** Generate invoice, Edit draft, Send to client, Apply retainer, Record payment, Write off (requires approval), Export
- **Invoice generation:** Auto-populated from matter fee arrangement and time entries; billing specialist reviews before sending

### 4. Payment Plans
- **Data displayed:** Active plans with client, total, paid, next due date, amount, status
- **Actions:** Create plan, Edit plan, Record payment, Pause plan, Cancel plan (approval required)
- **[ASSUMPTION]** Payment plans are monthly installments; flexible scheduling

### 5. Retainer Management
- **Data displayed:** Per-matter retainer balances, thresholds, replenishment history
- **Actions:** Request replenishment (auto-email to client), Record retainer receipt, Adjust threshold
- **Per FSD:** Retainer replenishment rules are configurable; escalation when below threshold

### 6. Dunning Sequences
- **Data displayed:** Active sequences with client, invoice, amount, current step, next action, approval status
- **Actions:** Approve next step, Pause sequence, Skip step, Override step, Configure rules
- **Per FSD:** Automated dunning with approvals at escalation points

---

## INTERACTIONS & STATES

### Default State
Dashboard tab showing KPIs and action items. Invoice list populated.

### Empty State
"No billing activity for the selected period. [Generate first invoice →]"

### Loading State
Skeleton shimmer per panel; KPI cards shimmer.

### Error State
- Payment processing failure: "Payment recording failed. [Retry]"
- Invoice send failure: "Email delivery failed for [client]. [Retry] [Download PDF to send manually]"

### Blocked/Gated States
1. **Write-off requires approval:** "Write off" button shows ⚠ icon; click opens approval request form
2. **Invoice for matter without engagement letter:** Warning "⚠ Matter missing engagement letter. Invoice can be generated but [review recommended]."
3. **Dunning step requires approval:** Step shows "Awaiting approval" status; cannot auto-execute

### Success/Confirmation State
- Invoice sent: toast "Invoice INV-[#] sent to [client email]"
- Payment recorded: invoice status updates, AR updates in real-time
- Write-off approved: toast "Write-off approved by [approver]. AR adjusted."

---

## AUTOMATIONS & AI ASSISTS

### What the system suggests
- **Invoice auto-generation:** System suggests invoices based on matter milestones and fee arrangements
- **Retainer replenishment alerts:** Auto-triggered when balance drops below threshold
- **Dunning automation:** Steps 1–2 execute automatically; steps 3–5 require approval
- **Payment allocation:** System suggests which retainer to apply against which invoice

### What requires human approval
- Invoice write-offs (CLO approval per FSD)
- Fee overrides (CLO approval per FSD)
- Dunning escalation (steps 3+: final notice, collections, write-off review)
- Payment plan creation/modification

### How suggestions are surfaced
- Auto-generated invoices appear as "Draft" with "🤖 Auto-generated" label
- Replenishment suggestions appear in Action Items panel
- Dunning steps awaiting approval highlighted with amber badge

---

## ROLE-BASED VISIBILITY

### CLO
- **Full read access** across all offices
- Approves: write-offs, fee overrides, pricing exceptions
- Sees: pricing variance (agreed vs. suggested range from underwriting)
- **Editable:** Write-off approvals, fee override approvals

### CFO
- **Full read/write access** to all billing functions
- Primary approver for collections escalation and write-off reviews (joint with CLO)
- **Editable:** All billing operations, dunning configuration, payment plans

### Billing/Collections Specialist
- **Full read/write** for assigned matters/offices
- Can: generate invoices, record payments, manage payment plans, execute approved dunning steps
- Cannot: approve write-offs or fee overrides
- **Editable:** Invoices, payments, payment plans, dunning execution

### Attorney
- **Read-only** billing summary on Matter Workspace (Screen 7)
- Can review time entries before invoicing
- Cannot generate invoices or manage payments
- **Editable:** Own time entries (pre-invoice only)

### Paralegal
- **Can enter time entries** linked to matters
- Read-only on billing/collections
- **Editable:** Own time entries

### Intake / Marketing
- **No access**

### Client
- **Sees invoices and payment options on Client Portal** (Screen 14), not this screen

---

## EDGE CASES & GUARDRAILS

### Capacity exceeded
Not directly applicable. Billing workload is demand-driven.

### Payment fails
- Failed payment: status updates to "Failed" with reason; automatic retry disabled; billing specialist notified
- Client on payment plan misses payment: dunning sequence activates for that installment

### Required documents missing
- If engagement letter missing: invoice shows warning but is not blocked (billing specialist judgment)

### Deadlines at risk
- If matter has billing milestones tied to deadlines: alert shown in billing dashboard action items

### Trust accounting boundary
- Invoices that draw from trust accounts show "Trust application" indicator and link to Trust Accounting screen (Screen 11)
- Trust transfers require separate approval workflow (Screen 11)
- **[ASSUMPTION]** This screen does NOT directly modify trust ledgers — it initiates requests that are fulfilled on Screen 11
