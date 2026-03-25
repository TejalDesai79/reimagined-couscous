# SECTION 5 — AUDIT LOGGING & IMMUTABLE EVENT TAXONOMY

## 5.1 Audit Log Record Structure

Every auditable event produces a record in the `AuditLog` entity with the following fields:

| Field | Type | Description |
|-------|------|-------------|
| `log_id` | ULID | Unique, time-ordered identifier |
| `firm_entity_id` | FK | Tenant partition key |
| `event_category` | string | Domain category (see taxonomy below) |
| `event_type` | string | Specific event identifier (dot notation) |
| `actor_user_id` | FK (nullable) | User who performed the action (null for system events) |
| `actor_role` | string | Role code of the actor at time of event |
| `actor_ip_address` | string | IP address of the actor's session |
| `entity_type` | string | The type of entity affected |
| `entity_id` | ULID | The ID of the entity affected |
| `action` | string | The verb: create, update, approve, deny, delete_attempt, override, login, etc. |
| `before_state` | JSON | Snapshot of changed fields BEFORE the action (null for creates) |
| `after_state` | JSON | Snapshot of changed fields AFTER the action (null for deletes) |
| `reason` | text | Free-text reason (required for overrides, approvals, denials, waivers) |
| `source_screen` | string | Which platform screen triggered the event |
| `metadata` | JSON | Additional context (e.g., related entity IDs, computed values) |
| `occurred_at` | timestamp | When the event occurred |
| `created_at` | timestamp | When the log entry was persisted |

### Immutability Rules
- The AuditLog table is **APPEND-ONLY**. No UPDATE or DELETE operations are permitted.
- No user, including CLO or system administrators, can modify or remove audit entries.
- Retention minimum: **7 years**. No automated purge.
- Exports include a SHA-256 hash of the entry for chain-of-custody verification.

---

## 5.2 Actions That MUST Generate Audit Entries

The following actions are **mandatory audit events**. The system must not allow these actions to complete without writing an audit entry.

### Tier 1 — Highest Sensitivity (Trust, Compliance, Security)
- Trust ledger deposits, transfers, disbursements
- Trust transfer request creation, approval, denial, execution
- Trust reconciliation start, completion, discrepancy detection, resolution
- Trust compliance exceptions (negative balance attempts, commingling attempts)
- Ethical wall creation, modification, access denial events
- Conflict check overrides (CLO)
- Statutory deadline waivers (CLO)
- Legal hold creation, modification, release
- User login, logout, failed login, MFA events
- Role changes, permission changes
- Data exports (who, what scope, when)

### Tier 2 — High Sensitivity (Financial, Approvals)
- Invoice creation, send, payment application, write-off request/approval/denial
- Fee arrangement creation and modification
- Consultation fee waiver requests and CLO decisions
- Dunning step approvals and escalations
- Payment failures and refunds
- Retainer threshold changes
- Budget approval, forecast assumption changes
- Capacity overrides (CLO force-open/close slots)
- Expansion request decisions (approve/deny/defer)

### Tier 3 — Standard Operations
- Lead creation, stage transitions, qualification, disqualification
- Matter creation, stage transitions, closure
- Task creation, completion, reassignment
- Document uploads, version changes, signature events
- Appointment booking, completion, cancellation, no-show marking
- Call record creation, AI summary approval
- Portal message creation, staff approval for outbound
- Content approval workflow events, publication
- Public signal flag/dismiss/opt-out/conversion events
- Notification delivery events
- Underwriting score calculation and override events

---

## 5.3 Event Taxonomy

Events follow the pattern: `{category}.{entity}.{action}`

### Category: identity
```
identity.user.created
identity.user.updated
identity.user.status_changed          (active/inactive/suspended)
identity.user.role_changed
identity.user.office_assigned
identity.user.office_removed
identity.user.login_success
identity.user.login_failed
identity.user.logout
identity.user.mfa_enabled
identity.user.mfa_disabled
identity.user.session_timeout
identity.role.permission_granted
identity.role.permission_revoked
```

### Category: matter
```
matter.created
matter.stage_transition               (before/after stage; gating check result)
matter.attorney_reassigned
matter.paralegal_reassigned
matter.fee_arrangement_set
matter.fee_arrangement_modified
matter.ethical_wall_created
matter.ethical_wall_modified
matter.ethical_wall_access_denied     (when restricted user attempts access)
matter.legal_hold_created
matter.legal_hold_released
matter.closed
matter.reopened
```

### Category: compliance
```
compliance.conflict_check.executed
compliance.conflict_check.clear
compliance.conflict_check.found
compliance.conflict_check.overridden  (CLO; reason required)
compliance.deadline.created
compliance.deadline.updated
compliance.deadline.met
compliance.deadline.overdue
compliance.deadline.waived            (CLO; reason required)
compliance.required_artifact.missing_escalated
compliance.required_artifact.satisfied
compliance.engagement_letter.missing_blocked
compliance.gating_check.failed       (stage transition blocked)
compliance.gating_check.passed
```

