# SECTION 4 — RBAC & PERMISSIONS MATRIX

## 4.1 Role Definitions

| Code | Role | Scope | Description |
|------|------|-------|-------------|
| CLO | Chief Legal Officer | Firm | Full platform authority; controls capacity, pricing, compliance overrides |
| CEO | Managing Partner / CEO | Firm | Executive read access; limited to dashboards, reporting, marketing |
| COO | Chief Operating Officer | Firm | Operational read access; intake oversight, SLA, staffing |
| CFO | Chief Financial Officer | Firm | Financial authority; billing, accounting, trust (primary approver), forecasting |
| INTAKE | Intake Specialist | Office | Lead management, qualification, scheduling, fee collection |
| ATTORNEY | Attorney | Own matters | Matter work, consults, task management, AI summary approval |
| PARALEGAL | Paralegal / Case Manager | Assigned matters | Document, task, deadline management; time entries |
| BILLING | Billing / Collections Specialist | Assigned/Office | Invoice, payment, dunning, retainer management |
| MARKETING | Marketing Coordinator | Firm (marketing domain) | Content, campaigns, attribution, demand shaping requests |
| CLIENT | Client (Portal) | Own matters | Portal: status, documents, messages, payments |

---

## 4.2 Domain Permission Matrix

Legend: **V** = View, **C** = Create, **E** = Edit, **D** = Delete, **A** = Approve, **X** = Export, **—** = No access

### A) Identity & Org Structure

| Action | CLO | CEO | COO | CFO | INTAKE | ATTORNEY | PARALEGAL | BILLING | MARKETING | CLIENT |
|--------|-----|-----|-----|-----|--------|----------|-----------|---------|-----------|--------|
| Users - View | V (firm) | V (firm) | V (firm) | V (firm) | V (office) | V (own) | V (own) | V (own) | V (own) | — |
| Users - Create/Edit | C/E | — | — | — | — | — | — | — | — | — |
| Roles/Permissions | C/E/A | — | — | — | — | — | — | — | — | — |
| Office Config | C/E | — | V | — | — | — | — | — | — | — |
| Practice Areas | C/E | — | V | — | — | — | — | — | — | — |

### B) CRM / Intake

| Action | CLO | CEO | COO | CFO | INTAKE | ATTORNEY | PARALEGAL | BILLING | MARKETING | CLIENT |
|--------|-----|-----|-----|-----|--------|----------|-----------|---------|-----------|--------|
| Leads - View | V (firm) | — | V (firm) | — | V (office) | — | — | — | — | — |
| Leads - Create/Edit | C/E | — | — | — | C/E (office) | — | — | — | — | — |
| Leads - Qualify/Disqualify | A | — | — | — | C/E | — | — | — | — | — |
| Conflict Check - View | V | — | V | — | V (office) | — | — | — | — | — |
| Conflict Check - Override | A | — | — | — | — | — | — | — | — | — |
| Underwriting Score - View | V | — | V | — | V (office) | V (own consults) | — | — | — | — |
| Underwriting Score - Override | V | — | — | — | E (with reason) | — | — | — | — | — |
| Consultation Fee - Collect | — | — | — | — | C | — | — | — | — | — |
| Consultation Fee - Waive/Override | A | — | — | — | — (request only) | — | — | — | — | — |

### C) Matter Management

| Action | CLO | CEO | COO | CFO | INTAKE | ATTORNEY | PARALEGAL | BILLING | MARKETING | CLIENT |
|--------|-----|-----|-----|-----|--------|----------|-----------|---------|-----------|--------|
| Matters - View | V (firm) | — | V (firm) | — | V (originated, read-only) | V (own) | V (assigned) | V (billing tab) | — | — |
| Matters - Create | C | — | — | — | — | C | — | — | — | — |
| Matters - Edit | E | — | — | — | — | E (own) | E (assigned) | — | — | — |
| Stage Transitions | A (override) | — | — | — | — | E (own) | E (limited)* | — | — | — |
| Tasks - Create/Edit | C/E | — | — | — | — | C/E (own) | C/E (assigned) | — | — | — |
| Tasks - Complete | — | — | — | — | — | E | E | — | — | — |
| Deadlines - View | V | — | V | — | — | V (own) | V (assigned) | — | — | — |
| Deadlines - Waive (statutory) | A | — | — | — | — | — | — | — | — | — |
| Ethical Wall - Configure | C/E | — | — | — | — | — | — | — | — | — |
| AI Summaries - Approve | — | — | — | — | — | A (own) | A (assigned) | — | — | — |
| Matter Reassignment | E | — | — | — | — | — | — | — | — | — |
| Fee Arrangement - Set/Modify | E (audit) | — | — | — | — | — | — | — | — | — |

