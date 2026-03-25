# SECTION 8 — DATA GOVERNANCE & RETENTION (FUNCTIONAL)

## 8.1 Data Retention Categories

### Retention Schedule

| Category | Data Types | Minimum Retention | Retention Start Event | Purge Method |
|----------|-----------|-------------------|----------------------|-------------|
| **Active Matter Data** | Matter, Tasks, Deadlines, PostureSummaries | Duration of matter + post-close retention | Matter close date | Scheduled review after retention period |
| **Closed Matter File** | All matter-scoped entities | 7 years post-close (configurable per practice area) | Matter.closed_at | Automated flag for review; manual approval to purge |
| **Client Contact Data** | Contact, Household | 7 years after last matter closes | Last Matter.closed_at for the Contact | Review + purge after period |
| **Documents (Client-Provided)** | Documents with source = client_upload | Matches matter retention | Matter close date | Purged with matter file |
| **Documents (Firm Work Product)** | Documents with source = firm_draft | Matches matter retention | Matter close date | Purged with matter file |
| **Documents (Court Filings)** | Documents with source = court_filing | Matches matter retention or longer per jurisdiction | Matter close date | Review; may require extended retention |
| **Trust Ledger Records** | TrustLedger, TrustLedgerTransaction | 7 years post-final reconciliation (or longer per state bar) | Last TrustReconciliation date for ledger | Review; manual approval required |
| **Trust Reconciliation Artifacts** | TrustReconciliation, TrustReconciliationArtifact | 7 years post-reconciliation | TrustReconciliation.reconciled_at | Automated flag; manual approval |
| **Communication Recordings** | RecordingArtifact | Per jurisdiction consent rules; default 3 years | Recording creation date | Automated deletion after retention_expires_at |
| **Communication Transcripts** | TranscriptArtifact | Matches recording retention | Recording creation date | Deleted when associated recording is deleted |
| **Call Records (Metadata)** | CallRecord, CallEvent | 5 years | Call end date | Automated purge |
| **SMS/Email Messages** | SMSMessage, EmailMessage | 3 years (or matches matter retention if matter-linked) | Message sent date | Automated purge for unlinked; matter retention for linked |
| **Portal Messages** | PortalMessage | Matches matter retention | Matter close date | Purged with matter file |
| **Financial Records** | Invoice, Payment, JournalEntry, Budget | 7 years | Fiscal year end | Automated flag; manual approval |
| **Billing Records** | PaymentPlan, DunningSequence, RetainerTransaction | 7 years | Last transaction date | Automated flag; manual approval |
| **Marketing Data** | Campaign, ContentItem, ContentAttributionEvent | 3 years | Campaign end date or publication date | Automated purge |
| **Public Signal Data** | PublicSignalLead | 1 year; or until opt-out (then immediate PII removal) | Signal creation date | Automated purge; immediate on opt-out |
| **Audit Logs** | AuditLog | 7 years minimum; **never auto-deleted** | Log creation date | No automated purge; manual archive after 7 years with CLO approval |
| **System Events** | SystemEvent | 90 days | Event date | Automated purge (operational, not compliance) |
| **Notifications** | Notification | 1 year | Notification creation date | Automated purge |
| **KPI Computations** | KPIComputation | 5 years | Computation date | Automated rollup to monthly aggregates after 1 year; purge detail after 5 |
| **Capacity Computations** | CapacityComputation | 2 years | Computation date | Automated purge |

---

## 8.2 Legal Hold Model

### LegalHold Entity

```
LegalHold:
  - hold_id (PK, ULID)
  - firm_entity_id (FK → FirmEntity)
  - name (string; descriptive identifier)
  - scope_type (enum: matter, contact, date_range, custom)
  - matter_id (FK → Matter) — optional
  - contact_id (FK → Contact) — optional
  - date_range_start (date) — optional
  - date_range_end (date) — optional
  - custom_scope_description (text) — optional
  - reason (text) — required
  - status (enum: active, released)
  - created_by_user_id (FK → User; CLO or compliance officer)
  - created_at (timestamp)
  - released_by_user_id (FK → User)
  - released_at (timestamp)
  - released_reason (text)
```

### Legal Hold Behavior

1. **Creation:** Only CLO or designated compliance officer can create a legal hold. Logged in AuditLog.

2. **Scope cascading:**
   - Matter hold → preserves all matter-scoped entities (tasks, documents, deadlines, invoices, trust ledger, communications, timeline items)
   - Contact hold → preserves all data related to the contact across all their matters
   - Date range hold → preserves all data with timestamps within the range

3. **Retention override:** When a legal hold is active, the automated retention system skips ALL entities within the hold's scope — no purging, no archiving, no deletion.

