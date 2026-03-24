# AC-11: Trust Accounting

## Feature Overview
Trust/IOLTA account management with full fiduciary compliance: per-matter trust ledgers, deposit and disbursement tracking, three-way reconciliation, approval-gated transfers, and immutable audit trail.

---

## AC-11.1 — Access Control

| Feature | CFO | CLO | Billing Specialist | Attorney |
|---------|-----|-----|--------------------|----------|
| Ledger view | Full R/W | Read-only | Read (own matters) | Read (own matters) |
| Record deposit | ✅ | — | ✅ (initiates) | — |
| Request transfer | ✅ | — | ✅ (initiates) | — |
| Approve transfer | ✅ (primary) | ✅ (secondary) | — | — |
| Reconciliation | ✅ | Read-only | — | — |
| Audit trail | Full access | Full access | Limited (own) | — |
| Disbursements | ✅ (approve) | ✅ (approve) | ✅ (initiate) | — |

**Given** any other role (Intake, Paralegal, Marketing, Client)
**Then** they have no access to this screen

---

## AC-11.2 — Summary KPIs

**Given** the Trust Accounting screen loads
**Then** the header KPI cards show: total trust balance (all matters combined), count of matters with active trust, pending approval count, and last reconciliation date

---

## AC-11.3 — Ledger View Tab (Default)

**Given** the Ledger View tab is active
**Then** each matter with a trust balance is listed with: matter number, client name, current trust balance, last activity date, and status (🟢 Active, 🟡 Low balance, ⬜ Zero balance)

**Given** a user clicks a matter row
**Then** the matter trust ledger slide-over opens

**Given** the matter trust ledger slide-over opens
**Then** it shows the chronological ledger with: date, description, debit, credit, and running balance

**Given** a pending transfer exists for the matter
**Then** it shows as a projected entry: "Pending: [description] — ⏳ Awaiting approval"

**Given** available actions in the slide-over
**Then** they are: [Request Transfer], [Record Deposit], [View full history]

---

## AC-11.4 — Transfer Gating (Hard Rule)

**Given** any trust transfer is requested
**Then** it ALWAYS goes to an approval queue — there is no self-approval or bypass

**Given** a disbursement or transfer would result in a negative trust balance
**Then** it is blocked with: "Insufficient trust balance. Available: $[X]. Requested: $[Y]." — this is a hard block with no override

**Given** trust funds are transferred to operating
**Then** they MUST be matched to an invoice or approved disbursement — commingling prevention is system-enforced

---

## AC-11.5 — Pending Approvals Tab

**Given** the Pending Approvals tab is active
**Then** each pending transfer or disbursement shows: matter, client, amount, type (transfer/disbursement), requester name, and request timestamp

**Given** the approver is CFO or CLO and views a pending request
**Then** the available actions are: [Approve], [Deny], [Request more info]

**Given** a transfer is approved
**Then** the trust ledger updates, the operating account reflects the transfer, and a toast displays: "Trust transfer approved. $[amount] moved to operating. Ledger updated."

**Given** a large transfer (> $10,000 by default, configurable) is requested
**Then** an additional confirmation step is triggered before approval is accepted

---

## AC-11.6 — Three-Way Reconciliation Tab

**Given** the Reconciliation tab is active
**Then** it shows the most recent reconciliation comparing three balances:
1. Bank statement balance
2. Sum of client ledger balances
3. Platform trust register

**Given** all three balances match
**Then** the status shows: "✅ RECONCILED — All three balances match" with the reconciler name and date

**Given** the balances do not match
**Then** the status shows: "⚠ DISCREPANCY DETECTED — Balances do not match. Reconciliation cannot be completed until resolved."

**Given** a user starts a new reconciliation
**Then** they can import a bank statement via CSV/OFX upload or direct bank feed

**Given** a reconciliation is completed successfully
**Then** it is recorded with: timestamp and reconciler (CFO) name

**Given** the Export button is clicked
**Then** the reconciliation report is exported for audit purposes

---

## AC-11.7 — Stale Reconciliation Warning

**Given** more than 7 days have elapsed since the last reconciliation
**Then** a banner displays: "⚠ Trust reconciliation overdue. Last reconciled: [date]. [Start reconciliation]"

---

## AC-11.8 — Audit Trail Tab

**Given** the Audit Trail tab is active
**Then** every trust account action is listed with: timestamp, actor, action type, matter, amount, and status

**Given** the audit trail is displayed
**Then** filter by event type and date range are available

**Given** "Export full audit trail" is clicked
**Then** a full audit trail export is triggered

**Given** the audit trail
**Then** it is append-only and immutable — no entry can be modified or deleted by any user

---

## AC-11.9 — Matter Close with Trust Balance

**Given** a user attempts to close a matter that has a trust balance > $0
**Then** the system blocks the close and prompts: "Trust balance of $[X] must be disbursed or returned to client before closing."

---

## AC-11.10 — Error States

**Given** trust data fails to load
**Then** the error message displays: "Unable to load trust data. This is a critical system — [contact administrator immediately]."

---

## AC-11.11 — Empty State

**Given** no trust accounts are active
**Then** the message displays: "No trust accounts active. Trust ledgers are created automatically when retainers are deposited."
