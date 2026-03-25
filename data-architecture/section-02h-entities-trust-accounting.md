# SECTION 2 — CANONICAL ENTITY LIST (continued)

## Part H: Trust Accounting (Compliance-First)

---

### TrustLedger

**Purpose:** A per-matter trust account ledger within an IOLTA or trust bank account. Enforces strict fiduciary segregation: each matter has its own running balance. Commingling is prevented at the data and application layer.

**Key Attributes:**
- `ledger_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `matter_id` (FK → Matter) — required
- `contact_id` (FK → Contact) — required
- `bank_account_id` (FK → GeneralLedgerAccount; the IOLTA bank account) — required
- `balance` (decimal) — required (current balance; always ≥ 0)
- `status` (enum: active, zero_balance, closed) — required
- `last_activity_at` (timestamp) — optional
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `ledger_id`
**Unique Constraints:** `(matter_id)` — one trust ledger per matter

**Invariants:**
- `balance` may NEVER go negative. The system must hard-block any transaction that would result in a negative balance. No override exists.
- A matter cannot be closed while `balance > 0`. System enforces this.

**Relationships:**
- Belongs to one Matter and one Contact
- Has many TrustLedgerTransactions
- Has many TrustTransferRequests

**Audit Requirements:**
- Log: creation, every balance change, status transitions — HIGHEST SENSITIVITY. Immutable audit trail.

---

### TrustLedgerTransaction

**Purpose:** An immutable ledger entry recording a deposit, disbursement, or transfer on a trust ledger. Double-entry: every transaction has a matching debit/credit in the trust register.

**Key Attributes:**
- `transaction_id` (PK, ULID) — required
- `ledger_id` (FK → TrustLedger) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `matter_id` (FK → Matter) — required
- `type` (enum: deposit, transfer_to_operating, disbursement_to_beneficiary, disbursement_to_client, refund, interest_allocation, bank_fee) — required
- `debit_amount` (decimal; default 0) — required
- `credit_amount` (decimal; default 0) — required
- `running_balance` (decimal) — required (balance after this transaction)
- `description` (string) — required
- `reference_number` (string; e.g., "REC-20260324-001") — optional
- `payment_method` (enum: check, ach, wire, card) — optional
- `invoice_id` (FK → Invoice) — optional (for transfer_to_operating linked to an invoice)
- `transfer_request_id` (FK → TrustTransferRequest) — optional
- `payee_name` (string) — optional (for disbursements)
- `payee_address` (text) — optional
- `initiated_by_user_id` (FK → User) — required
- `approved_by_user_id` (FK → User) — optional (for transactions requiring approval)
- `approved_at` (timestamp) — optional
- `created_at` (timestamp) — required

**Primary Key:** `transaction_id`

**Invariant:** This entity is APPEND-ONLY. Transactions cannot be edited or deleted. Corrections are made by posting reversing entries.

**Relationships:**
- Belongs to one TrustLedger
- Optionally linked to one Invoice or TrustTransferRequest

**Audit Requirements:**
- Each transaction IS an audit entry. Additionally logged in the global AuditLog with actor, amount, matter, and ledger balance after.

---

### TrustTransferRequest

**Purpose:** An approval-gated request to move funds from a trust ledger to the operating account (for invoice payment) or to a third party (disbursement). Per FSD: ALL trust transfers require approval.

**Key Attributes:**
- `request_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `ledger_id` (FK → TrustLedger) — required
- `matter_id` (FK → Matter) — required
- `type` (enum: transfer_to_operating, disbursement_to_beneficiary, disbursement_to_client, refund) — required
- `amount` (decimal) — required
- `purpose` (text) — required
- `invoice_id` (FK → Invoice) — optional (required for transfer_to_operating)
- `payee_name` (string) — optional (for disbursements)
- `payee_details` (JSON; address, bank info for wire) — optional
- `requested_by_user_id` (FK → User; Billing or Attorney) — required
- `status` (enum: pending, approved, denied, executed, cancelled) — required
- `approved_by_user_id` (FK → User; CFO primary, CLO secondary) — optional
- `approval_notes` (text) — optional
- `denied_reason` (text) — optional
- `approved_at` (timestamp) — optional
- `executed_at` (timestamp) — optional
- `resulting_transaction_id` (FK → TrustLedgerTransaction) — optional
- `requires_additional_confirmation` (boolean) — required (default false; true if amount > large_transfer_threshold)
- `additional_confirmation_by_user_id` (FK → User) — optional
- `created_at` (timestamp) — required

