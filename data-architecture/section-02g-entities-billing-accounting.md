# SECTION 2 — CANONICAL ENTITY LIST (continued)

## Part G: Billing, Collections & Accounting (Cash + Accrual)

---

### Invoice

**Purpose:** A bill issued to a client for legal services. Supports fixed fee, hourly, tiered, and hybrid arrangements. Links to matter, retainer, and payment records.

**Key Attributes:**
- `invoice_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `invoice_number` (string; format: `INV-{sequence}`) — required
- `matter_id` (FK → Matter) — required
- `contact_id` (FK → Contact; billed party) — required
- `office_id` (FK → Office) — required
- `attorney_user_id` (FK → User) — required
- `status` (enum: draft, pending_review, sent, partial_payment, paid, overdue, written_off, cancelled) — required
- `fee_arrangement` (enum: fixed_fee, hourly, tiered, hybrid) — required
- `subtotal` (decimal) — required
- `consultation_fee_credit` (decimal; default 0; amount credited from consult fee) — required
- `retainer_applied` (decimal; default 0) — required
- `total_due` (decimal; computed: subtotal - consultation_fee_credit - retainer_applied) — required
- `total_paid` (decimal; default 0) — required
- `balance_due` (decimal; computed: total_due - total_paid) — required
- `milestone_description` (string) — optional (e.g., "50% milestone — drafting complete")
- `issued_date` (date) — optional
- `due_date` (date) — optional
- `sent_at` (timestamp) — optional
- `paid_at` (timestamp) — optional
- `is_auto_generated` (boolean) — required (default false)
- `trust_application_amount` (decimal; default 0; amount drawn from trust) — optional
- `trust_transfer_request_id` (FK → TrustTransferRequest) — optional
- `dunning_sequence_id` (FK → DunningSequence) — optional
- `write_off_id` (FK → WriteOff) — optional
- `created_by_user_id` (FK → User) — required
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `invoice_id`
**Unique Constraints:** `(firm_entity_id, invoice_number)`

**Relationships:**
- Belongs to one Matter, Contact, Office
- Has many InvoiceLineItems
- Has many Payments
- May link to one TrustTransferRequest (for trust draws)
- May link to one DunningSequence
- May link to one WriteOff

**Lifecycle States:** draft → pending_review → sent → partial_payment → paid | overdue → written_off | cancelled

**Audit Requirements:**
- Log: creation, status changes, send events, payment applications, retainer applications, write-off requests

---

### InvoiceLineItem

**Purpose:** A single line on an invoice describing a service, credit, or adjustment.

**Key Attributes:**
- `line_item_id` (PK, ULID) — required
- `invoice_id` (FK → Invoice) — required
- `description` (string) — required
- `quantity` (decimal; default 1) — required
- `unit_price` (decimal) — required
- `amount` (decimal; computed: quantity × unit_price) — required
- `line_type` (enum: service, credit, adjustment, discount) — required
- `sequence_order` (integer) — required

**Primary Key:** `line_item_id`

---

### Payment

**Purpose:** A payment received from a client against an invoice or as a retainer deposit.

**Key Attributes:**
- `payment_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `contact_id` (FK → Contact) — required
- `invoice_id` (FK → Invoice) — optional (null for retainer deposits)
- `matter_id` (FK → Matter) — optional
- `payment_method_id` (FK → PaymentMethod) — required
- `amount` (decimal) — required
- `status` (enum: pending, processing, completed, failed, refunded) — required
- `payment_type` (enum: invoice_payment, retainer_deposit, consultation_fee, refund) — required
- `reference_number` (string) — optional
- `failure_reason` (string) — optional
- `processed_at` (timestamp) — optional
- `created_at` (timestamp) — required

**Primary Key:** `payment_id`

**Relationships:**
- Belongs to one Contact
- Optionally applied to one Invoice
- Uses one PaymentMethod

**Audit Requirements:**
- Log: creation, status changes, failures, refunds

---

### PaymentMethod

**Purpose:** A stored payment method for a client (card or ACH). PCI-compliant; stores token references only.

