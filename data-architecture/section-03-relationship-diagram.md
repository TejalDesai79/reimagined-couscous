# SECTION 3 — RELATIONSHIP DIAGRAM (TEXTUAL)

## Core Entity Relationships

The following describes the 15 core entities and their primary relationships, grouped by scope.

---

## 3.1 Tenancy & Organizational Hierarchy

```
FirmEntity (tenant root)
  │
  ├──1:N── Office
  │          ├──N:M── User (via UserOffice)
  │          ├──1:N── Lead
  │          ├──1:N── Matter
  │          ├──1:1── OfficeMainNumber ──1:1── CallFlow ──1:1── IVRMenu ──1:N── IVROption ──1:1── Queue
  │          └──N:M── PracticeArea (via OfficePracticeArea)
  │
  ├──1:N── Jurisdiction
  │          ├──1:N── DeadlineRule (versioned, by practice area)
  │          └──1:N── RequiredArtifactRule (versioned, by practice area)
  │
  ├──1:N── User
  │          ├──N:1── Role ──N:M── Permission (via RolePermission)
  │          ├──N:M── PracticeArea (via UserPracticeArea; attorneys)
  │          ├──N:M── SkillTag (via UserSkillTag)
  │          └──0:1── AttorneyCapacityProfile (attorneys only)
  │
  └──1:N── PracticeArea
             └──1:N── MatterType ──1:1── WorkflowTemplate ──1:N── WorkflowStage ──1:N── TaskTemplate
```

---

## 3.2 Lead-to-Matter Lifecycle

```
Contact ──1:N── Lead
                  │
                  ├──0:1── IntakeQuestionnaire
                  ├──0:1── ConflictCheck
                  ├──0:1── ClientUnderwritingScore ──1:N── UnderwritingFactor
                  │                                  ├──1:N── StrategyRecommendation
                  │                                  └──1:N── PricingRecommendation
                  ├──0:1── ConsultationFeePayment
                  ├──0:1── ReferralSource
                  ├──0:1── Campaign
                  ├──0:N── Appointment
                  │
                  └── [converts to] ──0:1── Matter
```

---

## 3.3 Matter as Central Hub

```
Matter (central entity — most relationships converge here)
  │
  ├──N:1── Contact (client)
  ├──N:1── Office
  ├──N:1── PracticeArea
  ├──N:1── Jurisdiction
  ├──N:1── MatterType
  ├──N:1── User (attorney)
  ├──N:1── User (paralegal)
  ├──0:1── Lead (origin)
  │
  ├──1:N── Task
  ├──1:N── Deadline
  ├──1:N── Document ──1:N── DocumentVersion
  │                   └──0:1── SignaturePacket
  ├──1:N── FilingEvent
  ├──1:N── MatterRiskSignal
  ├──1:N── MatterPostureSummary (versioned snapshots)
  ├──1:N── CommunicationTimelineItem
  │
  ├──1:N── Invoice ──1:N── InvoiceLineItem
  │                  ├──1:N── Payment
  │                  └──0:1── DunningSequence ──1:N── DunningStep
  │
  ├──0:1── RetainerAccount ──1:N── RetainerTransaction
  ├──0:1── TrustLedger ──1:N── TrustLedgerTransaction
  │                      └──1:N── TrustTransferRequest
  │
  ├──1:N── Appointment
  ├──1:N── CallRecord (matched by caller ID)
  └──1:N── PortalMessage
```

---

## 3.4 Financial & Trust Accounting

```
Invoice ──1:N── Payment
  │        └──N:1── PaymentMethod ──N:1── Contact
  │
  ├── [retainer draw] ── RetainerAccount ──1:N── RetainerTransaction
  │
  └── [trust draw] ── TrustTransferRequest ──N:1── TrustLedger
                        │                          │
                        │                          ├──1:N── TrustLedgerTransaction (APPEND-ONLY)
                        │                          └──N:1── Matter
                        │
                        └── [approved →] TrustLedgerTransaction
                                          └── [reconciled via] TrustReconciliation ──1:N── TrustReconciliationArtifact

GeneralLedgerAccount ──1:N── JournalEntry (double-entry)
  │
  └── [IOLTA accounts flagged as is_trust_account = true]
```

---

