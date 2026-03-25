# SECTION 2 — CANONICAL ENTITY LIST (continued)

## Part B: CRM / Intake

---

### Lead

**Purpose:** A prospective client that has entered the intake pipeline. Progresses through stages from initial inquiry to qualification and scheduling.

**Key Attributes:**
- `lead_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — required
- `contact_id` (FK → Contact) — required
- `practice_area_id` (FK → PracticeArea) — required
- `referral_source_id` (FK → ReferralSource) — optional
- `campaign_id` (FK → Campaign) — optional
- `assigned_to_user_id` (FK → User) — optional (assigned intake specialist)
- `stage` (enum: lead, qualified, fee_pending, consult_scheduled, consult_completed, engaged, disqualified, stale) — required
- `stage_entered_at` (timestamp) — required (timestamp when current stage was entered)
- `underwriting_score_id` (FK → ClientUnderwritingScore) — optional
- `conflict_check_id` (FK → ConflictCheck) — optional
- `consultation_fee_required` (boolean) — required
- `consultation_fee_status` (enum: not_required, not_collected, processing, paid, failed, waived, credited) — required
- `consultation_fee_waiver_reason` (text) — optional
- `consultation_fee_waiver_approved_by` (FK → User) — optional
- `source_channel` (enum: website_form, phone, referral_attorney, referral_client, google_ad, social_media, newsletter, public_signal, walk_in, other) — required
- `urgency` (enum: low, moderate, high, critical) — optional
- `intake_questionnaire_id` (FK → IntakeQuestionnaire) — optional
- `notes` (text) — optional
- `is_prior_client` (boolean) — required (checked against 12-month engagement window)
- `prior_engagement_within_12mo` (boolean) — required
- `has_professional_referral` (boolean) — required
- `matter_id` (FK → Matter) — optional (populated when lead converts to engaged matter)
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `lead_id`
**Unique Constraints:** `(firm_entity_id, contact_id, practice_area_id, created_at)` — prevents duplicate leads for the same contact + practice in the same moment

**Relationships:**
- Belongs to one FirmEntity, one Office, one Contact
- Optionally has one IntakeQuestionnaire
- Optionally has one ConflictCheck
- Optionally has one ClientUnderwritingScore
- May reference one ReferralSource and one Campaign
- May convert to one Matter
- Has many LeadActivityEvents (via activity timeline)

**Lifecycle States:**
```
lead → qualified → fee_pending → consult_scheduled → consult_completed → engaged
lead → disqualified
lead → stale (auto, after threshold hours without progression)
```

**Audit Requirements:**
- Log: creation, stage transitions, fee payment events, qualification decisions, disqualification with reason, score overrides, fee waiver requests and decisions

---

### Contact

**Purpose:** A person (potential or actual client) with contact information. Shared across leads and matters. A contact may have multiple leads and matters over time.

**Key Attributes:**
- `contact_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `first_name` (string) — required
- `last_name` (string) — required
- `email` (string) — optional
- `phone_primary` (string) — optional
- `phone_secondary` (string) — optional
- `address_line_1` (string) — optional
- `address_line_2` (string) — optional
- `city` (string) — optional
- `state` (string) — optional
- `zip` (string) — optional
- `date_of_birth` (date) — optional
- `is_client` (boolean) — required (true once any matter reaches "engaged")
- `client_portal_user_id` (FK → User) — optional (linked when portal access is granted)
- `household_id` (FK → Household) — optional
- `opted_out_of_marketing` (boolean) — required (default false)
- `opted_out_of_public_signals` (boolean) — required (default false)
- `public_signal_opt_out_at` (timestamp) — optional
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `contact_id`
**Unique Constraints:** `(firm_entity_id, email)` where email is not null; `(firm_entity_id, phone_primary)` where phone is not null — soft uniqueness for deduplication

**Relationships:**
- Belongs to one FirmEntity
- Optionally belongs to one Household
- Has many Leads
- Has many Matters (as client)
- May have one User (portal access)

**Audit Requirements:**
- Log: creation, edits to PII fields, opt-out changes, client status changes

---

### Household

**Purpose:** Groups related contacts (e.g., married couple, parent-child) for estate planning context and cross-matter awareness.

