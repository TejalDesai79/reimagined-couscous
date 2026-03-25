# SECTION 2 — CANONICAL ENTITY LIST (continued)

## Part C: Matter Management

---

### Matter

**Purpose:** A legal engagement with a client. Central entity linking all case activity: tasks, documents, billing, communications, and posture tracking.

**Key Attributes:**
- `matter_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — required
- `matter_number` (string; format: `{YYYY}-{PracticeCode}-{sequence}`) — required
- `name` (string; descriptive, e.g., "Garcia Estate Plan") — required
- `contact_id` (FK → Contact; the client) — required
- `household_id` (FK → Household) — optional
- `matter_type_id` (FK → MatterType) — required
- `practice_area_id` (FK → PracticeArea) — required
- `jurisdiction_id` (FK → Jurisdiction) — required
- `lead_id` (FK → Lead) — optional (originating lead)
- `attorney_user_id` (FK → User) — required
- `paralegal_user_id` (FK → User) — optional
- `stage` (enum: engaged, in_progress, client_review, delivery, closed) — required
- `workflow_template_id` (FK → WorkflowTemplate) — required
- `current_posture_step` (integer) — required
- `total_posture_steps` (integer) — required
- `fee_arrangement` (enum: fixed_fee, hourly, tiered, hybrid, contingency) — required
- `agreed_fee_amount` (decimal) — optional (for fixed/tiered)
- `consultation_fee_credited` (boolean) — required (default false; true when consult fee is credited)
- `estimated_completion_date` (date) — optional
- `opened_at` (timestamp) — required
- `closed_at` (timestamp) — optional
- `close_reason` (enum: completed, withdrawn, transferred, disqualified) — optional
- `ethical_wall_active` (boolean) — required (default false)
- `ethical_wall_restricted_user_ids` (FK[] → User) — optional
- `legal_hold_active` (boolean) — required (default false)
- `value_estimate` (decimal) — optional
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `matter_id`
**Unique Constraints:** `(firm_entity_id, matter_number)`

**Relationships:**
- Belongs to one FirmEntity, Office, Contact, MatterType, PracticeArea, Jurisdiction
- Originated from one Lead (optional)
- Assigned to one Attorney (User) and optionally one Paralegal (User)
- Has one WorkflowTemplate (determines posture phases)
- Has many Tasks, Documents, Deadlines, FilingEvents, Invoices
- Has one TrustLedger (optional; created when trust funds are deposited)
- Has many MatterRiskSignals
- Has many MatterPostureSummaries (versioned)
- Has many CommunicationTimelineItems

**Lifecycle States:**
```
engaged → in_progress → client_review → delivery → closed
```

**Audit Requirements:**
- Log: creation, stage transitions, attorney/paralegal reassignments, fee arrangement changes, ethical wall creation/modification, close events, legal hold changes

---

### MatterType

**Purpose:** Classifies the specific type of matter within a practice area (e.g., Estate Planning → Comprehensive Estate Plan with Trust; Estate Administration → Probate Pathway).

**Key Attributes:**
- `matter_type_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `practice_area_id` (FK → PracticeArea) — required
- `name` (string) — required
- `code` (string) — required
- `workflow_template_id` (FK → WorkflowTemplate) — required
- `default_deadline_rules` (FK[] → DeadlineRule) — optional
- `default_required_artifacts` (FK[] → RequiredArtifactRule) — optional
- `is_active` (boolean) — required

**Primary Key:** `matter_type_id`
**Unique Constraints:** `(firm_entity_id, code)`

**Relationships:**
- Belongs to one PracticeArea
- References one WorkflowTemplate
- Has many default DeadlineRules and RequiredArtifactRules

---

### WorkflowStage

**Purpose:** A single phase within a matter's workflow template (e.g., Intake, Consult, Scope/Price, Drafting, Review, Execution, Delivery). Ordered within a template.

**Key Attributes:**
- `stage_id` (PK, ULID) — required
- `workflow_template_id` (FK → WorkflowTemplate) — required
- `name` (string; internal label) — required
- `client_facing_label` (string; client-friendly label for portal) — required
- `sequence_order` (integer) — required
- `is_gated` (boolean) — required (true = prerequisites must be met before entering)
- `gate_conditions` (JSON; list of prerequisites: required artifacts, prior stage completion, etc.) — optional
- `owner_role` (enum: attorney, paralegal, intake, system) — optional (default owner)
- `estimated_duration_days` (integer) — optional

