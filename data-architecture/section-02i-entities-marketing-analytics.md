# SECTION 2 — CANONICAL ENTITY LIST (continued)

## Part I: Marketing & Content

---

### ContentItem

**Purpose:** A piece of marketing content (blog post, newsletter, social media post) managed through the content calendar with an approval workflow.

**Key Attributes:**
- `content_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `title` (string) — required
- `content_type` (enum: blog_post, newsletter, social_media, whitepaper, video, other) — required
- `practice_area_id` (FK → PracticeArea) — optional
- `status` (enum: ai_draft, in_draft, in_review, approved, scheduled, published, archived) — required
- `author_user_id` (FK → User; Marketing) — required
- `reviewer_user_id` (FK → User; Attorney for review step) — optional
- `approver_user_id` (FK → User; CLO for final approval) — optional
- `scheduled_publish_date` (date) — optional
- `scheduled_publish_time` (time) — optional
- `published_at` (timestamp) — optional
- `is_ai_drafted` (boolean) — required (default false)
- `ai_topic_suggestion_basis` (text) — optional (e.g., "High intake volume for estate planning")
- `contains_legal_conclusions` (boolean) — required (default false; set during attorney review)
- `campaign_id` (FK → Campaign) — optional
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `content_id`

**Relationships:**
- Belongs to one FirmEntity
- Has many ContentDrafts (versions)
- Has many ContentApprovals (approval workflow events)
- May have one ContentPublicationEvent

**Lifecycle States:** ai_draft → in_draft → in_review → approved → scheduled → published → archived

**Audit Requirements:**
- Log: creation, status transitions, approval/rejection events, publication — full approval workflow audit trail per FSD

---

### ContentDraft

**Purpose:** A version of a content item. Each edit creates a new draft version.

**Key Attributes:**
- `draft_id` (PK, ULID) — required
- `content_id` (FK → ContentItem) — required
- `version_number` (integer) — required
- `body` (text; rich text content) — required
- `edited_by_user_id` (FK → User) — required
- `is_ai_generated` (boolean) — required
- `ai_model_version` (string) — optional
- `created_at` (timestamp) — required

**Primary Key:** `draft_id`
**Unique Constraints:** `(content_id, version_number)`

---

### ContentApproval

**Purpose:** A single approval or rejection event in the content workflow. Maps to the approval steps: Marketing edit → Attorney review → CLO approval.

**Key Attributes:**
- `approval_id` (PK, ULID) — required
- `content_id` (FK → ContentItem) — required
- `step` (enum: marketing_review, attorney_review, clo_approval) — required
- `decision` (enum: approved, rejected) — required
- `reviewer_user_id` (FK → User) — required
- `notes` (text) — optional
- `legal_conclusions_flagged` (boolean) — required (default false; attorney sets this during review)
- `decided_at` (timestamp) — required

**Primary Key:** `approval_id`

**Audit Requirements:**
- Log: every approval and rejection event

---

### ContentPublicationEvent

**Purpose:** Records the publication of a content item with metadata for attribution tracking.

**Key Attributes:**
- `event_id` (PK, ULID) — required
- `content_id` (FK → ContentItem) — required
- `published_url` (string) — optional
- `platform` (enum: website, email, social_facebook, social_linkedin, social_instagram, other) — required
- `published_by_user_id` (FK → User) — required
- `published_at` (timestamp) — required

**Primary Key:** `event_id`

---

### ContentAttributionEvent

**Purpose:** Links a lead to the content item or campaign that generated it. Used for marketing ROI calculation.

**Key Attributes:**
- `attribution_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `lead_id` (FK → Lead) — required
- `content_id` (FK → ContentItem) — optional
- `campaign_id` (FK → Campaign) — optional
- `source_channel` (enum: website_seo, google_ads, social_media, newsletter, referral, public_signal, direct, other) — required
- `attribution_model` (enum: first_touch, last_touch, linear) — required (default: first_touch)
- `attributed_at` (timestamp) — required

**Primary Key:** `attribution_id`

**Relationships:**
- Links one Lead to optional ContentItem and/or Campaign

---

### PublicSignalLead

**Purpose:** A potential lead identified from public sources (obituaries, court records) per FSD. Subject to strict ethics and compliance controls: prominent disclaimer, opt-out support, no automated outreach.

