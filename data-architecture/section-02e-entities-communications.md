# SECTION 2 — CANONICAL ENTITY LIST (continued)

## Part E: Communications (Embedded UCaaS — Office Main Number + IVR Only)

---

### OfficeMainNumber

**Purpose:** The primary phone number for an office. All calls route through this number's IVR. Per FSD: per-office main number only — no per-user DDI.

**Key Attributes:**
- `main_number_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — required
- `phone_number` (string; E.164 format) — required
- `display_name` (string; e.g., "Main Street Office") — required
- `call_flow_id` (FK → CallFlow) — required
- `is_active` (boolean) — required
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `main_number_id`
**Unique Constraints:** `(firm_entity_id, office_id)` — one main number per office; `(phone_number)` globally unique

**Relationships:**
- Belongs to one Office
- Has one CallFlow

**Audit Requirements:**
- Log: creation, number changes, activation/deactivation

---

### CallFlow

**Purpose:** Defines the inbound call routing logic for an office main number: IVR menu, business hours, after-hours behavior, and overflow handling.

**Key Attributes:**
- `call_flow_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — required
- `ivr_menu_id` (FK → IVRMenu) — required
- `after_hours_rule_id` (FK → AfterHoursRule) — required
- `overflow_rule_id` (FK → OverflowRule) — optional
- `business_hours` (JSON; inherited from or overriding Office) — required
- `holiday_schedule_id` (FK → HolidaySchedule) — optional
- `is_active` (boolean) — required
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `call_flow_id`

**Relationships:**
- Belongs to one Office
- References one IVRMenu, one AfterHoursRule, optionally one OverflowRule

**Audit Requirements:**
- Log: all configuration changes (CLO/COO only)

---

### IVRMenu

**Purpose:** The interactive voice response menu presented to callers when they reach an office main number.

**Key Attributes:**
- `ivr_menu_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — required
- `greeting_text` (text) — required
- `options` (FK[] → IVROption; ordered list) — required
- `timeout_seconds` (integer; default 10) — required
- `timeout_action` (enum: repeat, transfer_operator, voicemail) — required
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `ivr_menu_id`

**Relationships:**
- Belongs to one Office
- Has many IVROptions (ordered)

---

### IVROption

**Purpose:** A single menu option within an IVR (e.g., "Press 1 for New Client Intake").

**Key Attributes:**
- `option_id` (PK, ULID) — required
- `ivr_menu_id` (FK → IVRMenu) — required
- `key_press` (string; "1", "2", "3", "0") — required
- `label` (string; e.g., "New Client Intake") — required
- `announcement_text` (text) — required
- `target_queue_id` (FK → Queue) — required
- `sequence_order` (integer) — required

**Primary Key:** `option_id`
**Unique Constraints:** `(ivr_menu_id, key_press)`

**Relationships:**
- Belongs to one IVRMenu
- Routes to one Queue

---

### Queue

**Purpose:** A call queue that holds callers waiting for an available agent. Each IVR option routes to a queue.

**Key Attributes:**
- `queue_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — required
- `name` (string; e.g., "New Client Intake", "Existing Client", "Billing") — required
- `ring_group_id` (FK → RingGroup) — required
- `max_wait_seconds` (integer) — optional
- `hold_music_enabled` (boolean) — required (default true)
- `estimated_wait_announcement` (boolean) — required (default true)
- `sla_target_seconds` (integer; e.g., 30) — required
- `sla_target_percentage` (integer; e.g., 80 = 80% answered within target) — required
- `abandoned_threshold_seconds` (integer) — optional
- `created_at` (timestamp) — required

**Primary Key:** `queue_id`
**Unique Constraints:** `(office_id, name)`

**Relationships:**
- Belongs to one Office
- Referenced by IVROption (routing target)
- Has one RingGroup

---

### RingGroup

**Purpose:** A group of agents (users) who can answer calls from a specific queue.