### Category: deadline
```
deadline.created
deadline.due_date_changed
deadline.status_changed               (upcoming → at_risk → overdue → met)
deadline.waived                        (CLO only; reason + before/after)
deadline.deletion_attempted
deadline.deletion_blocked             (statutory deadlines)
```

### Category: document
```
document.created
document.uploaded
document.version_created
document.status_changed
document.client_visibility_changed
document.signature_initiated
document.signature_completed
document.signature_expired
document.retention_expired
document.deletion_blocked             (legal hold)
document.deletion_executed
```

### Category: billing
```
billing.invoice.created
billing.invoice.auto_generated
billing.invoice.sent
billing.invoice.payment_applied
billing.invoice.retainer_applied
billing.invoice.status_changed
billing.payment.attempted
billing.payment.completed
billing.payment.failed
billing.payment.refunded
billing.retainer.deposited
billing.retainer.drawn
billing.retainer.threshold_changed
billing.retainer.replenishment_requested
billing.write_off.requested
billing.write_off.approved
billing.write_off.denied
billing.fee_override.requested
billing.fee_override.approved
billing.fee_override.denied
billing.dunning.step_executed
billing.dunning.step_approved
billing.dunning.sequence_paused
billing.payment_plan.created
billing.payment_plan.installment_missed
billing.consultation_fee.collected
billing.consultation_fee.failed
billing.consultation_fee.waiver_requested
billing.consultation_fee.waiver_approved
billing.consultation_fee.waiver_denied
```

### Category: trust
```
trust.ledger.created
trust.deposit.recorded
trust.transfer.requested
trust.transfer.approved               (highest sensitivity: approver, amount, matter, balance after)
trust.transfer.denied
trust.transfer.executed
trust.disbursement.requested
trust.disbursement.approved
trust.disbursement.executed
trust.reconciliation.started
trust.reconciliation.completed
trust.reconciliation.discrepancy_found
trust.reconciliation.discrepancy_resolved
trust.compliance_exception.negative_balance_attempt
trust.compliance_exception.commingling_attempt
trust.compliance_exception.stale_reconciliation
trust.compliance_exception.unauthorized_access
trust.compliance_exception.large_transfer_alert
trust.compliance_exception.matter_close_with_balance
trust.recording_access                (who listened to recording, when)
```

### Category: communications
```
comms.call.started
comms.call.answered
comms.call.transferred
comms.call.ended
comms.call.missed
comms.call.abandoned
comms.call.voicemail_left
comms.call.recording_started
comms.call.recording_stopped
comms.call.recording_accessed          (who listened)
comms.call.ai_summary_generated
comms.call.ai_summary_approved
comms.call.consent_obtained
comms.call.consent_not_obtained
comms.sms.sent
comms.sms.delivered
comms.sms.failed
comms.email.sent
comms.email.delivered
comms.email.bounced
comms.portal_message.created
comms.portal_message.staff_approved
comms.portal_message.read
comms.sla.breached
```

### Category: capacity
```
capacity.computation.executed
capacity.slot.opened_by_engine
capacity.slot.closed_by_engine
capacity.slot.booked
capacity.slot.cancelled
capacity.override.created              (CLO: who, what, previous value, new value, reason)
capacity.override.slot_force_opened
capacity.override.attorney_closed_to_intake
capacity.override.target_hours_adjusted
capacity.expansion_request.created
capacity.expansion_request.approved
capacity.expansion_request.modified_and_approved
capacity.expansion_request.denied
capacity.expansion_request.deferred
capacity.policy.threshold_changed
```

### Category: underwriting
```
underwriting.score.calculated
underwriting.score.overridden          (user, reason, before/after)
underwriting.strategy.suggested
underwriting.pricing.suggested
underwriting.model_version.changed
```

### Category: marketing
```
marketing.content.created
marketing.content.ai_drafted
marketing.content.submitted_for_review
marketing.content.attorney_reviewed
marketing.content.attorney_rejected
marketing.content.clo_approved
marketing.content.clo_rejected
marketing.content.published
marketing.demand_shaping.recommended
marketing.demand_shaping.approval_requested
marketing.demand_shaping.approved
marketing.demand_shaping.denied
marketing.public_signal.captured
marketing.public_signal.flagged
marketing.public_signal.dismissed
marketing.public_signal.opted_out       (immediate + permanent)
marketing.public_signal.converted_to_lead
marketing.public_signal.compliance_reviewed
```

### Category: client_portal
```
portal.login
portal.logout
portal.document.uploaded
portal.document.viewed
portal.document.downloaded
portal.document.signed
portal.message.sent
portal.message.read
portal.payment.attempted
portal.payment.completed
portal.payment.failed
portal.payment_method.added
portal.payment_method.removed
portal.session.timeout
```

### Category: system
```
system.data_export.initiated           (who, what scope, purpose)
system.data_export.completed
system.legal_hold.applied
system.legal_hold.released
system.retention.deletion_scheduled
system.retention.deletion_blocked      (legal hold)
system.retention.deletion_executed
system.config.changed                  (any system configuration change)
```