**Key Attributes:**
- `signal_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `source_type` (enum: obituary, court_record, public_notice) — required
- `source_name` (string; publication name) — required
- `source_date` (date) — required
- `summary` (text) — required
- `geographic_area` (string; county/city) — optional
- `status` (enum: new, flagged_for_review, dismissed, opted_out, converted_to_lead) — required
- `flagged_by_user_id` (FK → User) — optional
- `compliance_reviewed` (boolean) — required (default false)
- `compliance_reviewer_user_id` (FK → User; CLO) — optional
- `lead_id` (FK → Lead) — optional (if converted)
- `opt_out_contact_id` (FK → Contact) — optional
- `opted_out_at` (timestamp) — optional
- `ethics_disclaimer_acknowledged` (boolean) — required (default false; must be true before any action)
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `signal_id`

**Invariants:**
- No automated outreach may be triggered from this entity
- Opt-out is immediate and permanent
- All conversions to Lead require explicit user action and compliance review

**Lifecycle States:** new → flagged_for_review → dismissed | opted_out | converted_to_lead

**Audit Requirements:**
- Log: creation, flag events, compliance review, opt-out events, conversion events — COMPLIANCE CRITICAL due to bar ethics rules

---

## Part J: Analytics & Observability

---

### KPIMetricDefinition

**Purpose:** Defines a KPI with its name, formula, data sources, and refresh cadence. Used by the analytics engine to compute KPI values.

**Key Attributes:**
- `metric_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string; e.g., "Collections Rate") — required
- `code` (string; e.g., "COLLECTIONS_RATE") — required
- `description` (text) — required
- `formula` (text; human-readable formula) — required
- `data_sources` (JSON; entities and fields used) — required
- `unit` (enum: currency, percentage, count, days, hours, ratio) — required
- `direction_is_positive` (enum: up, down, neutral) — required (e.g., "up" for revenue, "down" for cycle time)
- `refresh_cadence` (enum: real_time, hourly, daily, weekly, monthly) — required
- `drilldown_dimensions` (string[]; e.g., ["office", "practice_area", "attorney"]) — required
- `dashboard_ids` (FK[] → Dashboard) — optional
- `is_active` (boolean) — required

**Primary Key:** `metric_id`
**Unique Constraints:** `(firm_entity_id, code)`

---

### KPIComputation

**Purpose:** A computed value for a KPI at a specific point in time, for a specific scope.

**Key Attributes:**
- `computation_id` (PK, ULID) — required
- `metric_id` (FK → KPIMetricDefinition) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — optional (null = firmwide)
- `practice_area_id` (FK → PracticeArea) — optional
- `attorney_user_id` (FK → User) — optional
- `period_start` (date) — required
- `period_end` (date) — required
- `value` (decimal) — required
- `prior_period_value` (decimal) — optional
- `trend` (enum: up, down, flat) — optional
- `trend_percentage` (decimal) — optional
- `computed_at` (timestamp) — required

**Primary Key:** `computation_id`

---

### Dashboard

**Purpose:** A named dashboard configuration (e.g., "Firmwide Executive Dashboard", "Office Dashboard: Main Street").

**Key Attributes:**
- `dashboard_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string) — required
- `type` (enum: firmwide_executive, office_operations, billing, accounting, marketing, personal) — required
- `visible_to_roles` (enum[]; e.g., [CLO, CEO, COO, CFO]) — required
- `is_default_for_role` (JSON; role → boolean mapping) — optional

**Primary Key:** `dashboard_id`

**Relationships:**
- Has many DashboardWidgets

---

### DashboardWidget

**Purpose:** A widget on a dashboard displaying a specific KPI, chart, or data panel.

**Key Attributes:**
- `widget_id` (PK, ULID) — required
- `dashboard_id` (FK → Dashboard) — required
- `metric_id` (FK → KPIMetricDefinition) — optional
- `widget_type` (enum: kpi_card, chart_line, chart_bar, chart_funnel, table, heat_map, alert_list) — required
- `title` (string) — required
- `position` (JSON; {row, col, width, height}) — required
- `configuration` (JSON; visualization options, filters, etc.) — optional

**Primary Key:** `widget_id`

---

### AuditLog

**Purpose:** Immutable, append-only log of all auditable events across the platform. The single source of truth for compliance, security, and forensic analysis.

**Key Attributes:**
- `log_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `event_category` (string; e.g., "identity", "matter", "trust", "billing", "compliance") — required
- `event_type` (string; e.g., "user.login", "matter.stage_transition", "trust.transfer_approved") — required
- `actor_user_id` (FK → User) — optional (null for system events)
- `actor_role` (string) — optional
- `actor_ip_address` (string) — optional
- `entity_type` (string; e.g., "matter", "invoice", "trust_ledger") — required
- `entity_id` (ULID) — required
- `action` (string; e.g., "create", "update", "approve", "delete_attempt") — required
- `before_state` (JSON) — optional (previous state of changed fields)
- `after_state` (JSON) — optional (new state of changed fields)
- `reason` (text) — optional (for overrides, approvals, denials)
- `source_screen` (string; e.g., "screen_4_intake_console", "screen_11_trust") — optional
- `metadata` (JSON) — optional (additional context)
- `occurred_at` (timestamp) — required
- `created_at` (timestamp) — required

