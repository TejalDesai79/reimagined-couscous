# Screen 11: Trust Accounting Screen

## Purpose
Manage trust/IOLTA accounts with full compliance: per-matter trust ledgers, deposit and disbursement tracking, three-way reconciliation, transfer approvals, and complete audit trail. This screen enforces the strict fiduciary requirements of client trust accounting.

## Primary Users
CFO (primary), Billing/Collections Specialist (operational), CLO (oversight), Attorney (view only for their matters)

## When This Screen Is Used (Workflow Stage)
Throughout matter lifecycle when trust funds are involved. Critical during engagement (retainer deposit), billing (trust-to-operating transfers), and close (trust balance return).

---

## LAYOUT (TEXTUAL WIREFRAME)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ [Global Nav Shell — see Screen 1]                                       │
├─────────────────────────────────────────────────────────────────────────┤
│ TRUST HEADER                                                            │
│ Trust Accounting          [Account: IOLTA Main ▾]           [⟳]       │
│ [Ledger View] [Reconciliation] [Pending Approvals (3)] [Audit Trail]   │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐                   │
│ │TOTAL TRUST│ │MATTERS   │ │PENDING   │ │LAST      │                   │
│ │BALANCE   │ │WITH TRUST│ │APPROVALS │ │RECONCILED│                   │
│ │$312,400  │ │42        │ │3         │ │Mar 22    │                   │
│ └──────────┘ └──────────┘ └──────────┘ └──────────┘                   │
│                                                                         │
│ TAB: LEDGER VIEW (default)                                              │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ [Search matter ▾] [Filter: All ▾]  [+ Record Deposit]            │  │
│ │                                                                   │  │
│ │ Matter       │ Client      │ Balance  │ Last Activity │ Status   │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ EP-0147     │ Garcia, M.  │ $2,000   │ Mar 24       │ 🟢 Active│  │
│ │ EA-0045     │ Davis, R.   │ $15,200  │ Mar 22       │ 🟢 Active│  │
│ │ EL-0072     │ Thompson, J │ $800     │ Mar 20       │ 🟡 Low   │  │
│ │ TX-0089     │ Williams, R │ $0       │ Mar 18       │ ⬜ Zero   │  │
│ │ ...                                                               │  │
│ │ [Click row for matter trust ledger]                               │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ MATTER TRUST LEDGER (slide-over on row click)                           │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Trust Ledger: EP-0147 — Garcia, Maria                             │  │
│ │ Current Balance: $2,000.00                                        │  │
│ │                                                                   │  │
│ │ Date     │ Description              │ Debit   │ Credit  │ Balance│  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ Mar 24   │ Retainer deposit (ACH)   │         │ $2,000  │ $2,000│  │
│ │          │ Ref: REC-20260324-001    │         │         │       │  │
│ │ (future) │ Pending: Invoice INV-0892│ $1,850  │         │ $150  │  │
│ │          │ ⏳ Awaiting approval     │         │         │       │  │
│ │                                                                   │  │
│ │ [Request Transfer] [Record Deposit] [View full history]          │  │
│ │                                                                   │  │
│ │ ⚠ Pending transfer: $1,850 to operating (INV-0892)              │  │
│ │ Requested by: D. Kim (Billing)  │  Mar 24, 11:00 AM             │  │
│ │ [Approve] [Deny] [Request more info]     ← CFO/CLO only         │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: RECONCILIATION                                                     │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Three-Way Reconciliation — As of Mar 22, 2026                     │  │
│ │                                                                   │  │
│ │ Bank statement balance:          $312,400.00                     │  │
│ │ Sum of client ledger balances:   $312,400.00                     │  │
│ │ Platform trust register:         $312,400.00                     │  │
│ │                                                                   │  │
│ │ Status: ✅ RECONCILED — All three balances match                  │  │
│ │ Reconciled by: CFO (D. Wilson)  │  Mar 22, 2026                  │  │
│ │                                                                   │  │
│ │ Outstanding items: None                                           │  │
│ │                                                                   │  │
│ │ [Start new reconciliation] [View history] [Export for audit]     │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: PENDING APPROVALS (3)                                              │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ 1. Transfer: EP-0147 (Garcia) → Operating — $1,850               │  │
│ │    Invoice: INV-0892  │  Requested: D. Kim  │  Mar 24            │  │
│ │    [Approve] [Deny] [Details]                                    │  │
│ │                                                                   │  │
│ │ 2. Transfer: EA-0045 (Davis) → Operating — $4,500                │  │
│ │    Invoice: INV-0889  │  Requested: D. Kim  │  Mar 23            │  │
│ │    [Approve] [Deny] [Details]                                    │  │
│ │                                                                   │  │
│ │ 3. Disbursement: EA-0045 (Davis) → Estate beneficiary — $8,000  │  │
│ │    Distribution per admin plan  │  Requested: K. Park  │  Mar 22 │  │
│ │    [Approve] [Deny] [Details]                                    │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ TAB: AUDIT TRAIL                                                        │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ [Filter: All ▾] [Date range: _________ to _________]             │  │
│ │                                                                   │  │
│ │ Mar 24, 11:00 AM │ Transfer requested │ EP-0147 │ $1,850        │  │
│ │                   │ By: D. Kim (Billing) │ Pending approval      │  │
│ │ Mar 24, 9:08 AM  │ Deposit recorded   │ EP-0147 │ $2,000        │  │
│ │                   │ By: D. Kim (Billing) │ ACH confirmed         │  │
│ │ Mar 22, 4:30 PM  │ Reconciliation     │ All     │ ✅ Matched    │  │
│ │                   │ By: D. Wilson (CFO) │                         │  │
│ │ Mar 22, 2:00 PM  │ Transfer approved  │ EL-0072 │ $1,200        │  │
│ │                   │ By: D. Wilson (CFO) │ To operating            │  │
│ │ ...                                                               │  │
│ │ [Export full audit trail →]                                       │  │
│ └───────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## KEY COMPONENTS