*Paralegal cannot advance Review → Execution without attorney approval.

### D) Documents & Signatures

| Action | CLO | CEO | COO | CFO | INTAKE | ATTORNEY | PARALEGAL | BILLING | MARKETING | CLIENT |
|--------|-----|-----|-----|-----|--------|----------|-----------|---------|-----------|--------|
| Documents - View | V (firm) | — | — | — | V (originated) | V (own) | V (assigned) | V (billing docs) | — | V (shared)** |
| Documents - Upload | C | — | — | — | — | C (own) | C (assigned) | C (billing) | — | C (portal) |
| Documents - Request from Client | — | — | — | — | — | C | C | — | — | — |
| E-Signatures - Initiate | — | — | — | — | — | C (own) | C (assigned) | — | — | — |
| E-Signatures - Sign | — | — | — | — | — | — | — | — | — | C (own) |
| Document Retention Config | E | — | — | — | — | — | — | — | — | — |

**Client sees only documents explicitly shared to portal (is_client_visible = true).

### E) Communications

| Action | CLO | CEO | COO | CFO | INTAKE | ATTORNEY | PARALEGAL | BILLING | MARKETING | CLIENT |
|--------|-----|-----|-----|-----|--------|----------|-----------|---------|-----------|--------|
| Queue Monitor - View | V (firm) | — | V (firm) | — | V (office) | V (read-only) | — | — | — | — |
| Queue - Answer/Transfer | — | — | — | — | C/E | — | — | — | — | — |
| Softphone - Use | — | — | — | — | C/E | C/E | C/E | C/E | — | — |
| Call Records - View | V (firm) | — | V (firm) | — | V (office) | V (own) | V (own) | V (own) | — | — |
| Call Recording - Listen | V (firm)*** | — | — | — | — | V (own) | — | — | — | — |
| Call Recording - Silent Monitor | V*** | — | — | — | — | — | — | — | — | — |
| IVR Config | E | — | E | — | — | — | — | — | — | — |
| Directory - View | V (firm) | — | V (firm) | — | V (firm) | V (firm) | V (firm) | V (firm) | — | — |
| Portal Messages - View/Reply | — | — | — | — | — | V/E (own) | V/E (assigned) | — | — | V/C (own) |
| Portal Messages - Approve outbound | — | — | — | — | — | A | A | — | — | — |

***With appropriate legal compliance notification per jurisdiction.

### F) Scheduling & Capacity

| Action | CLO | CEO | COO | CFO | INTAKE | ATTORNEY | PARALEGAL | BILLING | MARKETING | CLIENT |
|--------|-----|-----|-----|-----|--------|----------|-----------|---------|-----------|--------|
| Appointments - View | V (firm) | — | V (firm) | — | V (office) | V (own) | — | — | — | — |
| Appointments - Book/Reschedule | — | — | — | — | C/E (office) | — | — | — | — | E (reschedule link) |
| Appointments - Cancel | — | — | — | — | E | — | — | — | — | E (cancel link) |
| Appointments - Mark Complete | — | — | — | — | E | — | — | — | — | — |
| Appointments - Mark No-Show | — | — | — | — | E | — | — | — | — | — |
| No-Show Analytics | V | — | V | — | V (office) | — | — | — | — | — |
| Capacity Overview | V | V (read) | V | — | — | — | — | — | — | — |
| Capacity - Modify (overrides) | C/E/A | — | — | — | — | — | — | — | — | — |
| What-If Simulator | C/E | — | V (read-only) | — | — | — | — | — | — | — |
| Expansion Requests - Create | — | — | — | — | C | — | — | — | — | — |
| Expansion Requests - Approve/Deny | A | — | — | — | — | — | — | — | — | — |
| Open Appointment Policy | C/E | — | — | — | — | — | — | — | — | — |

