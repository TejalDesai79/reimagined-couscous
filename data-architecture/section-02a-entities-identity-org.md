# SECTION 2 — CANONICAL ENTITY LIST (WITH ATTRIBUTES)

## Part A: Identity & Org Structure

---

### User

**Purpose:** Represents any person who authenticates and interacts with the platform (staff or client).

**Key Attributes:**
- `user_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `email` (string, unique within firm_entity) — required
- `first_name` (string) — required
- `last_name` (string) — required
- `phone` (string) — optional
- `status` (enum: active, inactive, suspended) — required
- `role_id` (FK → Role) — required
- `office_ids` (FK[] → Office; many-to-many via UserOffice) — required (at least 1 for staff)
- `practice_area_ids` (FK[] → PracticeArea; many-to-many via UserPracticeArea) — optional (attorneys, paralegals)
- `skill_tag_ids` (FK[] → SkillTag; many-to-many via UserSkillTag) — optional
- `bar_number` (string) — optional (attorneys only)
- `bar_state` (string) — optional (attorneys only)
- `ucaas_extension` (string) — optional (staff with phone access)
- `ucaas_status` (enum: available, on_call, away, dnd, ooo) — optional
- `timezone` (string, IANA) — required (default from primary office)
- `last_login_at` (timestamp) — optional
- `mfa_enabled` (boolean) — required (default true for clients)
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `user_id`
**Unique Constraints:** `(firm_entity_id, email)`

**Relationships:**
- Belongs to one FirmEntity
- Belongs to one Role
- Assigned to many Offices (via UserOffice join)
- Assigned to many PracticeAreas (via UserPracticeArea join)
- Tagged with many SkillTags (via UserSkillTag join)
- Referenced by: Task (owner), Matter (attorney, paralegal), Lead (assigned_to), AuditLog (actor)

**Lifecycle States:** active → inactive → suspended (or back to active)

**Audit Requirements:**
- Log: creation, role change, status change, office assignment change, login events, MFA changes

---

### Role

**Purpose:** Defines a named access role that maps to a permission set. One role per user.

**Key Attributes:**
- `role_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string) — required
- `code` (enum: CLO, CEO, COO, CFO, INTAKE, ATTORNEY, PARALEGAL, BILLING, MARKETING, CLIENT) — required
- `description` (text) — optional
- `is_system_role` (boolean) — required (true for default roles; false for custom)
- `created_at` (timestamp) — required

**Primary Key:** `role_id`
**Unique Constraints:** `(firm_entity_id, code)`

**Relationships:**
- Belongs to one FirmEntity
- Has many Permissions (via RolePermission join)
- Has many Users

**Audit Requirements:**
- Log: creation, permission changes

---

### Permission

**Purpose:** Granular permission that can be assigned to a role. Defines access to a specific domain action.

**Key Attributes:**
- `permission_id` (PK, ULID) — required
- `domain` (enum: identity, crm, matters, documents, communications, scheduling, billing, accounting, trust, marketing, analytics) — required
- `action` (enum: view, create, edit, delete, approve, export, override) — required
- `resource` (string; e.g., "lead", "matter", "invoice", "trust_transfer") — required
- `scope` (enum: own, office, firm) — required
  - `own` = only entities assigned to the user
  - `office` = entities within the user's assigned offices
  - `firm` = all entities across the firm
- `description` (text) — optional

**Primary Key:** `permission_id`
**Unique Constraints:** `(domain, action, resource, scope)`

**Relationships:**
- Assigned to many Roles (via RolePermission join)

**Audit Requirements:**
- Log: role-permission assignment changes

---

### Office

**Purpose:** A physical office location within the firm. Core organizational unit for scoping data and operations.