**Key Attributes:**
- `ring_group_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `queue_id` (FK → Queue) — required
- `member_user_ids` (FK[] → User; via RingGroupMember) — required
- `ring_strategy` (enum: round_robin, longest_idle, simultaneous) — required

**Primary Key:** `ring_group_id`

**Relationships:**
- Belongs to one Queue
- Has many Users (members)

---

### RoutingRule

**Purpose:** Conditional routing logic applied to inbound calls (e.g., route known client numbers to their assigned attorney's voicemail after hours).

**Key Attributes:**
- `rule_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — required
- `name` (string) — required
- `condition` (JSON; matching criteria: caller_number, time_of_day, caller_id_match) — required
- `action` (enum: route_to_queue, route_to_user, voicemail, callback) — required
- `target_id` (ULID; queue_id or user_id depending on action) — required
- `priority` (integer) — required
- `is_active` (boolean) — required

**Primary Key:** `rule_id`

**Relationships:**
- Belongs to one Office

---

### AfterHoursRule

**Purpose:** Defines what happens to calls received outside of business hours.

**Key Attributes:**
- `rule_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — required
- `action` (enum: voicemail, auto_email, forward_external, announcement_only) — required
- `voicemail_greeting_text` (text) — optional
- `auto_email_address` (string) — optional (e.g., intake@firm.com)
- `forward_number` (string) — optional
- `announcement_text` (text) — optional

**Primary Key:** `rule_id`

---

### OverflowRule

**Purpose:** Defines overflow behavior when queue wait exceeds thresholds or all agents are busy.

**Key Attributes:**
- `rule_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `queue_id` (FK → Queue) — required
- `trigger_wait_seconds` (integer) — required
- `action` (enum: voicemail, callback_offer, overflow_queue, announcement) — required
- `overflow_queue_id` (FK → Queue) — optional
- `callback_enabled` (boolean) — required

**Primary Key:** `rule_id`

---

### DirectoryEntry

**Purpose:** Represents a user's entry in the firmwide phone directory with real-time presence status.

**Key Attributes:**
- `entry_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `user_id` (FK → User) — required
- `display_name` (string) — required
- `office_id` (FK → Office) — required
- `role_label` (string) — required
- `extension` (string) — required
- `presence_status` (enum: available, on_call, away, dnd, ooo) — required (real-time via UCaaS integration)

**Primary Key:** `entry_id`
**Unique Constraints:** `(firm_entity_id, extension)`

**Relationships:**
- Belongs to one User and one Office

---

### CallEvent

**Purpose:** A real-time event during a call lifecycle (ring, answer, hold, transfer, end). Used for live queue monitoring and call analytics.

**Key Attributes:**
- `event_id` (PK, ULID) — required
- `call_record_id` (FK → CallRecord) — required
- `event_type` (enum: inbound_ring, answered, hold_start, hold_end, transfer_initiated, transfer_completed, mute, unmute, ended, voicemail_start, voicemail_end) — required
- `actor_user_id` (FK → User) — optional
- `metadata` (JSON; transfer_to, hold_duration, etc.) — optional
- `occurred_at` (timestamp) — required

**Primary Key:** `event_id`

**Relationships:**
- Belongs to one CallRecord

---

### CallRecord

**Purpose:** Metadata record for a completed or in-progress phone call. Links caller to client/matter context. Does NOT store audio — that's in RecordingArtifact.

**Key Attributes:**
- `call_record_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — required
- `direction` (enum: inbound, outbound, internal) — required
- `caller_number` (string) — required
- `called_number` (string) — required
- `ivr_option_selected` (string) — optional
- `queue_id` (FK → Queue) — optional
- `answered_by_user_id` (FK → User) — optional
- `transferred_to_user_id` (FK → User) — optional
- `matched_contact_id` (FK → Contact) — optional (caller ID match)
- `matched_matter_id` (FK → Matter) — optional
- `start_time` (timestamp) — required
- `answer_time` (timestamp) — optional
- `end_time` (timestamp) — optional
- `duration_seconds` (integer) — optional
- `wait_time_seconds` (integer) — optional (queue wait)
- `status` (enum: in_progress, completed, missed, abandoned, voicemail) — required
- `disposition` (enum: resolved, callback_scheduled, transferred, no_action, voicemail_left) — optional
- `notes` (text) — optional (agent notes during call)
- `ai_summary` (text) — optional
- `ai_summary_approved` (boolean) — required (default false)
- `ai_summary_approved_by` (FK → User) — optional
- `ai_summary_approved_at` (timestamp) — optional
- `recording_artifact_id` (FK → RecordingArtifact) — optional
- `transcript_artifact_id` (FK → TranscriptArtifact) — optional
- `consent_record_id` (FK → ConsentRecord) — optional
- `created_at` (timestamp) — required

