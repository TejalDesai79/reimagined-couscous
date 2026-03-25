# SECTION 2 — CANONICAL ENTITY LIST (continued)

## Part D: Documents & Signatures

---

### Document

**Purpose:** A file (client-uploaded, firm-generated, or external) associated with a matter. Supports versioning, retention policies, and signature tracking.

**Key Attributes:**
- `document_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `matter_id` (FK → Matter) — required
- `document_type_id` (FK → DocumentType) — required
- `name` (string) — required
- `description` (text) — optional
- `source` (enum: client_upload, firm_draft, court_filing, external, ai_generated) — required
- `uploaded_by_user_id` (FK → User) — optional (null for system-generated)
- `current_version_id` (FK → DocumentVersion) — required
- `status` (enum: draft, in_review, approved, final, executed, archived) — required
- `is_required_artifact` (boolean) — required (true if it satisfies a RequiredArtifactRule)
- `required_artifact_rule_id` (FK → RequiredArtifactRule) — optional
- `retention_policy_id` (FK → DocumentRetentionPolicy) — optional
- `storage_path` (string; reference to secure object storage) — required
- `mime_type` (string) — required
- `file_size_bytes` (bigint) — required
- `is_client_visible` (boolean) — required (true = visible on Client Portal)
- `signature_required` (boolean) — required (default false)
- `signature_packet_id` (FK → SignaturePacket) — optional
- `legal_hold_active` (boolean) — required (inherited from matter or explicit)
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `document_id`

**Relationships:**
- Belongs to one Matter
- Has one DocumentType
- Has many DocumentVersions (one current)
- Optionally has one SignaturePacket
- Optionally satisfies one RequiredArtifactRule
- Referenced by: CommunicationTimelineItem, FilingEvent

**Lifecycle States:** draft → in_review → approved → final → executed → archived

**Audit Requirements:**
- Log: creation, uploads, version changes, status transitions, client visibility changes, deletion (blocked if under legal hold)

---

### DocumentType

**Purpose:** Classification of document kinds (e.g., Engagement Letter, Will, Trust Instrument, POA, Photo ID, Deed, Beneficiary Form, Court Filing).

**Key Attributes:**
- `document_type_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string) — required
- `code` (string; e.g., "ENG_LETTER", "WILL", "TRUST", "POA", "PHOTO_ID") — required
- `category` (enum: client_provided, firm_work_product, court_document, financial, correspondence, other) — required
- `default_retention_years` (integer) — required
- `is_active` (boolean) — required

**Primary Key:** `document_type_id`
**Unique Constraints:** `(firm_entity_id, code)`

**Relationships:**
- Referenced by many Documents and RequiredArtifactRules

---

### DocumentVersion

**Purpose:** Immutable record of a specific version of a document. Each edit or re-upload creates a new version.

**Key Attributes:**
- `version_id` (PK, ULID) — required
- `document_id` (FK → Document) — required
- `version_number` (integer; auto-increment per document) — required
- `storage_path` (string) — required
- `file_hash` (string; SHA-256) — required
- `file_size_bytes` (bigint) — required
- `mime_type` (string) — required
- `uploaded_by_user_id` (FK → User) — optional
- `change_description` (text) — optional
- `created_at` (timestamp) — required

**Primary Key:** `version_id`
**Unique Constraints:** `(document_id, version_number)`

**Relationships:**
- Belongs to one Document

**Audit Requirements:**
- Each version creation is an immutable event (append-only)

---

### SignaturePacket

**Purpose:** Manages the e-signature workflow for a document, tracking signatories, status, and completion.

**Key Attributes:**
- `packet_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `document_id` (FK → Document) — required
- `matter_id` (FK → Matter) — required
- `status` (enum: pending, in_progress, completed, expired, cancelled) — required
- `signatories` (JSON; array of: {contact_id, role, status, signed_at, ip_address}) — required
- `total_signatures_required` (integer) — required
- `total_signatures_completed` (integer) — required (default 0)
- `external_provider_id` (string) — optional (reference to e-sign provider)
- `external_envelope_id` (string) — optional
- `initiated_by_user_id` (FK → User) — required
- `completed_at` (timestamp) — optional
- `expires_at` (timestamp) — optional
- `created_at` (timestamp) — required

**Primary Key:** `packet_id`

**Relationships:**
- Belongs to one Document and one Matter
- References Contacts as signatories (via JSON structure)

**Lifecycle States:** pending → in_progress → completed | expired | cancelled

**Audit Requirements:**
- Log: creation, each signature event (who, when, IP), completion, expiration, cancellation — COMPLIANCE CRITICAL for engagement letters

---

### DocumentRetentionPolicy

**Purpose:** Configurable retention policy applied to document types. Defines how long documents are retained after matter close.

**Key Attributes:**
- `policy_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string) — required
- `document_type_id` (FK → DocumentType) — optional (null = default for all)
- `practice_area_id` (FK → PracticeArea) — optional
- `retention_years` (integer) — required
- `retention_start_event` (enum: matter_close, document_creation, last_access) — required
- `is_active` (boolean) — required

**Primary Key:** `policy_id`

**Relationships:**
- Optionally scoped to DocumentType and PracticeArea
- Applied to many Documents

---

### EvidenceOfDelivery

**Purpose:** Records proof of delivery for physical or digital document delivery (e.g., certified mail receipts, courier confirmations, portal delivery confirmations).

**Key Attributes:**
- `delivery_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `matter_id` (FK → Matter) — required
- `document_id` (FK → Document) — optional
- `delivery_method` (enum: certified_mail, courier, email, portal, hand_delivery) — required
- `recipient_name` (string) — required
- `recipient_address` (text) — optional
- `tracking_number` (string) — optional
- `delivered_at` (timestamp) — optional
- `confirmed_at` (timestamp) — optional
- `status` (enum: pending, in_transit, delivered, returned, failed) — required
- `proof_artifact_path` (string) — optional (scan of receipt, tracking confirmation)
- `created_at` (timestamp) — required

**Primary Key:** `delivery_id`

**Relationships:**
- Belongs to one Matter, optionally one Document

**Audit Requirements:**
- Log: creation, status changes, delivery confirmation