**Key Attributes:**
- `office_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string) — required
- `code` (string, short identifier, e.g., "MAIN", "DOWN", "SUBN") — required
- `address_line_1` (string) — required
- `address_line_2` (string) — optional
- `city` (string) — required
- `state` (string, 2-letter) — required
- `zip` (string) — required
- `timezone` (string, IANA) — required
- `primary_jurisdiction_id` (FK → Jurisdiction) — required
- `business_hours` (JSON; day → open/close times) — required
- `holiday_schedule_id` (FK → HolidaySchedule) — optional
- `phone_number` (string; main office number) — required
- `status` (enum: active, inactive) — required
- `stale_lead_threshold_hours` (integer; default 48) — required
- `auto_cancel_appointment_hours` (integer; default 24) — required
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `office_id`
**Unique Constraints:** `(firm_entity_id, code)`, `(firm_entity_id, phone_number)`

**Relationships:**
- Belongs to one FirmEntity
- Belongs to one Jurisdiction (primary)
- Has many Users (via UserOffice)
- Has many Leads, Matters, Appointments
- Has one OfficeMainNumber
- Has many PracticeAreas offered (via OfficePracticeArea)

**Audit Requirements:**
- Log: creation, business hours changes, configuration changes

---

### PracticeArea

**Purpose:** A legal practice specialty (e.g., Estate Planning, Estate Administration, Elder Law, Tax Planning, Fiduciary Litigation).

**Key Attributes:**
- `practice_area_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string) — required
- `code` (string; e.g., "EP", "EA", "EL", "TX", "FL") — required
- `description` (text) — optional
- `is_active` (boolean) — required
- `default_consultation_fee_amount` (decimal) — optional
- `default_workflow_template_id` (FK → WorkflowTemplate) — optional

**Primary Key:** `practice_area_id`
**Unique Constraints:** `(firm_entity_id, code)`

**Relationships:**
- Belongs to one FirmEntity
- Offered at many Offices (via OfficePracticeArea)
- Practiced by many Users/Attorneys (via UserPracticeArea)
- Referenced by: Lead, Matter, Appointment, MatterType

**Audit Requirements:**
- Log: creation, deactivation, fee amount changes

---

### Team

**Purpose:** A named group of users for assignment and organizational purposes (e.g., "Main Street Intake Team", "Estate Planning Attorneys").

**Key Attributes:**
- `team_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string) — required
- `office_id` (FK → Office) — optional (null = firmwide team)
- `practice_area_id` (FK → PracticeArea) — optional
- `member_user_ids` (FK[] → User; many-to-many via TeamMember) — required

**Primary Key:** `team_id`

**Relationships:**
- Belongs to one FirmEntity
- Optionally scoped to one Office
- Has many Users (via TeamMember)

**Audit Requirements:**
- Log: creation, membership changes

---

### Jurisdiction

**Purpose:** A state-level regulatory jurisdiction that governs deadlines, artifact requirements, trust rules, and bar rules for marketing compliance.

**Key Attributes:**
- `jurisdiction_id` (PK, ULID) — required
- `state_code` (string, 2-letter; e.g., "VA", "NC") — required
- `state_name` (string) — required
- `is_active` (boolean) — required
- `trust_accounting_rule_set` (text/JSON) — optional (high-level reference to state bar trust rules)
- `recording_consent_type` (enum: one_party, two_party) — required
- `created_at` (timestamp) — required

**Primary Key:** `jurisdiction_id`
**Unique Constraints:** `(state_code)`

**Relationships:**
- Referenced by: Office (primary_jurisdiction), Matter (jurisdiction), DeadlineRule, RequiredArtifactRule

**Audit Requirements:**
- Log: creation, rule updates

---

### FirmEntity

**Purpose:** Top-level tenant representing a law firm or an acquired entity within a holding structure. All data is partitioned under a FirmEntity.

**Key Attributes:**
- `firm_entity_id` (PK, ULID) — required
- `name` (string) — required
- `legal_name` (string) — required
- `ein` (string) — optional (Employer Identification Number)
- `holding_company_id` (FK → HoldingCompany) — optional (for multi-entity future)
- `primary_jurisdiction_id` (FK → Jurisdiction) — required
- `status` (enum: active, inactive) — required
- `created_at` (timestamp) — required

**Primary Key:** `firm_entity_id`

**Relationships:**
- Has many Offices
- Has many Users
- Has many Matters, Leads, Invoices, etc. (all domain entities)
- Optionally belongs to a HoldingCompany (future)

**Audit Requirements:**
- Log: creation, status changes, jurisdiction changes

---

### SkillTag

**Purpose:** A tag applied to users for capacity routing, expertise matching, and scheduling optimization (e.g., "Medicaid planning", "High-net-worth estates", "Spanish-speaking").

**Key Attributes:**
- `skill_tag_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string) — required
- `category` (enum: expertise, language, certification, other) — required

**Primary Key:** `skill_tag_id`
**Unique Constraints:** `(firm_entity_id, name)`

**Relationships:**
- Applied to many Users (via UserSkillTag)
- Referenced by: capacity routing logic, appointment slot matching

**Audit Requirements:**
- Log: creation, assignment to users