**Key Attributes:**
- `household_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string; typically "The [Last Name] Family") — required
- `created_at` (timestamp) — required

**Primary Key:** `household_id`

**Relationships:**
- Belongs to one FirmEntity
- Has many Contacts

**Audit Requirements:**
- Log: creation, membership changes

---

### ReferralSource

**Purpose:** Tracks the origin source of leads for attribution and fee waiver logic. Distinguishes professional referrals (which waive consultation fees per FSD) from non-professional sources.

**Key Attributes:**
- `referral_source_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string) — required
- `type` (enum: attorney_referral, cpa_referral, financial_advisor, client_referral, other_professional, non_professional) — required
- `is_professional` (boolean) — required (drives consultation fee waiver logic)
- `contact_name` (string) — optional
- `contact_email` (string) — optional
- `contact_phone` (string) — optional
- `is_active` (boolean) — required

**Primary Key:** `referral_source_id`

**Relationships:**
- Belongs to one FirmEntity
- Referenced by many Leads

**Audit Requirements:**
- Log: creation, type changes (affects fee waiver logic)

---

### Campaign

**Purpose:** A marketing campaign for lead attribution. Links to marketing content and tracks performance.

**Key Attributes:**
- `campaign_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `name` (string) — required
- `channel` (enum: google_ads, social_media, seo, newsletter, content_marketing, direct_mail, event, other) — required
- `practice_area_id` (FK → PracticeArea) — optional
- `office_ids` (FK[] → Office) — optional (targeted offices)
- `status` (enum: planned, active, paused, completed) — required
- `start_date` (date) — optional
- `end_date` (date) — optional
- `budget_amount` (decimal) — optional
- `created_at` (timestamp) — required

**Primary Key:** `campaign_id`

**Relationships:**
- Belongs to one FirmEntity
- Referenced by many Leads (for attribution)
- Has many ContentItems (for content campaigns)

**Audit Requirements:**
- Log: creation, status changes, budget changes

---

### IntakeQuestionnaire

**Purpose:** Stores the client's digital intake form responses. Template varies by practice area.

**Key Attributes:**
- `questionnaire_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `lead_id` (FK → Lead) — required
- `practice_area_id` (FK → PracticeArea) — required
- `template_version` (string) — required (version of the questionnaire template used)
- `responses` (JSON; structured key-value pairs) — required
- `is_complete` (boolean) — required
- `completion_percentage` (integer, 0–100) — required
- `submitted_at` (timestamp) — optional
- `created_at` (timestamp) — required

**Primary Key:** `questionnaire_id`
**Unique Constraints:** `(lead_id)` — one questionnaire per lead

**Relationships:**
- Belongs to one Lead
- Used by underwriting scoring engine

**Audit Requirements:**
- Log: creation, completion, individual field updates if tracked

---

### ConflictCheck

**Purpose:** Records the result of an automated conflict-of-interest check run against a lead's contact information and related parties.

**Key Attributes:**
- `conflict_check_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `lead_id` (FK → Lead) — required
- `contact_id` (FK → Contact) — required
- `status` (enum: clear, potential_conflict, conflict_found, overridden) — required
- `matched_records` (JSON; array of matched entity references with similarity scores) — optional
- `override_reason` (text) — optional (required when status = overridden)
- `override_approved_by` (FK → User) — optional (CLO only)
- `overridden_at` (timestamp) — optional
- `checked_at` (timestamp) — required
- `created_at` (timestamp) — required

**Primary Key:** `conflict_check_id`

**Relationships:**
- Belongs to one Lead and one Contact
- Override approved by one User (CLO)

**Lifecycle States:** clear | potential_conflict | conflict_found | overridden

**Audit Requirements:**
- Log: every check run, status result, override events (who, when, reason) — IMMUTABLE entries; overrides are highest-sensitivity

---

### ClientUnderwritingScore

**Purpose:** AI-generated scoring of a lead's value and complexity for routing, pricing, and strategy suggestions. Explainable by design.

**Key Attributes:**
- `score_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `lead_id` (FK → Lead) — required
- `score_value` (integer, 1–10) — required
- `score_tier` (enum: high, medium, low) — required
- `factors` (FK[] → UnderwritingFactor; one-to-many) — required
- `is_overridden` (boolean) — required (default false)
- `override_reason` (text) — optional
- `overridden_by_user_id` (FK → User) — optional
- `overridden_at` (timestamp) — optional
- `model_version` (string) — required (AI model version for reproducibility)
- `calculated_at` (timestamp) — required
- `created_at` (timestamp) — required

**Primary Key:** `score_id`
**Unique Constraints:** `(lead_id)` — one active score per lead (new scores create new records; previous become historical)

**Relationships:**
- Belongs to one Lead
- Has many UnderwritingFactors
- Referenced by StrategyRecommendation and PricingRecommendation

**Audit Requirements:**
- Log: calculation events, override events with reason and actor

---

### UnderwritingFactor

