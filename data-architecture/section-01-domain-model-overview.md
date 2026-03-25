# Legal Operations Platform — Data Architecture

## Document Version
- **Version:** 1.0
- **Date:** 2026-03-25
- **Source:** FSD v0.2, Wireframes v1.0, Acceptance Criteria v1.0
- **Purpose:** Implementation-grade data model, RBAC, audit taxonomy, KPI dictionary, and scaling approach

---

# SECTION 1 — DOMAIN MODEL OVERVIEW

## 1.1 Domains

The platform is organized into ten bounded domains. Each domain owns its entities and exposes data to other domains via well-defined references (foreign keys or domain events).

| # | Domain | Core Responsibility | Key Entities |
|---|--------|-------------------|--------------|
| A | **Identity & Org Structure** | Users, roles, permissions, offices, practice areas, firm hierarchy | User, Role, Permission, Office, PracticeArea, Team, Jurisdiction, FirmEntity, SkillTag |
| B | **CRM / Intake** | Lead capture, qualification, conflict checks, underwriting, consultation fee management | Lead, Contact, Household, ReferralSource, Campaign, IntakeQuestionnaire, ConflictCheck, ClientUnderwritingScore, UnderwritingFactor, StrategyRecommendation, PricingRecommendation, ConsultationFeePolicy |
| C | **Matter Management** | Matter lifecycle, posture tracking, tasks, deadlines, risk, filings | Matter, MatterType, WorkflowStage, Task, TaskTemplate, Deadline, DeadlineRule, RequiredArtifactRule, FilingEvent, MatterRiskSignal, MatterPostureSummary |
| D | **Documents & Signatures** | Document storage, versioning, signatures, retention | Document, DocumentType, DocumentVersion, SignaturePacket, DocumentRetentionPolicy, EvidenceOfDelivery |
| E | **Communications** | UCaaS (office main number + IVR), messaging, email, portal messaging, SLA tracking | OfficeMainNumber, CallFlow, IVRMenu, IVROption, Queue, RingGroup, RoutingRule, AfterHoursRule, OverflowRule, DirectoryEntry, CallEvent, CallRecord, VoicemailArtifact, RecordingArtifact, TranscriptArtifact, ConsentRecord, SMSMessage, EmailMessage, PortalMessage, CommunicationTimelineItem, ResponsivenessSLA, SLAEvent |
| F | **Scheduling & Capacity** | Appointments, capacity engine, open appointment slots, no-show tracking | Appointment, AppointmentType, AppointmentStatus, AppointmentConfirmationEvent, NoShowRecord, AttorneyCapacityProfile, CapacityComputation, OpenAppointmentSlot, OpenAppointmentPolicy, CapacityOverride |
| G | **Billing & Collections** | Invoices, payments, payment plans, AR aging, retainers, dunning, write-offs | Invoice, InvoiceLineItem, Payment, PaymentMethod, PaymentPlan, AccountsReceivableAgingSnapshot, RetainerAccount, RetainerTransaction, WriteOff, DunningSequence, DunningStep |
| H | **Accounting & Forecasting** | P&L, balance sheet, cash flow, budgets, forecasting | Budget, BudgetLine, ForecastModel, ForecastSnapshot, GeneralLedgerAccount, JournalEntry, CashReceipt, Disbursement |
| I | **Trust Accounting** | IOLTA/trust ledgers, transfers, reconciliation, compliance | TrustLedger, TrustLedgerTransaction, TrustTransferRequest, TrustReconciliation, TrustReconciliationArtifact, TrustComplianceException |
| J | **Marketing & Content** | Content calendar, campaigns, attribution, demand shaping, public signal monitoring | ContentItem, ContentCalendar, ContentDraft, ContentApproval, ContentPublicationEvent, ContentAttributionEvent, PublicSignalLead |
| K | **Analytics & Observability** | KPIs, dashboards, audit log, notifications, risk alerts | KPI, KPIMetricDefinition, KPIComputation, Dashboard, DashboardWidget, AuditLog, SystemEvent, RiskAlert, RemediationAction, Notification |

---

## 1.2 Canonical Identifiers

### ID Formation

All entities use a composite identifier strategy:

```
Format: {entity_prefix}_{ulid}
Example: MTR_01HX7YGBP2QZRK4M9VNWCD3E5F
```

- **ULID** (Universally Unique Lexicographically Sortable Identifier) — 128-bit, time-ordered, collision-resistant
- **Entity prefix** — 3-letter mnemonic (e.g., USR, OFC, MTR, LED, INV, TST) for human readability in logs and debugging
- **No sequential integers** as primary keys in application layer (database may use internal sequences for indexing)

### Uniqueness Guarantees