**Primary Key:** `call_record_id`

**Relationships:**
- Belongs to one Office
- Matched to one Contact and/or Matter (if caller ID resolves)
- Has many CallEvents
- Optionally has one RecordingArtifact, one TranscriptArtifact, one ConsentRecord

**Audit Requirements:**
- Log: creation, AI summary approval events, recording access events

---

### VoicemailArtifact

**Purpose:** Stores metadata for a voicemail message left by a caller.

**Key Attributes:**
- `voicemail_id` (PK, ULID) — required
- `call_record_id` (FK → CallRecord) — required
- `storage_path` (string) — required
- `duration_seconds` (integer) — required
- `transcription` (text) — optional (AI-transcribed)
- `is_listened` (boolean) — required (default false)
- `listened_by_user_id` (FK → User) — optional
- `created_at` (timestamp) — required

**Primary Key:** `voicemail_id`

---

### RecordingArtifact

**Purpose:** Stores metadata and reference to a call recording file. Subject to consent and retention rules.

**Key Attributes:**
- `recording_id` (PK, ULID) — required
- `call_record_id` (FK → CallRecord) — required
- `storage_path` (string; encrypted storage reference) — required
- `duration_seconds` (integer) — required
- `file_size_bytes` (bigint) — required
- `consent_record_id` (FK → ConsentRecord) — required
- `retention_expires_at` (timestamp) — required
- `is_deleted` (boolean) — required (default false; true after retention expires)
- `created_at` (timestamp) — required

**Primary Key:** `recording_id`

**Relationships:**
- Belongs to one CallRecord
- Requires one ConsentRecord

**Audit Requirements:**
- Log: creation, access events (who listened), deletion

---

### TranscriptArtifact

**Purpose:** AI-generated transcription of a call recording. Subject to same consent and retention rules as the recording.

**Key Attributes:**
- `transcript_id` (PK, ULID) — required
- `call_record_id` (FK → CallRecord) — required
- `recording_id` (FK → RecordingArtifact) — required
- `content` (text) — required
- `model_version` (string) — required
- `confidence_score` (decimal) — optional
- `created_at` (timestamp) — required

**Primary Key:** `transcript_id`

---

### ConsentRecord

**Purpose:** Records whether recording/transcription consent was obtained for a call, per jurisdiction-specific consent laws (one-party vs. two-party).

**Key Attributes:**
- `consent_id` (PK, ULID) — required
- `call_record_id` (FK → CallRecord) — required
- `jurisdiction_id` (FK → Jurisdiction) — required
- `consent_type_required` (enum: one_party, two_party) — required
- `consent_obtained` (boolean) — required
- `consent_method` (enum: ivr_announcement, agent_verbal, pre_agreed) — optional
- `agent_user_id` (FK → User) — optional
- `obtained_at` (timestamp) — optional

**Primary Key:** `consent_id`

**Audit Requirements:**
- Log: creation, any modification — COMPLIANCE CRITICAL

---

### SMSMessage

**Purpose:** An outbound or inbound SMS message, typically used for appointment confirmations, reminders, and notifications.

**Key Attributes:**
- `sms_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `direction` (enum: outbound, inbound) — required
- `from_number` (string) — required
- `to_number` (string) — required
- `body` (text) — required
- `purpose` (enum: appointment_confirmation, appointment_reminder, payment_reminder, general_notification) — required
- `related_entity_type` (string; e.g., "appointment", "matter") — optional
- `related_entity_id` (ULID) — optional
- `status` (enum: queued, sent, delivered, failed, bounced) — required
- `provider_message_id` (string) — optional
- `failure_reason` (string) — optional
- `retry_count` (integer; default 0) — required
- `sent_at` (timestamp) — optional
- `delivered_at` (timestamp) — optional
- `created_at` (timestamp) — required

**Primary Key:** `sms_id`

---

### EmailMessage

**Purpose:** An outbound or inbound email message linked to a matter, lead, or system event.

**Key Attributes:**
- `email_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `direction` (enum: outbound, inbound) — required
- `from_address` (string) — required
- `to_addresses` (string[]) — required
- `cc_addresses` (string[]) — optional
- `subject` (string) — required
- `body_html` (text) — required
- `body_text` (text) — optional
- `purpose` (enum: appointment_confirmation, appointment_reminder, document_request, invoice, dunning, general) — required
- `related_entity_type` (string) — optional
- `related_entity_id` (ULID) — optional
- `status` (enum: queued, sent, delivered, bounced, failed) — required
- `provider_message_id` (string) — optional
- `failure_reason` (string) — optional
- `sent_at` (timestamp) — optional
- `created_at` (timestamp) — required