**Primary Key:** `stage_id`
**Unique Constraints:** `(workflow_template_id, sequence_order)`

**Relationships:**
- Belongs to one WorkflowTemplate
- Has many Tasks (via TaskTemplate)

---

### Task

**Purpose:** A unit of work within a matter, assigned to a user, with due date and dependency tracking.

**Key Attributes:**
- `task_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `matter_id` (FK → Matter) — required
- `stage_id` (FK → WorkflowStage) — optional
- `name` (string) — required
- `description` (text) — optional
- `owner_user_id` (FK → User) — required
- `status` (enum: pending, in_progress, completed, deferred, cancelled) — required
- `priority` (enum: low, medium, high, critical) — required
- `due_date` (date) — optional
- `due_time` (time) — optional
- `dependency_task_ids` (FK[] → Task) — optional (tasks that must complete first)
- `dependency_status` (enum: ready, pending, blocked) — computed
- `is_ai_suggested` (boolean) — required (default false)
- `ai_suggestion_basis` (text) — optional
- `completed_at` (timestamp) — optional
- `completed_by_user_id` (FK → User) — optional
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `task_id`

**Relationships:**
- Belongs to one Matter
- Optionally belongs to one WorkflowStage
- Assigned to one User (owner)
- Depends on zero or more other Tasks

**Lifecycle States:** pending → in_progress → completed | deferred | cancelled

**Audit Requirements:**
- Log: creation, status changes, reassignment, completion, AI suggestion acceptance/rejection

---

### TaskTemplate

**Purpose:** A reusable task definition within a workflow template. Instantiated into Tasks when a matter is created with that workflow.

**Key Attributes:**
- `template_id` (PK, ULID) — required
- `workflow_template_id` (FK → WorkflowTemplate) — required
- `stage_id` (FK → WorkflowStage) — required
- `name` (string) — required
- `description` (text) — optional
- `default_owner_role` (enum: attorney, paralegal, intake, billing) — required
- `default_duration_days` (integer) — optional
- `dependency_template_ids` (FK[] → TaskTemplate) — optional
- `sequence_order` (integer) — required

**Primary Key:** `template_id`

**Relationships:**
- Belongs to one WorkflowTemplate and one WorkflowStage
- Instantiated into Task entities per matter

---

### Deadline

**Purpose:** A time-bound obligation on a matter — statutory, regulatory, or internal. Statutory deadlines are compliance-critical and cannot be deleted by non-CLO users.

**Key Attributes:**
- `deadline_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `matter_id` (FK → Matter) — required
- `deadline_rule_id` (FK → DeadlineRule) — optional (if generated from a rule)
- `name` (string) — required
- `type` (enum: statutory, filing, regulatory, internal, sla) — required
- `due_date` (date) — required
- `due_time` (time) — optional
- `status` (enum: upcoming, at_risk, overdue, met, waived) — required
- `is_statutory` (boolean) — required
- `can_delete` (boolean) — computed (false for statutory unless CLO)
- `completed_at` (timestamp) — optional
- `completed_by_user_id` (FK → User) — optional
- `waived_by_user_id` (FK → User) — optional (CLO only for statutory)
- `waived_reason` (text) — optional
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `deadline_id`

**Relationships:**
- Belongs to one Matter
- Optionally derived from one DeadlineRule

**Lifecycle States:** upcoming → at_risk (< 7 days) → overdue (past due) → met | waived

**Audit Requirements:**
- Log: creation, due date changes, status transitions, waiver events (CLO only, with reason), deletion attempts — HIGHEST sensitivity for statutory deadlines

---

### DeadlineRule

**Purpose:** A jurisdiction-specific and practice-area-specific rule template that generates deadlines when a matter is created or reaches a triggering event. Versioned with effective dates.