| Scope | Constraint |
|-------|-----------|
| User email | Globally unique within FirmEntity |
| Matter number | Unique within FirmEntity; format: `{YYYY}-{PracticeCode}-{sequence}` (e.g., `2026-EP-0147`) |
| Invoice number | Unique within FirmEntity; format: `INV-{sequence}` |
| Lead | Unique by Contact + FirmEntity (deduplication on name + phone/email) |
| Trust ledger | One per Matter (unique constraint: matter_id within TrustLedger) |

### Tenancy Model

The platform uses a **single-tenant, multi-entity** model:

- **FirmEntity** is the top-level tenant — represents a single law firm or an acquired firm entity
- All data is partitioned by `firm_entity_id` at the database level
- Within a FirmEntity, data is further scoped by `office_id` where applicable
- Cross-entity queries (for holding company roll-ups) require explicit join authorization

```
FirmEntity (tenant root)
  └── Office (1..10 in v1)
       └── User (assigned to 1+ offices)
       └── Matter (opened at an office)
       └── Lead (created at an office)
       └── OfficeMainNumber (1 per office)
```

---

## 1.3 Multi-Office and Multi-State Structure

### Office Entity

Each office has:
- Physical address and timezone
- Primary jurisdiction (state)
- One main phone number (per-office; not per-user DDI)
- Business hours configuration
- Practice areas offered (subset of firm-wide practice areas)
- Staff assignments (users belong to 1+ offices)

### Jurisdiction Entity

A Jurisdiction represents a state-level regulatory scope:
- Governs: deadline rules, required artifact rules, filing requirements, trust accounting rules, bar rules for marketing
- Initial deployment: Virginia (VA) and North Carolina (NC)
- Each matter is assigned a primary jurisdiction at creation
- Jurisdiction-scoped rules have effective dates for versioning

### FirmEntity (Acquisitions Support)

- A FirmEntity is a discrete legal entity within a holding structure
- v1: single FirmEntity per deployment
- Future: multiple FirmEntities under a HoldingCompany, each with its own office set, matter numbering, and trust accounts
- Data isolation between FirmEntities is enforced at the query layer

### Multi-State Scaling Approach

```
FirmEntity
  ├── Jurisdiction: VA
  │    ├── DeadlineRule (VA-specific probate timelines)
  │    ├── RequiredArtifactRule (VA engagement letter requirements)
  │    └── TrustAccountingRule (VA IOLTA compliance)
  ├── Jurisdiction: NC
  │    ├── DeadlineRule (NC-specific probate timelines)
  │    ├── RequiredArtifactRule (NC engagement letter requirements)
  │    └── TrustAccountingRule (NC IOLTA compliance)
  └── Office: Main Street (jurisdiction: VA)
       └── Matter: 2026-EP-0147 (jurisdiction: VA → inherits VA rules)
```

---

## 1.4 Data Retention and Legal Hold Requirements (High Level)

### Retention Categories

| Category | Minimum Retention | Rationale |
|----------|------------------|-----------|
| Active matter data | Duration of matter + retention period | Engagement obligations |
| Closed matter data | 7 years post-close (configurable per practice area) | Statute of limitations; malpractice tail |
| Trust ledger records | 7 years post-final reconciliation (or longer per state bar) | Fiduciary compliance |
| Trust reconciliation artifacts | 7 years post-reconciliation | Audit requirements |
| Communication recordings | Per consent and jurisdiction rules; default 3 years | Compliance; defensibility |
| Communication transcripts | Matches recording retention | Same |
| Documents (client-provided) | Matches matter retention | Part of matter file |
| Documents (firm work product) | Matches matter retention | Part of matter file |
| Marketing data (campaigns, attribution) | 3 years | Business analytics |
| Public signal monitoring data | 1 year (or until opt-out; then immediate PII removal) | Ethics compliance |
| Audit logs | 7 years minimum; never auto-deleted | Compliance; immutability |
| Financial records (invoices, payments, GL) | 7 years | Tax and regulatory |

### Legal Hold Model

- A **LegalHold** entity can be applied to a Matter, Contact, or date range
- When a legal hold is active, ALL data associated with the held entity is exempt from automated retention deletion
- Legal holds are created by CLO or designated compliance officer
- Legal holds are logged in the audit trail (creation, modification, release)
- System prevents deletion of any entity or artifact that is under an active legal hold
- Legal holds survive matter closure

### Export Requirements

- All entity data must be exportable in structured format (CSV/JSON) for regulatory requests
- Trust accounting data must be exportable in formats compatible with state bar audit requirements
- Audit logs must be exportable with chain-of-custody metadata (hash, timestamp, exporter)
- Client data export must support data subject access requests (where applicable)
- All exports are logged in the audit trail with: who exported, what scope, when, and purpose