**Key Attributes:**
- `method_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `contact_id` (FK → Contact) — required
- `type` (enum: card, ach) — required
- `last_four` (string, 4 chars) — required
- `card_brand` (string; Visa, MC, Amex, etc.) — optional
- `expiry_month` (integer) — optional (cards only)
- `expiry_year` (integer) — optional (cards only)
- `token_reference` (string; payment processor token — NOT raw card data) — required
- `is_default` (boolean) — required
- `is_active` (boolean) — required
- `created_at` (timestamp) — required

**Primary Key:** `method_id`

**Relationships:**
- Belongs to one Contact

**Audit Requirements:**
- Log: creation, removal, default changes — NO raw card data ever logged

---

### PaymentPlan

**Purpose:** A structured installment agreement for paying an outstanding balance over time.

**Key Attributes:**
- `plan_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `contact_id` (FK → Contact) — required
- `matter_id` (FK → Matter) — required
- `invoice_id` (FK → Invoice) — optional
- `total_amount` (decimal) — required
- `total_paid` (decimal; default 0) — required
- `installment_amount` (decimal) — required
- `frequency` (enum: weekly, biweekly, monthly) — required
- `next_due_date` (date) — required
- `installments_remaining` (integer) — required
- `status` (enum: active, completed, past_due, paused, cancelled) — required
- `created_by_user_id` (FK → User) — required
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `plan_id`

**Lifecycle States:** active → completed | past_due → paused → cancelled

**Audit Requirements:**
- Log: creation, payment events, status changes, pause/cancel (with reason)

---

### DunningSequence

**Purpose:** An automated collections sequence for overdue invoices with escalation steps. Steps 1–2 auto-execute; steps 3+ require approval.

**Key Attributes:**
- `sequence_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `invoice_id` (FK → Invoice) — required
- `contact_id` (FK → Contact) — required
- `current_step` (integer; 1–5) — required
- `total_steps` (integer; default 5) — required
- `status` (enum: active, paused, completed, cancelled) — required
- `paused_reason` (text) — optional
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `sequence_id`

**Relationships:**
- Belongs to one Invoice and one Contact
- Has many DunningSteps

---

### DunningStep

**Purpose:** An individual step within a dunning sequence (e.g., friendly reminder, final notice, collections escalation).

**Key Attributes:**
- `step_id` (PK, ULID) — required
- `sequence_id` (FK → DunningSequence) — required
- `step_number` (integer; 1–5) — required
- `name` (string; e.g., "Friendly reminder", "Second notice", "Final notice", "Collections escalation", "Write-off review") — required
- `action_type` (enum: email, email_and_call, collections_referral, write_off_review) — required
- `trigger_days_past_due` (integer; e.g., 7, 21, 45, 60, 90) — required
- `requires_approval` (boolean) — required (steps 1–2 = false; 3–5 = true)
- `approval_role` (enum: billing, cfo, clo) — optional (required when requires_approval = true)
- `status` (enum: pending, approved, executed, skipped, overridden) — required
- `approved_by_user_id` (FK → User) — optional
- `approved_at` (timestamp) — optional
- `executed_at` (timestamp) — optional
- `created_at` (timestamp) — required

**Primary Key:** `step_id`

**Audit Requirements:**
- Log: approval events, execution events, skip events

---

### AccountsReceivableAgingSnapshot

**Purpose:** A point-in-time snapshot of AR aging across the firm, used for dashboard KPIs and trend analysis.

**Key Attributes:**
- `snapshot_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `snapshot_date` (date) — required
- `office_id` (FK → Office) — optional (null = firmwide)
- `current_amount` (decimal) — required
- `days_1_30_amount` (decimal) — required
- `days_31_60_amount` (decimal) — required
- `days_61_90_amount` (decimal) — required
- `days_over_90_amount` (decimal) — required
- `total_ar` (decimal) — required
- `created_at` (timestamp) — required

**Primary Key:** `snapshot_id`

---

### RetainerAccount

**Purpose:** An operating retainer account for a specific matter. Distinct from trust accounts (TrustLedger). Tracks retainer deposits, draws, and balance against a threshold.

**Key Attributes:**
- `retainer_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `matter_id` (FK → Matter) — required
- `contact_id` (FK → Contact) — required
- `account_type` (enum: operating_retainer) — required
- `balance` (decimal) — required (current balance)
- `threshold` (decimal) — required (replenishment trigger threshold)
- `total_deposited` (decimal) — required
- `total_drawn` (decimal) — required
- `is_below_threshold` (boolean) — computed
- `is_depleted` (boolean) — computed (balance = 0)
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `retainer_id`
**Unique Constraints:** `(matter_id, account_type)` — one operating retainer per matter

**Relationships:**
- Belongs to one Matter and one Contact
- Has many RetainerTransactions

**Audit Requirements:**
- Log: deposits, draws, threshold changes, replenishment events

---

### RetainerTransaction

**Purpose:** A single deposit or withdrawal on a retainer account.

**Key Attributes:**
- `transaction_id` (PK, ULID) — required
- `retainer_id` (FK → RetainerAccount) — required
- `type` (enum: deposit, draw, replenishment, refund) — required
- `amount` (decimal) — required
- `description` (string) — required
- `invoice_id` (FK → Invoice) — optional (for draws applied to invoices)
- `payment_id` (FK → Payment) — optional (for deposits)
- `running_balance` (decimal) — required
- `created_by_user_id` (FK → User) — required
- `created_at` (timestamp) — required

**Primary Key:** `transaction_id`

---

### WriteOff

**Purpose:** An approval-gated write-off of uncollectable AR. Requires CLO approval per FSD.

**Key Attributes:**
- `write_off_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `invoice_id` (FK → Invoice) — required
- `amount` (decimal) — required
- `reason` (text) — required
- `requested_by_user_id` (FK → User) — required
- `status` (enum: pending, approved, denied) — required
- `approved_by_user_id` (FK → User) — optional (CLO + CFO jointly per FSD)
- `approved_at` (timestamp) — optional
- `denial_reason` (text) — optional
- `created_at` (timestamp) — required