**Primary Key:** `request_id`

**Invariants:**
- Cannot be created if `amount > ledger.balance`. Pre-validated at request time.
- Must be linked to an invoice or approved disbursement purpose. System prevents purposeless transfers (commingling prevention).

**Lifecycle States:** pending → approved → executed | denied | cancelled

**Audit Requirements:**
- Log: creation, approval (with approver identity), denial (with reason), execution, cancellation — HIGHEST SENSITIVITY

---

### TrustReconciliation

**Purpose:** Records a three-way reconciliation between the bank statement, the sum of client ledger balances, and the platform trust register. Per FSD: must be performed regularly.

**Key Attributes:**
- `reconciliation_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `bank_account_id` (FK → GeneralLedgerAccount; IOLTA account) — required
- `reconciliation_date` (date) — required
- `bank_statement_balance` (decimal) — required
- `client_ledger_sum` (decimal) — required
- `platform_register_balance` (decimal) — required
- `status` (enum: in_progress, reconciled, discrepancy_found, resolved) — required
- `discrepancy_amount` (decimal) — optional
- `discrepancy_notes` (text) — optional
- `outstanding_items` (JSON; uncleared checks, pending deposits, etc.) — optional
- `reconciled_by_user_id` (FK → User; CFO) — required
- `reconciled_at` (timestamp) — optional
- `artifact_ids` (FK[] → TrustReconciliationArtifact) — optional
- `created_at` (timestamp) — required

**Primary Key:** `reconciliation_id`

**Lifecycle States:** in_progress → reconciled | discrepancy_found → resolved

**Audit Requirements:**
- Log: creation, completion, discrepancy events, resolution — HIGHEST SENSITIVITY

---

### TrustReconciliationArtifact

**Purpose:** Supporting document for a trust reconciliation (e.g., bank statement upload, exported ledger report).

**Key Attributes:**
- `artifact_id` (PK, ULID) — required
- `reconciliation_id` (FK → TrustReconciliation) — required
- `artifact_type` (enum: bank_statement, ledger_export, register_export, resolution_memo) — required
- `file_name` (string) — required
- `storage_path` (string) — required
- `file_hash` (string; SHA-256) — required
- `uploaded_by_user_id` (FK → User) — required
- `created_at` (timestamp) — required

**Primary Key:** `artifact_id`

---

### TrustComplianceException

**Purpose:** Records any trust compliance violation or near-miss (e.g., attempted negative balance, stale reconciliation, unauthorized access attempt).

**Key Attributes:**
- `exception_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `ledger_id` (FK → TrustLedger) — optional
- `type` (enum: negative_balance_attempt, commingling_attempt, stale_reconciliation, unauthorized_access, large_transfer_alert, matter_close_with_balance) — required
- `severity` (enum: warning, critical) — required
- `description` (text) — required
- `triggered_by_user_id` (FK → User) — optional
- `related_entity_type` (string) — optional
- `related_entity_id` (ULID) — optional
- `resolution_status` (enum: open, resolved, acknowledged) — required
- `resolved_by_user_id` (FK → User) — optional
- `resolved_at` (timestamp) — optional
- `resolution_notes` (text) — optional
- `created_at` (timestamp) — required

**Primary Key:** `exception_id`

**Audit Requirements:**
- Log: ALWAYS logged — trust compliance exceptions are immutable and highest-sensitivity
