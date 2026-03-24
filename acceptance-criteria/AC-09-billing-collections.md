# AC-09: Billing & Collections Workspace

## Feature Overview
Centralized workspace for invoice management, AR tracking, payment plans, automated dunning sequences, and retainer replenishment. Supports the full billing lifecycle with approval-gated automation.

---

## AC-09.1 — Access Control

| Role | Access |
|------|--------|
| CLO | Full read; approves write-offs and fee overrides |
| CFO | Full read/write across all billing functions; primary approver for collections escalation and write-offs |
| Billing/Collections Specialist | Full read/write for assigned matters/offices; cannot approve write-offs or fee overrides |
| Attorney | Read-only billing summary in Matter Workspace; can review own time entries before invoicing |
| Paralegal | Can enter time entries linked to matters; read-only on billing/collections |
| Intake, Marketing | No access |
| Client | Sees invoices and payment options on Client Portal (Screen 14), not this screen |

---

## AC-09.2 — Billing Dashboard Tab (Default)

**Given** the Billing Dashboard loads
**Then** five KPI cards display: Invoiced MTD, Collected MTD, AR Outstanding Total, Collections Rate (%), Retainer Balance Total

**Given** a KPI card is clicked
**Then** a drill-down shows data broken down by office, practice area, or attorney

---

## AC-09.3 — AR Aging Summary

**Given** the AR Aging summary loads
**Then** it shows aging buckets with amounts and percentages:
- Current
- 1–30 days
- 31–60 days
- 61–90 days
- > 90 days

**Given** a user clicks an aging bucket
**Then** a filtered invoice list for that bucket is shown

---

## AC-09.4 — Action Items Panel

**Given** the Action Items panel loads
**Then** it shows prioritized items with severity indicators:
- 🔴 Invoices > 90 days outstanding
- 🟡 Retainers below threshold
- 🟡 Payment plans past due
- ⚠ Dunning sequences awaiting approval
- 🔵 Invoices ready to send

---

## AC-09.5 — Invoice Management Tab

**Given** the Invoices tab is open
**Then** each invoice in the list shows: invoice number, client name, matter, amount, status, and date sent

**Given** invoice statuses are displayed
**Then** they are: Draft, Sent, Partial, Paid, Overdue, Written off

**Given** a user clicks an invoice row
**Then** an invoice detail slide-over opens

**Given** the invoice detail slide-over opens
**Then** it shows: client, matter, attorney, fee arrangement, line items, retainer application suggestion, and total

**Given** available invoice actions are shown (role-appropriate)
**Then** they include: [Edit] [Apply retainer] [Send to client] [Write off ⚠]

**Given** a user clicks "Write off"
**Then** the button shows ⚠ and clicking opens an approval request form (requires CLO approval per FSD)

**Given** a write-off is approved
**Then** AR is adjusted and a toast displays: "Write-off approved by [approver]. AR adjusted."

**Given** the invoice is for a matter without an engagement letter
**Then** a warning displays: "⚠ Matter missing engagement letter. Invoice can be generated but [review recommended]." — the invoice is NOT blocked

**Given** an invoice is sent successfully
**Then** a toast displays: "Invoice INV-[#] sent to [client email]"

**Given** an invoice email delivery fails
**Then** the error displays: "Email delivery failed for [client]. [Retry] [Download PDF to send manually]"

---

## AC-09.6 — Invoice Auto-Generation

**Given** matter milestones are reached
**Then** the system suggests invoices based on the matter's fee arrangement

**Given** an auto-generated invoice is suggested
**Then** it appears as a Draft with a "🤖 Auto-generated" label; the billing specialist reviews before sending

---

## AC-09.7 — Retainer Application

**Given** an invoice is being processed
**Then** the system suggests retainer application amounts automatically

**Given** the user applies the retainer
**Then** the available retainer balance decreases and the invoice updates accordingly

---

## AC-09.8 — Payment Plans Tab

**Given** the Payment Plans tab is open
**Then** each active plan shows: client name, total amount, amount paid, next due date, next installment amount, and status

**Given** payment plan statuses are displayed
**Then** they are: 🟢 Current, 🔴 Late

**Given** a user clicks "+ Create payment plan"
**Then** a plan creation form opens

**Given** a payment plan installment is missed
**Then** a dunning sequence activates for that installment

---

## AC-09.9 — Retainers Tab

**Given** the Retainers tab is open
**Then** it shows per-matter retainer balances, thresholds, and replenishment history

**Given** a retainer balance drops below the configured threshold
**Then** an auto-triggered replenishment request email is sent to the client (automated)

**Given** the replenishment request appears
**Then** it is shown in the Action Items panel

---

## AC-09.10 — Dunning Sequences Tab

**Given** the Dunning tab is open
**Then** each active sequence shows: client name, invoice, amount, current step (N/5), next action, and approval status

**Given** dunning steps are defined
**Then** the sequence follows these steps and approval rules:
1. Friendly reminder (email, auto at 7 days past due) — no approval required
2. Second notice (email, auto at 21 days) — no approval required
3. Final notice (email + call, requires billing specialist approval at 45 days)
4. Collections escalation (requires CFO/CLO approval at 60 days)
5. Write-off review (requires CLO + CFO approval at 90 days)

**Given** steps 1–2 are due
**Then** they execute automatically

**Given** steps 3–5 are due
**Then** they show "Awaiting approval" status and cannot auto-execute

**Given** the user clicks "Approve pending actions"
**Then** the pending dunning step is approved and executes

**Given** a dunning sequence step is awaiting approval
**Then** it is highlighted with an amber badge in the dashboard

---

## AC-09.11 — Trust Accounting Integration

**Given** an invoice draws from a trust account
**Then** it shows a "Trust application" indicator and a link to Screen 11 (Trust Accounting)

**Given** a billing specialist initiates a trust-to-operating transfer via the billing workspace
**Then** it creates a transfer request on Screen 11 — this screen does NOT directly modify trust ledgers

---

## AC-09.12 — Payment Recording

**Given** a payment is recorded
**Then** the invoice status updates and AR totals update in real-time

**Given** a payment recording fails
**Then** the error displays: "Payment recording failed. [Retry]"

**Given** a payment is received
**Then** a toast displays: "Payment recorded. AR updated."

---

## AC-09.13 — Fee Override

**Given** a billing situation requires a fee override
**Then** the override requires CLO approval (per FSD)

---

## AC-09.14 — Empty State

**Given** no billing activity exists for the selected period
**Then** the message displays: "No billing activity for the selected period. [Generate first invoice →]"

---

## AC-09.15 — Loading State

**Given** the workspace is loading
**Then** each panel shows skeleton shimmer; KPI cards shimmer
