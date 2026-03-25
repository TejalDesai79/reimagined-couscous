# SECTION 2 — CANONICAL ENTITY LIST (continued)

## Part F: Scheduling & Capacity

---

### Appointment

**Purpose:** A scheduled consultation or meeting between a client and an attorney. Only bookable on capacity-approved slots. Tracks the full lifecycle from booking through completion or no-show.

**Key Attributes:**
- `appointment_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — required
- `lead_id` (FK → Lead) — optional (pre-engagement consult)
- `matter_id` (FK → Matter) — optional (post-engagement meeting)
- `contact_id` (FK → Contact) — required
- `attorney_user_id` (FK → User) — required
- `appointment_type_id` (FK → AppointmentType) — required
- `practice_area_id` (FK → PracticeArea) — required
- `open_slot_id` (FK → OpenAppointmentSlot) — required (which capacity-approved slot was consumed)
- `scheduled_date` (date) — required
- `scheduled_time` (time) — required
- `duration_minutes` (integer) — required
- `location_type` (enum: in_office, video, phone) — required
- `location_detail` (string; room number or video link) — optional
- `status` (enum: pending_confirmation, confirmed, completed, no_show, cancelled, rescheduled) — required
- `consultation_fee_payment_id` (FK → ConsultationFeePayment) — optional
- `fee_status` (enum: paid, pending, waived, not_required, failed) — required
- `cancellation_reason` (text) — optional
- `cancelled_by` (enum: intake, client, system) — optional
- `cancelled_at` (timestamp) — optional
- `is_late_cancellation` (boolean) — required (default false; true if cancelled < 24 hours before)
- `no_show_record_id` (FK → NoShowRecord) — optional
- `reschedule_from_appointment_id` (FK → Appointment) — optional
- `consult_prep_data` (JSON; underwriting score, key concerns, suggested strategies, pricing) — optional
- `attorney_notes` (text) — optional
- `auto_cancel_deadline` (timestamp) — optional (per office config: default 24h from booking if fee unpaid)
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `appointment_id`

**Relationships:**
- Belongs to one Office, one Contact, one Attorney (User)
- Optionally linked to one Lead or one Matter
- Consumes one OpenAppointmentSlot
- May have one ConsultationFeePayment
- May have one NoShowRecord
- Has many AppointmentConfirmationEvents

**Lifecycle States:**
```
pending_confirmation → confirmed → completed | no_show | cancelled | rescheduled
```

**Audit Requirements:**
- Log: creation, confirmation, completion, no-show marking, cancellation (with reason and actor), rescheduling

---

### AppointmentType

**Purpose:** Classification of appointment kinds (e.g., Initial Consultation, Follow-Up, Document Signing, Estate Review).

**Key Attributes:**
- `type_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string) — required
- `code` (string) — required
- `default_duration_minutes` (integer) — required
- `requires_consultation_fee` (boolean) — required (default true for initial consult)
- `is_active` (boolean) — required

**Primary Key:** `type_id`
**Unique Constraints:** `(firm_entity_id, code)`

---

### AppointmentConfirmationEvent

**Purpose:** Records each outbound confirmation or reminder sent for an appointment, including delivery status.

**Key Attributes:**
- `event_id` (PK, ULID) — required
- `appointment_id` (FK → Appointment) — required
- `channel` (enum: email, sms) — required
- `message_type` (enum: confirmation, reminder_24h, reminder_same_day) — required
- `recipient_address` (string; email or phone number) — required
- `status` (enum: queued, sent, delivered, bounced, failed) — required
- `provider_message_id` (string) — optional
- `failure_reason` (string) — optional
- `retry_count` (integer; default 0) — required
- `sent_at` (timestamp) — optional
- `delivered_at` (timestamp) — optional
- `created_at` (timestamp) — required

**Primary Key:** `event_id`

**Relationships:**
- Belongs to one Appointment

---

### NoShowRecord

**Purpose:** Records when a client fails to attend a scheduled appointment. Feeds into underwriting scoring and scheduling analytics.

**Key Attributes:**
- `no_show_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `appointment_id` (FK → Appointment) — required
- `contact_id` (FK → Contact) — required
- `marked_by_user_id` (FK → User) — required (per FSD: cannot be automated; requires explicit user action)
- `client_type` (enum: new, returning) — required
- `source_channel` (string) — required (from lead source)
- `practice_area_id` (FK → PracticeArea) — required
- `attorney_user_id` (FK → User) — required
- `time_of_day` (string; morning, afternoon, evening) — required
- `revenue_impact_estimate` (decimal) — optional
- `created_at` (timestamp) — required

**Primary Key:** `no_show_id`

**Relationships:**
- Belongs to one Appointment and one Contact
- Referenced by underwriting model and scheduling analytics

**Audit Requirements:**
- Log: creation (who marked, when)

---

### AttorneyCapacityProfile

**Purpose:** Defines the target capacity parameters for a single attorney. Used by the capacity engine to compute utilization and manage open appointment slots.

**Key Attributes:**
- `profile_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `user_id` (FK → User; attorney) — required
- `office_id` (FK → Office) — required
- `target_weekly_hours` (integer; default 40) — required
- `max_active_matters` (integer) — required
- `complexity_weight_factor` (decimal; multiplier for complex matters) — required (default 1.0)
- `practice_area_ids` (FK[] → PracticeArea) — required
- `is_accepting_new_intake` (boolean) — required (default true)
- `temporarily_closed_to_intake` (boolean) — required (default false)
- `temporarily_closed_at` (timestamp) — optional
- `temporarily_closed_by_user_id` (FK → User) — optional (CLO)
- `pto_periods` (JSON; array of {start_date, end_date, type: PTO|OOO}) — optional
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `profile_id`
**Unique Constraints:** `(user_id)` — one profile per attorney