**Primary Key:** `log_id`

**Invariants:**
- APPEND-ONLY. No update or delete operations permitted on this table.
- Retention: 7 years minimum; no auto-deletion.
- Must be exportable with chain-of-custody metadata (hash, exporter, timestamp).

---

### SystemEvent

**Purpose:** A domain event published to the internal event bus for cross-domain coordination. Not persisted long-term like AuditLog — used for real-time processing and eventual consistency.

**Key Attributes:**
- `event_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `event_type` (string; e.g., "lead.qualified", "appointment.confirmed", "trust.transfer_executed") — required
- `source_domain` (string; e.g., "crm", "scheduling", "trust") — required
- `payload` (JSON) — required
- `published_at` (timestamp) — required
- `processed` (boolean) — required (default false)

**Primary Key:** `event_id`

---

### RiskAlert

**Purpose:** A system-generated alert for operational or compliance risk that should be surfaced on dashboards and notification feeds.

**Key Attributes:**
- `alert_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `type` (enum: deadline_risk, capacity_gap, retainer_low, ar_aging, sla_breach, trust_reconciliation_overdue, missing_artifact, compliance_block) — required
- `severity` (enum: info, warning, critical) — required
- `title` (string) — required
- `description` (text) — required
- `related_entity_type` (string) — optional
- `related_entity_id` (ULID) — optional
- `office_id` (FK → Office) — optional
- `matter_id` (FK → Matter) — optional
- `target_roles` (enum[]) — required (which roles should see this alert)
- `target_user_ids` (FK[] → User) — optional (specific users)
- `is_dismissible` (boolean) — required (false for compliance alerts)
- `is_dismissed` (boolean) — required (default false)
- `dismissed_by_user_id` (FK → User) — optional
- `is_resolved` (boolean) — required (default false)
- `resolved_at` (timestamp) — optional
- `created_at` (timestamp) — required

**Primary Key:** `alert_id`

**Relationships:**
- May reference a Matter, Office, or other entity

---

### RemediationAction

**Purpose:** A suggested or taken action to resolve a RiskAlert.

**Key Attributes:**
- `action_id` (PK, ULID) — required
- `alert_id` (FK → RiskAlert) — required
- `description` (string) — required
- `action_type` (enum: navigate_to_screen, send_reminder, request_document, escalate, system_auto) — required
- `target_url` (string) — optional
- `is_completed` (boolean) — required (default false)
- `completed_by_user_id` (FK → User) — optional
- `completed_at` (timestamp) — optional

**Primary Key:** `action_id`

---

### Notification

**Purpose:** A user-facing notification delivered via the notification bell, email, or push (configurable). Links to a source event.

**Key Attributes:**
- `notification_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `recipient_user_id` (FK → User) — required
- `title` (string) — required
- `body` (text) — required
- `priority` (enum: low, normal, high, critical) — required
- `channel` (enum: in_app, email, both) — required
- `source_event_type` (string) — optional
- `source_entity_type` (string) — optional
- `source_entity_id` (ULID) — optional
- `deep_link_url` (string) — optional
- `is_read` (boolean) — required (default false)
- `read_at` (timestamp) — optional
- `created_at` (timestamp) — required

**Primary Key:** `notification_id`

**Relationships:**
- Belongs to one User (recipient)