**Key Attributes:**
- `rule_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `jurisdiction_id` (FK → Jurisdiction) — required
- `practice_area_id` (FK → PracticeArea) — optional (null = all)
- `matter_type_id` (FK → MatterType) — optional
- `name` (string) — required
- `description` (text) — required
- `deadline_type` (enum: statutory, filing, regulatory, internal) — required
- `trigger_event` (string; e.g., "matter_created", "death_date", "appointment_date") — required
- `offset_days` (integer) — required (days from trigger event)
- `offset_direction` (enum: after, before) — required
- `is_business_days` (boolean) — required
- `effective_date` (date) — required
- `end_date` (date) — optional (null = currently active)
- `version` (integer) — required
- `created_at` (timestamp) — required

**Primary Key:** `rule_id`
**Unique Constraints:** `(jurisdiction_id, name, version)`

**Relationships:**
- Scoped to one Jurisdiction, optionally one PracticeArea and MatterType
- Generates many Deadlines

**Audit Requirements:**
- Log: creation, modification, version changes — CLO/COO controlled

---

### RequiredArtifactRule

**Purpose:** Defines documents or artifacts that must be present at specific matter stages. Jurisdiction-scoped and versioned.

**Key Attributes:**
- `rule_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `jurisdiction_id` (FK → Jurisdiction) — required
- `practice_area_id` (FK → PracticeArea) — optional
- `matter_type_id` (FK → MatterType) — optional
- `document_type_id` (FK → DocumentType) — required
- `required_at_stage_id` (FK → WorkflowStage) — required
- `is_blocking` (boolean) — required (true = matter cannot progress without it)
- `escalation_hours` (integer) — optional (hours after which absence escalates to 🔴)
- `effective_date` (date) — required
- `end_date` (date) — optional
- `version` (integer) — required
- `created_at` (timestamp) — required

**Primary Key:** `rule_id`

**Relationships:**
- Scoped to Jurisdiction, optionally PracticeArea and MatterType
- References one DocumentType
- References one WorkflowStage (where artifact is required)

**Audit Requirements:**
- Log: creation, modification, version changes

---

### FilingEvent

**Purpose:** Records a court filing, regulatory submission, or other official document event with status tracking.

**Key Attributes:**
- `filing_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `matter_id` (FK → Matter) — required
- `name` (string; e.g., "Petition for Probate") — required
- `filing_type` (enum: court_filing, regulatory_submission, recording, certification) — required
- `jurisdiction_id` (FK → Jurisdiction) — required
- `filed_date` (date) — optional
- `filed_by_user_id` (FK → User) — optional
- `document_id` (FK → Document) — optional
- `confirmation_number` (string) — optional
- `status` (enum: pending, filed, accepted, rejected) — required
- `notes` (text) — optional
- `created_at` (timestamp) — required

**Primary Key:** `filing_id`

**Relationships:**
- Belongs to one Matter
- Optionally references one Document (the filed document)

**Audit Requirements:**
- Log: creation, status changes, filing confirmation

---

### MatterRiskSignal

**Purpose:** A system-generated or user-flagged risk indicator on a matter. Aggregated in the risk panel on the Matter Workspace.

**Key Attributes:**
- `signal_id` (PK, ULID) — required
- `matter_id` (FK → Matter) — required
- `type` (enum: missing_artifact, deadline_risk, retainer_low, retainer_depleted, responsiveness_breach, compliance_block, ethical_wall) — required
- `severity` (enum: info, warning, critical) — required
- `description` (text) — required
- `related_entity_type` (string; e.g., "deadline", "document", "retainer") — optional
- `related_entity_id` (ULID) — optional
- `suggested_action` (text) — optional
- `is_resolved` (boolean) — required (default false)
- `resolved_at` (timestamp) — optional
- `resolved_by_user_id` (FK → User) — optional
- `created_at` (timestamp) — required

**Primary Key:** `signal_id`

**Relationships:**
- Belongs to one Matter
- Optionally references a related entity (Deadline, Document, RetainerAccount)

---

### MatterPostureSummary

**Purpose:** A versioned snapshot of a matter's posture (all phases, statuses, and ETAs) for historical tracking and client-portal display.

**Key Attributes:**
- `summary_id` (PK, ULID) — required
- `matter_id` (FK → Matter) — required
- `version` (integer, auto-increment per matter) — required
- `posture_data` (JSON; array of phase objects: name, status, owner, sub-tasks, ETA) — required
- `overall_status` (enum: on_track, at_risk, delayed, blocked) — required
- `estimated_completion_date` (date) — optional
- `generated_at` (timestamp) — required
- `generated_by` (enum: system, user) — required

**Primary Key:** `summary_id`
**Unique Constraints:** `(matter_id, version)`

**Relationships:**
- Belongs to one Matter

**Audit Requirements:**
- Not individually audited (system-generated); the creating event is logged in the matter timeline