**Relationships:**
- Belongs to one User (attorney) and one Office
- Referenced by CapacityComputation

**Audit Requirements:**
- Log: target hour changes, max matter changes, intake closure events (CLO action)

---

### CapacityComputation

**Purpose:** A point-in-time snapshot of an attorney's computed capacity. Generated by the capacity engine on a schedule and on-demand.

**Key Attributes:**
- `computation_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `user_id` (FK → User; attorney) — required
- `profile_id` (FK → AttorneyCapacityProfile) — required
- `computed_at` (timestamp) — required
- `period_start` (date) — required
- `period_end` (date) — required
- `target_hours` (integer) — required
- `active_matter_count` (integer) — required
- `complexity_weighted_hours` (decimal) — required
- `utilization_percentage` (decimal) — required
- `deadline_pressure_count` (integer; statutory deadlines in period) — required
- `sla_commitment_hours` (decimal) — required
- `pto_hours_in_period` (decimal) — required
- `status` (enum: under_utilized, good, near_threshold, over_utilized) — required
- `open_appointment_slots_this_period` (integer) — required
- `consult_demand_this_period` (integer) — optional (from pipeline)

**Primary Key:** `computation_id`

**Relationships:**
- Belongs to one User (attorney) and one AttorneyCapacityProfile

---

### OpenAppointmentSlot

**Purpose:** A time slot approved by the capacity engine for new consultations. Only slots in this table can be booked by intake.

**Key Attributes:**
- `slot_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — required
- `attorney_user_id` (FK → User) — required
- `practice_area_id` (FK → PracticeArea) — required
- `date` (date) — required
- `start_time` (time) — required
- `end_time` (time) — required
- `duration_minutes` (integer) — required
- `status` (enum: open, booked, cancelled, closed_by_engine, closed_by_override) — required
- `booked_appointment_id` (FK → Appointment) — optional (populated when booked)
- `created_by` (enum: capacity_engine, clo_override) — required
- `override_id` (FK → CapacityOverride) — optional (if created via CLO override)
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `slot_id`
**Unique Constraints:** `(attorney_user_id, date, start_time)` — no double-booking

**Relationships:**
- Belongs to one Attorney (User) and one Office
- May be consumed by one Appointment
- May be created by one CapacityOverride

**Lifecycle States:** open → booked | cancelled | closed_by_engine | closed_by_override

**Audit Requirements:**
- Log: creation (whether by engine or CLO override), booking, closure events

---

### OpenAppointmentPolicy

**Purpose:** Configurable rules that govern how the capacity engine generates open appointment slots. CLO-controlled.

**Key Attributes:**
- `policy_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — optional (null = firmwide)
- `practice_area_id` (FK → PracticeArea) — optional
- `utilization_threshold_percent` (decimal; above this, engine closes new slots; default 85%) — required
- `min_open_slots_per_week` (integer; minimum floor even at high utilization) — optional
- `max_open_slots_per_attorney_per_week` (integer) — optional
- `slot_duration_minutes` (integer; default 60) — required
- `advance_booking_days` (integer; how far ahead to generate slots; default 14) — required
- `effective_date` (date) — required
- `end_date` (date) — optional

**Primary Key:** `policy_id`

**Audit Requirements:**
- Log: creation, threshold changes — CLO controlled

---

### CapacityOverride

**Purpose:** Records a CLO override to the capacity engine's decisions (e.g., force-opening a slot for an over-capacity attorney). All overrides are audit-logged.

**Key Attributes:**
- `override_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `attorney_user_id` (FK → User) — required
- `override_type` (enum: add_slot, close_to_intake, adjust_target_hours, reassign_matters, reopen_slot) — required
- `reason` (text) — required
- `approved_by_user_id` (FK → User; CLO) — required
- `previous_value` (JSON) — optional (state before override)
- `new_value` (JSON) — optional (state after override)
- `related_slot_id` (FK → OpenAppointmentSlot) — optional
- `related_expansion_request_id` (FK → CapacityExpansionRequest) — optional
- `created_at` (timestamp) — required

**Primary Key:** `override_id`

**Relationships:**
- Created by CLO (User)
- May relate to a specific OpenAppointmentSlot
- May relate to a CapacityExpansionRequest

**Audit Requirements:**
- Log: ALWAYS logged — this is a compliance-grade audit event. Fields: who, what, when, previous state, new state, reason

---

### CapacityExpansionRequest

**Purpose:** A request from an intake lead to the CLO for additional capacity in a practice area/office.

**Key Attributes:**
- `request_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — required
- `practice_area_id` (FK → PracticeArea) — required
- `requested_by_user_id` (FK → User; intake lead) — required
- `reason` (text) — required
- `current_gap` (integer; negative = constrained) — required
- `system_suggestion` (JSON; suggested resolution with attorney IDs and slot counts) — optional
- `status` (enum: pending, approved, modified_and_approved, denied, deferred) — required
- `decision_by_user_id` (FK → User; CLO) — optional
- `decision_reason` (text) — optional
- `decided_at` (timestamp) — optional
- `override_id` (FK → CapacityOverride) — optional (if approved, links to the override)
- `created_at` (timestamp) — required

**Primary Key:** `request_id`

**Lifecycle States:** pending → approved | modified_and_approved | denied | deferred

**Audit Requirements:**
- Log: creation, decision events (approve/deny/defer with reason)