4. **Deletion blocking:** If any automated or manual deletion attempt targets a held entity:
   - The deletion is blocked
   - An audit entry is logged: `system.retention.deletion_blocked`
   - The requesting user is notified: "This data is under legal hold and cannot be deleted."

5. **Survival:** Legal holds survive matter closure. A closed matter with an active hold retains all data indefinitely until the hold is released.

6. **Release:** Only CLO or compliance officer can release a hold. Requires a reason. Logged in AuditLog. After release, normal retention timers resume from the release date (not the original creation date).

---

## 8.3 Export Requirements

### Regulatory and Compliance Exports

| Export Type | Scope | Format | Authorized Roles | Use Case |
|-------------|-------|--------|------------------|----------|
| Trust Accounting Audit | Per IOLTA account, per period | CSV + PDF | CFO, CLO | State bar audit response |
| Trust Reconciliation Package | Per reconciliation | PDF bundle (statement + ledger + register) | CFO, CLO | State bar trust audit |
| Matter File Export | All data for a single matter | JSON + document ZIP | CLO, Attorney (own) | Malpractice defense, transfer, regulatory |
| Client Data Export | All data for a single contact | JSON + document ZIP | CLO | Data subject access request |
| Audit Log Export | Filtered by entity, actor, date range, category | CSV + JSON | CLO, CFO | Internal investigation, regulatory request |
| Financial Statements | P&L, Balance Sheet, Cash Flow for period | PDF, XLS | CFO, CLO, CEO, COO | Tax, regulatory, board reporting |
| AR Aging Report | Full AR snapshot | CSV, PDF | CFO, Billing | Collections management |
| Invoice Package | Invoice + supporting documents | PDF | CFO, Billing | Client dispute resolution |

### Export Controls

1. **Every export is logged** in AuditLog with: `system.data_export.initiated` and `system.data_export.completed` events capturing who exported, what scope, what format, and stated purpose.

2. **Chain-of-custody metadata** is embedded in every export file:
   ```json
   {
     "export_id": "EXP_01HX...",
     "exported_by": "user_id",
     "exported_at": "2026-03-25T14:30:00Z",
     "scope": "trust_ledger:matter_id=MTR_01HX...",
     "record_count": 47,
     "sha256_hash": "a4f2b9c..."
   }
   ```

3. **Audit log exports** include a cumulative hash chain where each entry's hash includes the previous entry's hash, enabling tamper detection.

4. **Access restriction:** Export permissions follow the RBAC matrix. A user can only export data they have view access to. Client portal does not support bulk data export (limited to downloading individual documents and payment statements).

---

## 8.4 Data Classification

| Classification | Examples | Controls |
|----------------|----------|----------|
| **Restricted** | Trust ledger transactions, call recordings, client PII (SSN, DOB), payment tokens | Encrypted at rest and in transit; access logged; minimal exposure |
| **Confidential** | Matter details, invoices, underwriting scores, legal strategy, work product | Role-based access; audit logged; encrypted at rest |
| **Internal** | KPI computations, capacity data, campaign metrics, operational dashboards | Staff-only access; standard encryption |
| **Public** | Published blog posts, firm directory (name/role only), office addresses | No access restriction after publication |

### PII Handling
- PII fields (Contact: name, email, phone, address, DOB) are marked in the data dictionary
- PII is encrypted at rest in the database
- PII is masked in logs (audit log stores entity references, not raw PII values in before/after states for PII fields)
- PII deletion is gated by legal holds and retention policies
- Public Signal opt-out triggers immediate PII removal from the PublicSignalLead record (summary retained, identifiers removed)

---

## 8.5 Backup and Recovery (Functional Requirements)

1. **Trust accounting data** requires point-in-time recovery capability to any point within the retention period — this is the highest-priority recovery class
2. **Audit logs** are backed up to immutable storage (write-once, read-many) separate from the primary database
3. **Matter data** is recoverable to the point of the most recent transaction
4. **Recovery testing** should be conducted quarterly for trust accounting data and annually for all other categories (operational requirement, not a data model concern)

---

## 8.6 Data Governance Roles

| Role | Governance Responsibility |
|------|--------------------------|
| CLO | Data retention policy approval; legal hold authority; compliance audit oversight |
| CFO | Trust data integrity; financial data accuracy; reconciliation sign-off |
| COO | Operational data quality; SLA configuration; system configuration |
| System Administrator | Backup execution; retention job management; access provisioning (under CLO direction) |

**No single individual** has the ability to both create data and delete/archive that same data without a second approver. This separation of duties is enforced at the RBAC level.
