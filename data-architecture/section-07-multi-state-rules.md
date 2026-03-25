# SECTION 7 — MULTI-STATE RULES SUPPORT (DATA DESIGN)

## 7.1 Jurisdiction-Scoped Rule Model

The platform supports jurisdiction-specific business rules through two primary rule entities: **DeadlineRule** and **RequiredArtifactRule**. These are versioned with effective dates to support rule changes over time without breaking historical matters.

### Rule Selection Flow

```
Matter is created:
  1. Determine jurisdiction (default: office primary jurisdiction; user can override)
  2. Determine practice area and matter type
  3. Query active rules:
     WHERE jurisdiction_id = matter.jurisdiction_id
       AND (practice_area_id = matter.practice_area_id OR practice_area_id IS NULL)
       AND (matter_type_id = matter.matter_type_id OR matter_type_id IS NULL)
       AND effective_date <= TODAY
       AND (end_date IS NULL OR end_date >= TODAY)
  4. Specificity wins: matter_type-specific rules override practice-area-only rules
  5. Generate Deadline instances and RequiredArtifact checklists for the matter
```

### Initial Jurisdictions

| State | Code | Key Characteristics |
|-------|------|-------------------|
| Virginia | VA | Probate: petition within 30 days of qualifying event. Creditor notice: 10 days after appointment. IOLTA: VSB rules. Recording consent: one-party. |
| North Carolina | NC | Probate: petition within 60 days of death. Creditor notice: published. IOLTA: NC State Bar Trust Account Rules. Recording consent: one-party. |

---

## 7.2 Versioning Rules with Effective Dates

Each rule entity uses an **effective date range** model:

```
DeadlineRule:
  - jurisdiction_id: VA
  - name: "Petition for Probate"
  - trigger_event: "death_date"
  - offset_days: 30
  - effective_date: 2020-01-01
  - end_date: NULL            ← Currently active
  - version: 1

DeadlineRule (future version):
  - jurisdiction_id: VA
  - name: "Petition for Probate"
  - trigger_event: "death_date"
  - offset_days: 45           ← Legislature changes deadline
  - effective_date: 2027-07-01
  - end_date: NULL
  - version: 2
```

**Version transition logic:**
- When `effective_date` of version 2 arrives, version 1 auto-receives `end_date = 2027-06-30`
- Matters opened before 2027-07-01 retain their version 1 deadlines (already instantiated)
- Matters opened on or after 2027-07-01 receive version 2 deadlines
- Historical rules are never deleted — only end-dated

---

## 7.3 How Matters Select Jurisdiction and Inherit Rule Sets

### At Matter Creation

1. **Default jurisdiction** = the office's `primary_jurisdiction_id`
2. **Override allowed**: If the matter's legal situs (where the property is, where the decedent died, where the court is) differs from the office location, the creating attorney or paralegal can select a different jurisdiction
3. **Jurisdiction lock**: Once deadlines and artifacts are instantiated from rules, changing the jurisdiction requires CLO approval (audit-logged) and regeneration of deadline/artifact sets

### Rule Instantiation

When a matter is created:
1. System queries all active `DeadlineRule` records matching the matter's jurisdiction + practice area + matter type
2. For each rule, a `Deadline` instance is created on the matter:
   - `due_date` = trigger_event_date ± offset_days (business days if `is_business_days = true`)
   - `type` = rule's deadline_type
   - `is_statutory` = true for statutory/filing types
   - `deadline_rule_id` references the source rule for traceability
3. System queries all active `RequiredArtifactRule` records matching the same criteria
4. For each rule, the matter's required document checklist is populated, linked to the appropriate workflow stage

### Trigger Events

Deadline rules reference trigger events by name. The system maps these to matter or external date fields:

| Trigger Event | Source |
|--------------|--------|
| `matter_created` | Matter.opened_at |
| `death_date` | Matter custom field (estate admin) |
| `appointment_date` | Court appointment date (estate admin) |
| `filing_date` | FilingEvent.filed_date |
| `engagement_date` | Matter.opened_at (for engagement letters) |
| `close_date` | Matter.closed_at (for post-close retention) |

---

## 7.4 Exceptions and Overrides

### Rule Exceptions

Some matters may have unique circumstances that require deviating from standard rules. The data model supports this via:

1. **Deadline-level overrides:** A specific `Deadline` instance on a matter can have its `due_date` adjusted by an attorney or paralegal. The original rule-derived date is preserved in the audit log (`before_state`).

2. **Statutory deadline waivers:** Only CLO can waive a statutory deadline. This creates a `Deadline.status = waived` with a `waived_reason` and audit entry.

3. **Jurisdiction exception notes:** A matter can have a `jurisdiction_exception_notes` field documenting why a non-standard jurisdiction was selected or why rules were modified.

### Override Audit Trail

Every exception or override generates an audit log entry:

```
Event: compliance.deadline.waived
Actor: CLO
Entity: Deadline (deadline_id)
Before: { due_date: "2026-04-23", status: "upcoming" }
After: { status: "waived" }
Reason: "Court granted extension per motion filed 2026-04-10"
```

---

## 7.5 Scaling to New States

Adding a new state requires:

1. **Create Jurisdiction record** with state code, consent type, and trust accounting rule reference
2. **Create DeadlineRule records** for each practice area's statutory and regulatory deadlines in that state
3. **Create RequiredArtifactRule records** for any state-specific document requirements
4. **Update Office records** that operate in the new state to reference the new Jurisdiction
5. **No code changes** — the rule engine is data-driven

### Acquisition Scaling

When a firm acquires a practice in a new state:
1. A new `FirmEntity` can be created (or the existing one extended)
2. A new `Office` is created with the new state's `Jurisdiction`
3. Jurisdiction-specific rules are loaded for the new state
4. Existing data model, RBAC, and audit infrastructure applies unchanged
5. Trust accounting: new IOLTA bank accounts are set up as new `GeneralLedgerAccount` entries with `is_trust_account = true`

---

## 7.6 Trust Accounting State Variations

Trust accounting rules vary by state. The model supports this via:

| State | IOLTA Rules | Reconciliation Requirement | Interest Rules |
|-------|-------------|---------------------------|----------------|
| VA | VSB Rule 1.15 | Monthly three-way reconciliation | Interest to VSB Legal Services Fund |
| NC | NC Rule 1.15-2 | Monthly three-way reconciliation | Interest to NC IOLTA program |

The `Jurisdiction.trust_accounting_rule_set` JSON field stores high-level reference to the applicable state bar rules. The `TrustReconciliation` entity's reconciliation workflow is state-agnostic (always three-way), but the reconciliation period (monthly, quarterly) and any state-specific reporting requirements are configurable per jurisdiction.

---

## 7.7 Marketing Bar Rules by State

Marketing and advertising rules vary by state. The `Jurisdiction` entity's presence determines which marketing compliance rules apply:

| State | Key Marketing Rules |
|-------|-------------------|
| VA | VSB Rules 7.1–7.5: advertising, solicitation, communication |
| NC | NC RPC 7.1–7.6: advertising, solicitation restrictions |

The `PublicSignalLead` entity includes a `compliance_reviewed` flag and `compliance_reviewer_user_id` (CLO) to ensure all public-signal-based outreach is reviewed against the applicable state's bar rules. The permanent ethics disclaimer on the Marketing screen is jurisdiction-aware.