**Primary Key:** `write_off_id`

**Audit Requirements:**
- Log: creation, approval/denial events — COMPLIANCE GRADE

---

### Budget

**Purpose:** A financial budget for a specific period (monthly/quarterly/annual), used for budget-vs-actual analysis.

**Key Attributes:**
- `budget_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string) — required
- `period_type` (enum: monthly, quarterly, annual) — required
- `period_start` (date) — required
- `period_end` (date) — required
- `status` (enum: draft, approved, active) — required
- `created_by_user_id` (FK → User; CFO) — required
- `created_at` (timestamp) — required

**Primary Key:** `budget_id`

**Relationships:**
- Has many BudgetLines

---

### BudgetLine

**Purpose:** A single line in a budget, representing a revenue or expense category.

**Key Attributes:**
- `line_id` (PK, ULID) — required
- `budget_id` (FK → Budget) — required
- `gl_account_id` (FK → GeneralLedgerAccount) — required
- `office_id` (FK → Office) — optional (null = firmwide)
- `practice_area_id` (FK → PracticeArea) — optional
- `amount` (decimal) — required
- `description` (string) — optional

**Primary Key:** `line_id`

---

### ForecastModel

**Purpose:** Configuration for a financial or capacity forecast. Defines the model type, horizon, and adjustable assumptions.

**Key Attributes:**
- `model_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string) — required
- `type` (enum: revenue, cash, collections, capacity) — required
- `horizon_days` (integer; default 90) — required
- `scenario` (enum: optimistic, base, conservative) — required
- `assumptions` (JSON; key-value pairs: conversion_rate, avg_fee, collections_rate, etc.) — required
- `created_by_user_id` (FK → User) — required
- `is_saved_scenario` (boolean) — required (default false)
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `model_id`

**Relationships:**
- Has many ForecastSnapshots

**Audit Requirements:**
- Log: assumption changes (CFO for financial; CLO for capacity)

---

### ForecastSnapshot

**Purpose:** A point-in-time output from a forecast model, storing the projected values for display on dashboards.

**Key Attributes:**
- `snapshot_id` (PK, ULID) — required
- `model_id` (FK → ForecastModel) — required
- `computed_at` (timestamp) — required
- `data_points` (JSON; array of {date, projected_value, confidence_low, confidence_high}) — required
- `drivers` (JSON; pipeline count, capacity %, collections rate, avg fee) — required

**Primary Key:** `snapshot_id`

---

### GeneralLedgerAccount

**Purpose:** High-level chart of accounts entry for organizing financial data. Supports cash and accrual basis reporting.

**Key Attributes:**
- `account_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `account_number` (string) — required
- `name` (string) — required
- `type` (enum: asset, liability, equity, revenue, expense) — required
- `sub_type` (string) — optional
- `is_trust_account` (boolean) — required (default false; true for IOLTA accounts)
- `is_active` (boolean) — required

**Primary Key:** `account_id`
**Unique Constraints:** `(firm_entity_id, account_number)`

---

### JournalEntry

**Purpose:** A double-entry bookkeeping record for accrual-basis accounting. Links debit and credit entries to GL accounts.

**Key Attributes:**
- `entry_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `entry_date` (date) — required
- `description` (string) — required
- `debit_account_id` (FK → GeneralLedgerAccount) — required
- `credit_account_id` (FK → GeneralLedgerAccount) — required
- `amount` (decimal) — required
- `source_entity_type` (string; "invoice", "payment", "trust_transfer") — optional
- `source_entity_id` (ULID) — optional
- `created_by_user_id` (FK → User) — required
- `is_reversing` (boolean) — required (default false)
- `reversed_entry_id` (FK → JournalEntry) — optional
- `created_at` (timestamp) — required

**Primary Key:** `entry_id`

**Audit Requirements:**
- Log: creation, reversals — journal entries are append-only, not editable
