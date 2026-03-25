# Data Architecture — Legal Operations Platform

## Document Version
- **Version:** 1.0
- **Date:** 2026-03-25
- **Source:** FSD v0.2, Wireframes v1.0, Acceptance Criteria v1.0
- **Purpose:** Implementation-grade data model, RBAC, audit taxonomy, KPI dictionary, and scaling approach

---

## Document Index

| Section | File | Description |
|---------|------|-------------|
| 1 | `section-01-domain-model-overview.md` | Domains, canonical IDs, tenancy, multi-office/state, retention framework |
| 2A | `section-02a-entities-identity-org.md` | User, Role, Permission, Office, PracticeArea, Team, Jurisdiction, FirmEntity, SkillTag |
| 2B | `section-02b-entities-crm-intake.md` | Lead, Contact, Household, ReferralSource, Campaign, IntakeQuestionnaire, ConflictCheck, Underwriting, Pricing, ConsultationFee |
| 2C | `section-02c-entities-matter-management.md` | Matter, MatterType, WorkflowStage, Task, TaskTemplate, Deadline, DeadlineRule, RequiredArtifactRule, FilingEvent, RiskSignal, PostureSummary |
| 2D | `section-02d-entities-documents-signatures.md` | Document, DocumentType, DocumentVersion, SignaturePacket, RetentionPolicy, EvidenceOfDelivery |
| 2E | `section-02e-entities-communications.md` | OfficeMainNumber, CallFlow, IVR, Queue, RingGroup, Routing, CallRecord/Event, Recording/Transcript, Consent, SMS, Email, PortalMessage, SLA |
| 2F | `section-02f-entities-scheduling-capacity.md` | Appointment, Confirmation, NoShow, AttorneyCapacityProfile, CapacityComputation, OpenAppointmentSlot/Policy, CapacityOverride, ExpansionRequest |
| 2G | `section-02g-entities-billing-accounting.md` | Invoice, Payment, PaymentMethod, PaymentPlan, Dunning, AR Aging, Retainer, WriteOff, Budget, Forecast, GL, JournalEntry |
| 2H | `section-02h-entities-trust-accounting.md` | TrustLedger, TrustLedgerTransaction, TrustTransferRequest, TrustReconciliation, TrustComplianceException |
| 2I | `section-02i-entities-marketing-analytics.md` | ContentItem, ContentDraft, ContentApproval, Publication, Attribution, PublicSignalLead, KPI, Dashboard, AuditLog, SystemEvent, RiskAlert, Notification |
| 3 | `section-03-relationship-diagram.md` | Textual ERD: tenancy, lead-to-matter, matter hub, financial, communications, marketing, observability |
| 4 | `section-04-rbac-permissions.md` | Full RBAC matrix for 10 roles across all domains; special access restrictions; CLO authority summary |
| 5 | `section-05-audit-logging.md` | Immutable audit log structure, 3-tier sensitivity, 14-category event taxonomy with 150+ event types |
| 6 | `section-06-kpi-dictionary.md` | 22 KPIs with formulas, data sources, refresh cadence, drilldowns |
| 7 | `section-07-multi-state-rules.md` | Jurisdiction-scoped rules, versioning, rule inheritance, VA/NC baseline, scaling to new states |
| 8 | `section-08-data-governance.md` | Retention schedule, legal hold model, export requirements, data classification, governance roles |

---

## Entity Count Summary

| Domain | Entity Count |
|--------|-------------|
| Identity & Org Structure | 9 |
| CRM / Intake | 12 |
| Matter Management | 11 |
| Documents & Signatures | 6 |
| Communications | 18 |
| Scheduling & Capacity | 10 |
| Billing & Collections | 13 |
| Trust Accounting | 5 |
| Marketing & Content | 7 |
| Analytics & Observability | 9 |
| **Total** | **100** |

---

## Key Design Decisions

1. **ULID-based identifiers** — time-ordered, collision-resistant, human-readable with entity prefix
2. **Single-tenant, multi-entity model** — FirmEntity is tenant root; all queries partitioned by firm_entity_id
3. **Jurisdiction-scoped rules with effective dates** — no code changes needed to add states
4. **Trust accounting hard blocks** — negative balance and commingling prevention are system-enforced invariants with no override
5. **Append-only audit trail** — immutable, 7-year minimum retention, exportable with chain-of-custody hashing
6. **Approval-gated automation** — dunning steps 1–2 auto-execute; all other automation requires human approval
7. **AI disclosure data model** — every AI-generated entity carries `is_ai_generated`, `model_version`, and `disclaimer_text` fields
8. **Capacity engine as data authority** — OpenAppointmentSlot table is the single source of truth for bookable slots; intake cannot bypass
9. **Legal holds cascade** — hold on matter preserves all scoped data; survives matter closure
10. **PII encryption and masking** — PII encrypted at rest; masked in audit logs; opt-out triggers immediate removal