**Purpose:** An individual factor contributing to an underwriting score. Each factor has an explainable label and point value (positive or negative).

**Key Attributes:**
- `factor_id` (PK, ULID) — required
- `score_id` (FK → ClientUnderwritingScore) — required
- `name` (string; e.g., "Asset range ($500K–$1M)") — required
- `direction` (enum: positive, negative) — required
- `point_value` (integer) — required
- `explanation` (text) — optional

**Primary Key:** `factor_id`

**Relationships:**
- Belongs to one ClientUnderwritingScore

---

### StrategyRecommendation

**Purpose:** AI-generated legal strategy suggestion for a lead, explicitly labeled as decision support (not legal advice). Advisory only — attorney makes final decisions.

**Key Attributes:**
- `recommendation_id` (PK, ULID) — required
- `lead_id` (FK → Lead) — required
- `score_id` (FK → ClientUnderwritingScore) — required
- `rank` (integer; 1 = recommended) — required
- `strategy_name` (string) — required
- `description` (text) — required
- `is_recommended` (boolean) — required
- `model_version` (string) — required
- `disclaimer_text` (string; always "Decision support only — not legal advice") — required (system-enforced)
- `created_at` (timestamp) — required

**Primary Key:** `recommendation_id`

**Relationships:**
- Belongs to one Lead and one ClientUnderwritingScore

---

### PricingRecommendation

**Purpose:** AI-generated pricing model and fee range suggestion. Attorney sets final pricing. Advisory only.

**Key Attributes:**
- `recommendation_id` (PK, ULID) — required
- `lead_id` (FK → Lead) — required
- `score_id` (FK → ClientUnderwritingScore) — required
- `pricing_model` (enum: fixed_fee, hourly, tiered, hybrid) — required
- `is_recommended_model` (boolean) — required
- `fee_range_low` (decimal) — required
- `fee_range_high` (decimal) — required
- `alternative_description` (text) — optional
- `basis` (text; e.g., "Cost-to-serve + capacity + margin targets") — required
- `disclaimer_text` (string; always "Suggested ranges — attorney sets final pricing") — required (system-enforced)
- `model_version` (string) — required
- `created_at` (timestamp) — required

**Primary Key:** `recommendation_id`

**Relationships:**
- Belongs to one Lead and one ClientUnderwritingScore

---

### ConsultationFeePolicy

**Purpose:** Office-level and practice-area-level configuration for consultation fee requirements, amounts, and waiver rules.

**Key Attributes:**
- `policy_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `office_id` (FK → Office) — optional (null = firmwide default)
- `practice_area_id` (FK → PracticeArea) — optional (null = all practice areas)
- `fee_amount` (decimal) — required
- `fee_required_rule` (text/JSON) — required
  - Default rule: fee required when (no professional referral) AND (not engaged client within prior 12 months)
- `waiver_requires_clo_approval` (boolean) — required (default true)
- `credit_to_engagement` (boolean) — required (default true; fee credited if client engages)
- `effective_date` (date) — required
- `end_date` (date) — optional (for versioning)
- `created_at` (timestamp) — required

**Primary Key:** `policy_id`

**Relationships:**
- Belongs to one FirmEntity
- Optionally scoped to Office and PracticeArea
- Referenced by Lead (fee determination logic)

**Audit Requirements:**
- Log: creation, amount changes, rule changes — these are CLO-controlled

---

### ConsultationFeePayment

**Purpose:** Records the payment (or attempted payment) of a consultation fee for a specific lead. Links to the broader Payment entity or can stand alone for pre-engagement payments.

**Key Attributes:**
- `fee_payment_id` (PK, ULID) — required
- `firm_entity_id` (FK → FirmEntity) — required
- `lead_id` (FK → Lead) — required
- `amount` (decimal) — required
- `status` (enum: pending, processing, paid, failed, waived, refunded) — required
- `payment_method` (enum: card, ach) — optional
- `payment_reference` (string; e.g., last 4 digits of card) — optional
- `failure_reason` (string) — optional
- `attempt_count` (integer) — required (default 0)
- `waived_by_user_id` (FK → User) — optional (CLO when fee is waived)
- `waiver_reason` (text) — optional
- `paid_at` (timestamp) — optional
- `created_at` (timestamp) — required
- `updated_at` (timestamp) — required

**Primary Key:** `fee_payment_id`

**Relationships:**
- Belongs to one Lead
- Optionally links to a Payment entity (when integrated with billing)

**Lifecycle States:** pending → processing → paid | failed | waived | refunded

**Audit Requirements:**
- Log: every payment attempt (success and failure), waiver events (who, when, reason), refunds