**Primary Key:** `email_id`

---

### PortalMessage

**Purpose:** A secure message exchanged between the client (via Client Portal) and the firm staff. Per FSD: all outbound messages require staff approval before delivery.

**Key Attributes:**
- `message_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `matter_id` (FK → Matter) — required
- `sender_user_id` (FK → User) — required (client or staff user)
- `sender_type` (enum: client, staff) — required
- `body` (text) — required
- `attachment_document_ids` (FK[] → Document) — optional
- `is_read` (boolean) — required (default false)
- `read_at` (timestamp) — optional
- `staff_approved` (boolean) — required (default false for staff outbound; auto-true for client inbound)
- `approved_by_user_id` (FK → User) — optional
- `approved_at` (timestamp) — optional
- `sla_response_target` (enum: standard, urgent) — required
- `sla_response_deadline` (timestamp) — required (calculated from creation + SLA rules)
- `sla_breached` (boolean) — required (default false)
- `created_at` (timestamp) — required

**Primary Key:** `message_id`

**Relationships:**
- Belongs to one Matter
- Sent by one User
- May have attached Documents

**Audit Requirements:**
- Log: creation, approval events (for outbound), read events, SLA breach events

---

### CommunicationTimelineItem

**Purpose:** Unified abstraction that aggregates all communication events into a single chronological timeline per matter. Polymorphic reference to the source entity.

**Key Attributes:**
- `timeline_item_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `matter_id` (FK → Matter) — optional (null for pre-engagement lead communications)
- `lead_id` (FK → Lead) — optional
- `channel` (enum: phone, sms, email, portal_message, in_person, video, system) — required
- `direction` (enum: inbound, outbound, internal, system) — required
- `source_entity_type` (string; "call_record", "sms_message", "email_message", "portal_message", "task", "document", "payment", "note") — required
- `source_entity_id` (ULID) — required
- `summary` (text) — required
- `actor_user_id` (FK → User) — optional
- `is_ai_generated_summary` (boolean) — required (default false)
- `ai_summary_approved` (boolean) — optional
- `occurred_at` (timestamp) — required
- `created_at` (timestamp) — required

**Primary Key:** `timeline_item_id`

**Relationships:**
- Belongs to one Matter or Lead
- References one source entity (polymorphic)

---

### ResponsivenessSLA

**Purpose:** Defines response time expectations for different communication types per FSD. Used to generate SLA deadlines on portal messages and client communications.

**Key Attributes:**
- `sla_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `channel` (enum: portal_message, email, phone_callback) — required
- `priority` (enum: standard, urgent) — required
- `target_hours` (integer) — required (e.g., 24 for standard portal, 4 for urgent)
- `is_business_hours_only` (boolean) — required (default true)
- `effective_date` (date) — required
- `end_date` (date) — optional

**Primary Key:** `sla_id`

---

### SLAEvent

**Purpose:** Records an SLA evaluation event — whether a specific communication met or breached its responsiveness SLA.

**Key Attributes:**
- `sla_event_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `sla_id` (FK → ResponsivenessSLA) — required
- `source_entity_type` (string) — required
- `source_entity_id` (ULID) — required
- `matter_id` (FK → Matter) — optional
- `target_response_at` (timestamp) — required
- `actual_response_at` (timestamp) — optional
- `is_breached` (boolean) — required
- `breached_duration_minutes` (integer) — optional
- `created_at` (timestamp) — required

**Primary Key:** `sla_event_id`

**Audit Requirements:**
- Log: breach events (internal notification; NOT exposed to client per AC-14)