### G) Billing & Collections

| Action | CLO | CEO | COO | CFO | INTAKE | ATTORNEY | PARALEGAL | BILLING | MARKETING | CLIENT |
|--------|-----|-----|-----|-----|--------|----------|-----------|---------|-----------|--------|
| Invoices - View | V (firm) | — | V (firm) | V (firm) | — | V (own, read) | — | V (assigned/office) | — | V (own, portal) |
| Invoices - Create/Edit | — | — | — | C/E | — | — | — | C/E | — | — |
| Invoices - Send | — | — | — | E | — | — | — | E | — | — |
| Payments - Record | — | — | — | C/E | — | — | — | C/E | — | C (portal pay) |
| Payment Plans - Manage | — | — | — | C/E | — | — | — | C/E | — | — |
| Write-Offs - Request | — | — | — | — | — | — | — | C | — | — |
| Write-Offs - Approve | A (joint) | — | — | A (joint) | — | — | — | — | — | — |
| Fee Overrides - Approve | A | — | — | — | — | — | — | — | — | — |
| Dunning - Execute (steps 1–2) | — | — | — | — | — | — | — | E (auto) | — | — |
| Dunning - Approve (steps 3–5) | A (step 5) | — | — | A (steps 3–5) | — | — | — | A (step 3) | — | — |
| Time Entries | — | — | — | — | — | C/E (own) | C/E (own) | — | — | — |
| Billing Dashboard / AR Aging | V | — | V | V | — | — | — | V | — | — |
| Export | X | — | X | X | — | — | — | X | — | — |

### H) Accounting & Forecasting

| Action | CLO | CEO | COO | CFO | INTAKE | ATTORNEY | PARALEGAL | BILLING | MARKETING | CLIENT |
|--------|-----|-----|-----|-----|--------|----------|-----------|---------|-----------|--------|
| Financial Statements | V (revenue) | V (read) | V (read) | V/E | — | — | — | — | — | — |
| Budget vs Actual | V (read) | V (read) | V (read) | V/E | — | — | — | — | — | — |
| Financial Forecasting | V (read) | V (read) | V (read) | V/E (adjust assumptions) | — | — | — | — | — | — |
| Capacity Forecasting | V/E (adjust inputs) | V (read) | V (read) | V (read) | — | — | — | — | — | — |
| Variance Thresholds | — | — | — | E | — | — | — | — | — | — |
| Export | X | X | X | X | — | — | — | — | — | — |

### I) Trust Accounting

| Action | CLO | CEO | COO | CFO | INTAKE | ATTORNEY | PARALEGAL | BILLING | MARKETING | CLIENT |
|--------|-----|-----|-----|-----|--------|----------|-----------|---------|-----------|--------|
| Trust Ledger - View | V (read) | — | — | V (full) | — | V (own matters) | — | V (own matters) | — | — |
| Trust Deposit - Record | — | — | — | C | — | — | — | C (initiate) | — | — |
| Trust Transfer - Request | — | — | — | C | — | — | — | C (initiate) | — | — |
| Trust Transfer - Approve | A (secondary) | — | — | A (primary) | — | — | — | — | — | — |
| Disbursement - Request | — | — | — | C | — | C (initiate) | — | C (initiate) | — | — |
| Disbursement - Approve | A | — | — | A | — | — | — | — | — | — |
| Reconciliation - Perform | — | — | — | C/E | — | — | — | — | — | — |
| Reconciliation - View | V (read) | — | — | V | — | — | — | — | — | — |
| Audit Trail - View | V | — | — | V | — | — | — | V (limited/own) | — | — |
| Audit Trail - Export | X | — | — | X | — | — | — | — | — | — |