## 3.5 Communications & Scheduling

```
OfficeMainNumber ──1:1── CallFlow
                           ├──1:1── IVRMenu ──1:N── IVROption ──N:1── Queue ──1:1── RingGroup ──N:M── User
                           ├──1:1── AfterHoursRule
                           └──0:1── OverflowRule

CallRecord ──1:N── CallEvent
  │
  ├──0:1── Contact (caller ID match)
  ├──0:1── Matter (matched)
  ├──0:1── RecordingArtifact ──1:1── ConsentRecord
  └──0:1── TranscriptArtifact

Appointment ──N:1── OpenAppointmentSlot ──0:1── CapacityOverride
  │
  ├──1:N── AppointmentConfirmationEvent
  ├──0:1── NoShowRecord
  └──N:1── Contact

AttorneyCapacityProfile ──1:N── CapacityComputation
  └── [overridden by] ── CapacityOverride (CLO; audit-logged)
```

---

## 3.6 Marketing & Attribution

```
Campaign ──1:N── ContentItem ──1:N── ContentDraft
                   │            └──1:N── ContentApproval
                   │
                   └──0:1── ContentPublicationEvent

ContentAttributionEvent ──N:1── Lead
                          ├──0:1── ContentItem
                          └──0:1── Campaign

PublicSignalLead ──0:1── Lead (if converted; compliance-gated)
```

---

## 3.7 Observability & Cross-Cutting

```
AuditLog (immutable, append-only)
  ├── references any entity via (entity_type, entity_id)
  └── references actor via (actor_user_id)

RiskAlert ──1:N── RemediationAction
  └── references entity via (related_entity_type, related_entity_id)

Notification ──N:1── User (recipient)
  └── references source via (source_entity_type, source_entity_id)

KPIMetricDefinition ──1:N── KPIComputation
Dashboard ──1:N── DashboardWidget ──N:1── KPIMetricDefinition
```

---

## 3.8 Entity Scope Classification

### Firm-Scoped (shared across all offices)
- FirmEntity, Jurisdiction, PracticeArea, MatterType, WorkflowTemplate, WorkflowStage, TaskTemplate
- Role, Permission, SkillTag, DocumentType, DocumentRetentionPolicy
- KPIMetricDefinition, Dashboard, ResponsivenessSLA
- ConsultationFeePolicy (firmwide defaults)
- DeadlineRule, RequiredArtifactRule (jurisdiction-scoped, not office-scoped)
- ForecastModel, Budget, GeneralLedgerAccount
- AuditLog, SystemEvent

### Office-Scoped
- Office, OfficeMainNumber, CallFlow, IVRMenu, Queue, RingGroup
- Lead (created at an office)
- Appointment, OpenAppointmentSlot
- CapacityExpansionRequest
- AccountsReceivableAgingSnapshot (office-level snapshots)
- ConsultationFeePolicy (office overrides)

### Matter-Scoped
- Matter, Task, Deadline, Document, FilingEvent
- MatterRiskSignal, MatterPostureSummary
- Invoice, RetainerAccount, TrustLedger
- CommunicationTimelineItem, PortalMessage
- TrustTransferRequest, TrustLedgerTransaction

### User-Scoped
- User, AttorneyCapacityProfile, CapacityComputation, DirectoryEntry
- Notification
- CallRecord (answered_by)
- NoShowRecord (marked_by)

### Contact-Scoped
- Contact, Household, PaymentMethod
- ConsultationFeePayment

---

## 3.9 Jurisdiction Mapping

```
Matter ──N:1── Jurisdiction
                  │
                  ├── DeadlineRule[] (filtered by jurisdiction + practice area + matter type)
                  ├── RequiredArtifactRule[] (filtered by jurisdiction + practice area)
                  └── ConsentRecord.consent_type_required (one_party vs two_party per state)

Office ──N:1── Jurisdiction (primary)
               └── affects: default jurisdiction for new matters created at that office
```

When a matter is created:
1. Default jurisdiction = office's primary jurisdiction
2. User can override if matter's legal situs differs from office location
3. All DeadlineRules and RequiredArtifactRules are inherited based on matter's jurisdiction + practice area + matter type
4. Consent requirements for call recording are derived from jurisdiction.recording_consent_type