### 1. Trust Summary KPIs
- **Data:** Total trust balance, active trust matters count, pending approvals count, last reconciliation date
- **Actions:** Click KPI for detail

### 2. Matter Trust Ledger
- **Data:** Per-matter chronological ledger (debits, credits, running balance); pending transfers shown as projected
- **Actions:** Record deposit, Request transfer, View history
- **Gating:** Transfers ALWAYS require approval (per FSD)

### 3. Three-Way Reconciliation
- **Data:** Bank statement balance, sum of client ledgers, platform register; matching status; outstanding items
- **Actions:** Start reconciliation (import bank statement), resolve discrepancies, mark reconciled, export for audit
- **[ASSUMPTION]** Bank statement import via CSV/OFX upload or direct bank feed integration

### 4. Pending Approvals Queue
- **Data:** All pending trust transactions awaiting approval
- **Actions:** Approve, Deny (with reason), Request more info
- **Approvers:** CFO (primary), CLO (secondary) per FSD — CLO has limited trust access but can approve transfers

### 5. Audit Trail
- **Data:** Every trust account action with timestamp, actor, action type, matter, amount, status
- **Actions:** Filter, search, export
- **Per FSD:** Audit trail is append-only, immutable

---

## INTERACTIONS & STATES

### Default State
Ledger view showing all matters with trust balances.

### Empty State
"No trust accounts active. Trust ledgers are created automatically when retainers are deposited."

### Error State
"Unable to load trust data. This is a critical system — [contact administrator immediately]."

### Blocked/Gated States
1. **Transfer without approval:** Cannot execute — always goes to approval queue
2. **Disbursement exceeds balance:** "Insufficient trust balance. Available: $[X]. Requested: $[Y]." — blocked
3. **Reconciliation mismatch:** "⚠ DISCREPANCY DETECTED — Balances do not match. Reconciliation cannot be completed until resolved."

### Success State
- Transfer approved: toast "Trust transfer approved. $[amount] moved to operating. Ledger updated."
- Reconciliation complete: "✅ Reconciliation successful" with timestamp and reconciler name

---

## ROLE-BASED VISIBILITY

| Feature | CFO | CLO | Billing Spec. | Attorney |
|---------|-----|-----|---------------|----------|
| Ledger view | Full R/W | Read-only | Read (own matters) | Read (own matters) |
| Record deposit | ✅ | — | ✅ (initiates) | — |
| Request transfer | ✅ | — | ✅ (initiates) | — |
| Approve transfer | ✅ (primary) | ✅ (secondary) | — | — |
| Reconciliation | ✅ | Read-only | — | — |
| Audit trail | Full access | Full access | Limited (own) | — |
| Disbursements | ✅ (approve) | ✅ (approve) | ✅ (initiate) | — |

Per FSD: CLO has limited trust access — no transfer authority (they can approve but not initiate). All other roles: **No access.**

---

## EDGE CASES & GUARDRAILS

- **Negative balance prevention:** System blocks any transfer/disbursement that would result in negative trust balance. Hard block, no override.
- **Commingling prevention:** Trust funds cannot be transferred to operating without matching an invoice or approved disbursement. System enforces purpose linkage.
- **Stale reconciliation:** If >7 days since last reconciliation: banner "⚠ Trust reconciliation overdue. Last reconciled: [date]. [Start reconciliation]"
- **Large transfer alert:** Transfers >$10,000 trigger additional confirmation step — **[ASSUMPTION]** threshold configurable
- **Matter close with trust balance:** Cannot close matter if trust balance >$0. System prompts: "Trust balance of $[X] must be disbursed or returned to client before closing."