**Critical Trust Controls:**
- CLO has trust transfer approval authority as secondary approver. CLO does NOT have transfer initiation authority (they can approve but not initiate per FSD).
- Negative balance prevention: HARD BLOCK — no role can override.
- Commingling prevention: transfers must be linked to an invoice or approved purpose — system-enforced.
- Large transfer (>$10K default): additional confirmation step required regardless of approver.

### J) Marketing

| Action | CLO | CEO | COO | CFO | INTAKE | ATTORNEY | PARALEGAL | BILLING | MARKETING | CLIENT |
|--------|-----|-----|-----|-----|--------|----------|-----------|---------|-----------|--------|
| Content Calendar | V/A (approve) | V (read) | — | — | — | V/A (review step) | — | — | V/C/E | — |
| Content Creation/Edit | — | — | — | — | — | — | — | — | C/E | — |
| Content Publish | — | — | — | — | — | — | — | — | E (after approval) | — |
| Funnel & Attribution | V (read) | V (read) | — | — | — | — | — | — | V/X | — |
| Demand Shaping - View | V | V (read) | — | — | — | — | — | — | V | — |
| Demand Shaping - Approve | A | — | — | — | — | — | — | — | — (request only) | — |
| Public Signals - View | V | — | — | — | — | — | — | — | V | — |
| Public Signals - Flag/Dismiss | — | — | — | — | — | — | — | — | E | — |
| Public Signals - Approve Outreach | A | — | — | — | — | — | — | — | — | — |

---

## 4.3 Special Access Restrictions

### Ethical Walls (Fiduciary Litigation)
- When `matter.ethical_wall_active = true`, users listed in `ethical_wall_restricted_user_ids` are denied ALL access to that matter
- The matter does not appear in their search results, matter lists, or any dashboard aggregations that would reveal matter-level detail
- Access denial is logged in AuditLog as a compliance event
- Only CLO can configure ethical walls

### Trust Ledger Segregation
- Trust data queries MUST filter by `firm_entity_id` AND `matter_id` — no cross-matter trust data leakage
- Billing specialists can view trust balances for their assigned matters but cannot approve transfers
- Attorneys can view trust balances for their matters but cannot initiate or approve transfers
- Trust audit trail access is restricted to CFO (full), CLO (full), and Billing (limited to own matters)

### Call Recording Access
- Call recordings are accessible only to: the agent who took the call, CLO (with compliance notice)
- COO can see call metadata (duration, disposition) but NOT listen to recordings
- Recording access events are logged in AuditLog

### Client Portal Data Isolation
- Client users can ONLY access data for their own matters
- Client queries are ALWAYS filtered by `contact_id` matching the authenticated client's contact
- No cross-client data leakage is possible — enforced at the query layer
- Client cannot see: internal notes, AI suggestions, pricing recommendations, underwriting scores, capacity data, staff assignments beyond name

---

## 4.4 CLO Authority Summary

The CLO role has the broadest authority in the platform. Explicit CLO controls:

| Authority | Scope |
|-----------|-------|
| Capacity thresholds and open appointment policies | Firmwide or per-office |
| Open appointment slot overrides (force-open/close) | Per attorney, audit-logged |
| Underwriting scoring thresholds (via Settings) | Firmwide |
| Consultation fee waiver/override approval | Per lead |
| Conflict check override approval | Per lead |
| Demand shaping approval (throttle/boost marketing) | Per practice area |
| Ethical wall configuration | Per matter |
| Statutory deadline waiver | Per deadline (audit-logged) |
| Fee arrangement override | Per matter (audit-logged) |
| Write-off approval (joint with CFO) | Per invoice |
| Content publication final approval | Per content item |
| Public signal outreach approval | Per signal |
| Trust transfer approval (secondary) | Per transfer request |
| Legal hold creation/release | Per matter or contact |

**CLO does NOT have:**
- Trust transfer initiation authority (can approve, not create requests)
- Unilateral financial forecast assumption changes (CFO domain)
- Ability to delete audit log entries (no one can)
- Ability to override negative trust balance blocks (no one can)
