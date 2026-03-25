# SECTION 6 — KPI DICTIONARY & CALCULATION LOGIC

## 6.1 Intake & Pipeline KPIs

---

### KPI: Leads Generated

- **Code:** `LEADS_GENERATED`
- **Definition:** Total number of new leads created in the period
- **Formula:** `COUNT(Lead WHERE created_at IN period)`
- **Data Sources:** Lead.created_at, Lead.office_id, Lead.practice_area_id, Lead.source_channel
- **Refresh Cadence:** Real-time
- **Drilldowns:** Office, practice area, source channel, day/week/month
- **Trend:** Direction positive = up

---

### KPI: Lead Qualification Rate

- **Code:** `LEAD_QUALIFICATION_RATE`
- **Definition:** Percentage of leads that reach "qualified" stage
- **Formula:** `COUNT(Lead WHERE stage IN (qualified, fee_pending, consult_scheduled, consult_completed, engaged)) / COUNT(Lead WHERE created_at IN period) × 100`
- **Data Sources:** Lead.stage, Lead.created_at
- **Refresh Cadence:** Daily
- **Drilldowns:** Office, practice area, source channel, intake specialist
- **Trend:** Direction positive = up

---

### KPI: Consult-to-Engagement Conversion Rate

- **Code:** `CONSULT_ENGAGEMENT_RATE`
- **Definition:** Percentage of completed consultations that convert to engaged matters
- **Formula:** `COUNT(Lead WHERE stage = engaged AND consult_completed_at IN period) / COUNT(Lead WHERE stage IN (consult_completed, engaged) AND consult_completed_at IN period) × 100`
- **Data Sources:** Lead.stage, Appointment.status (completed), Matter.created_at
- **Refresh Cadence:** Daily
- **Drilldowns:** Office, practice area, attorney, source channel
- **Trend:** Direction positive = up

---

### KPI: Lead-to-Engaged Conversion Rate (Full Funnel)

- **Code:** `LEAD_ENGAGED_RATE`
- **Definition:** Percentage of all leads that ultimately become engaged matters
- **Formula:** `COUNT(Lead WHERE stage = engaged AND created_at IN period) / COUNT(Lead WHERE created_at IN period) × 100`
- **Data Sources:** Lead.stage, Lead.created_at
- **Refresh Cadence:** Daily
- **Drilldowns:** Office, practice area, source channel
- **Note:** Trailing metric — leads created in period may not yet have reached engagement
- **Trend:** Direction positive = up

---

### KPI: Intake SLA Compliance

- **Code:** `INTAKE_SLA_COMPLIANCE`
- **Definition:** Percentage of leads progressed from "lead" stage within the SLA threshold (default: 48 hours)
- **Formula:** `COUNT(Lead WHERE (stage_entered_at for 'qualified' - created_at) <= stale_lead_threshold) / COUNT(Lead WHERE stage != 'lead' OR is_stale) × 100`
- **Data Sources:** Lead.stage, Lead.stage_entered_at, Lead.created_at, Office.stale_lead_threshold_hours
- **Refresh Cadence:** Real-time
- **Drilldowns:** Office, intake specialist
- **Trend:** Direction positive = up

---

## 6.2 Financial KPIs

---

### KPI: Revenue (MTD / QTD / YTD)

- **Code:** `REVENUE`
- **Definition:** Total legal fees earned in the period (accrual basis: invoiced; cash basis: collected)
- **Formula (Accrual):** `SUM(Invoice.subtotal WHERE issued_date IN period AND status NOT IN (draft, cancelled))`
- **Formula (Cash):** `SUM(Payment.amount WHERE processed_at IN period AND status = completed AND payment_type = invoice_payment)`
- **Data Sources:** Invoice, Payment
- **Refresh Cadence:** Daily (with intra-day updates on payment events)
- **Drilldowns:** Office, practice area, attorney, matter type
- **Trend:** Direction positive = up

---

### KPI: Average Fee per Case

- **Code:** `AVG_FEE_PER_CASE`
- **Definition:** Average total fee arrangement value per engaged matter
- **Formula:** `SUM(Matter.agreed_fee_amount WHERE opened_at IN period) / COUNT(Matter WHERE opened_at IN period)`
- **Data Sources:** Matter.agreed_fee_amount, Matter.opened_at
- **Refresh Cadence:** Daily
- **Drilldowns:** Office, practice area, attorney, matter type
- **Trend:** Direction positive = up

---

### KPI: Average Cycle Time (Days)

- **Code:** `AVG_CYCLE_TIME`
- **Definition:** Average days from matter open to matter close
- **Formula:** `AVG(Matter.closed_at - Matter.opened_at) WHERE closed_at IN period AND close_reason = completed`
- **Data Sources:** Matter.opened_at, Matter.closed_at
- **Refresh Cadence:** Daily
- **Drilldowns:** Office, practice area, attorney, matter type
- **Trend:** Direction positive = down (lower is better)

---

### KPI: Stage Dwell Time

- **Code:** `STAGE_DWELL_TIME`
- **Definition:** Average time a matter spends in each workflow stage
- **Formula:** `AVG(stage_exit_at - stage_entered_at) per stage, per period`
- **Data Sources:** MatterPostureSummary (versioned snapshots with stage transition timestamps), AuditLog (matter.stage_transition events)
- **Refresh Cadence:** Daily
- **Drilldowns:** Office, practice area, attorney, matter type, stage
- **Trend:** Direction positive = down

---

### KPI: Cost per Case (Proxy)

- **Code:** `COST_PER_CASE`
- **Definition:** Estimated cost to serve a matter based on time entries and allocated overhead
- **Formula:** `(SUM(TimeEntry.hours × attorney_hourly_cost_rate) + allocated_overhead) / COUNT(Matter closed in period)`
- **Data Sources:** Task (time entries if tracked), AttorneyCapacityProfile.target_weekly_hours (for cost rate derivation), Budget (overhead allocation)
- **Refresh Cadence:** Monthly
- **Drilldowns:** Office, practice area, matter type
- **Assumption:** Attorney cost rate is derived from compensation ÷ target billable hours. Overhead is allocated proportionally by revenue or headcount.
- **Trend:** Direction positive = down

---

### KPI: Contribution Margin Proxy

- **Code:** `CONTRIBUTION_MARGIN`
- **Definition:** Revenue minus estimated cost-to-serve, as a percentage of revenue
- **Formula:** `(Revenue - Cost_per_Case × Matter_Count) / Revenue × 100`
- **Data Sources:** Revenue KPI, Cost per Case KPI
- **Refresh Cadence:** Monthly
- **Drilldowns:** Office, practice area, matter type
- **Trend:** Direction positive = up

---

### KPI: Collections Rate

- **Code:** `COLLECTIONS_RATE`
- **Definition:** Percentage of invoiced amounts collected within the period
- **Formula:** `SUM(Payment.amount WHERE processed_at IN period AND status = completed) / SUM(Invoice.total_due WHERE issued_date IN period) × 100`
- **Data Sources:** Invoice, Payment
- **Refresh Cadence:** Daily
- **Drilldowns:** Office, practice area, attorney
- **Trend:** Direction positive = up

---

### KPI: AR Aging Buckets

- **Code:** `AR_AGING`
- **Definition:** Outstanding accounts receivable categorized by age
- **Formula:** Per aging bucket: `SUM(Invoice.balance_due WHERE balance_due > 0 AND days_since_issued IN bucket_range)`
- **Buckets:** Current (0 days), 1–30 days, 31–60 days, 61–90 days, >90 days
- **Data Sources:** Invoice.balance_due, Invoice.issued_date
- **Refresh Cadence:** Daily (snapshot captured in AccountsReceivableAgingSnapshot)
- **Drilldowns:** Office, practice area, attorney, client
- **Trend:** Direction positive = down (lower outstanding is better)

---

### KPI: Retainer Replenishment Frequency

- **Code:** `RETAINER_REPLENISHMENT_FREQ`
- **Definition:** Average number of retainer replenishment events per matter per month
- **Formula:** `COUNT(RetainerTransaction WHERE type = replenishment AND created_at IN period) / COUNT(DISTINCT RetainerAccount WHERE active in period)`
- **Data Sources:** RetainerTransaction, RetainerAccount
- **Refresh Cadence:** Monthly
- **Drilldowns:** Office, practice area
- **Trend:** Neutral (informational; high frequency may indicate either good engagement or chronic underfunding)

---

## 6.3 Responsiveness & SLA KPIs

---

### KPI: Time to Answer (Calls)

- **Code:** `TIME_TO_ANSWER`
- **Definition:** Average wait time before an inbound call is answered
- **Formula:** `AVG(CallRecord.answer_time - CallRecord.start_time) WHERE status = completed AND direction = inbound`
- **Data Sources:** CallRecord.start_time, CallRecord.answer_time
- **Refresh Cadence:** Real-time
- **Drilldowns:** Office, queue, time of day
- **Trend:** Direction positive = down

---

### KPI: Missed Call Rate

- **Code:** `MISSED_CALL_RATE`
- **Definition:** Percentage of inbound calls that were missed or abandoned
- **Formula:** `COUNT(CallRecord WHERE status IN (missed, abandoned)) / COUNT(CallRecord WHERE direction = inbound) × 100`
- **Data Sources:** CallRecord.status, CallRecord.direction
- **Refresh Cadence:** Real-time
- **Drilldowns:** Office, queue, time of day
- **Trend:** Direction positive = down

---

### KPI: Abandoned Call Rate

- **Code:** `ABANDONED_CALL_RATE`
- **Definition:** Percentage of callers who hung up while waiting in queue
- **Formula:** `COUNT(CallRecord WHERE status = abandoned) / COUNT(CallRecord WHERE direction = inbound) × 100`
- **Data Sources:** CallRecord.status
- **Refresh Cadence:** Real-time
- **Drilldowns:** Office, queue
- **Trend:** Direction positive = down

---

### KPI: Time to First Response (Portal Messages)

- **Code:** `TIME_TO_FIRST_RESPONSE`
- **Definition:** Average time from client portal message receipt to first staff response
- **Formula:** `AVG(first_staff_reply.created_at - client_message.created_at) WHERE client_message.sender_type = client`
- **Data Sources:** PortalMessage (thread pairs: client message → staff reply)
- **Refresh Cadence:** Daily
- **Drilldowns:** Office, attorney/paralegal, matter type
- **Trend:** Direction positive = down

---

### KPI: SLA Adherence Rate

- **Code:** `SLA_ADHERENCE`
- **Definition:** Percentage of client communications responded to within SLA target
- **Formula:** `COUNT(SLAEvent WHERE is_breached = false) / COUNT(SLAEvent) × 100`
- **Data Sources:** SLAEvent
- **Refresh Cadence:** Daily
- **Drilldowns:** Office, channel, priority
- **Trend:** Direction positive = up

---

### KPI: Call Queue SLA Compliance

- **Code:** `CALL_QUEUE_SLA`
- **Definition:** Percentage of calls answered within the queue's SLA target (e.g., 80% within 30 seconds)
- **Formula:** `COUNT(CallRecord WHERE wait_time_seconds <= Queue.sla_target_seconds) / COUNT(CallRecord WHERE queue_id = X AND direction = inbound) × 100`
- **Data Sources:** CallRecord.wait_time_seconds, Queue.sla_target_seconds
- **Refresh Cadence:** Real-time
- **Drilldowns:** Office, queue
- **Trend:** Direction positive = up

---

## 6.4 Scheduling & Capacity KPIs

---

### KPI: No-Show Rate

- **Code:** `NO_SHOW_RATE`
- **Definition:** Percentage of confirmed appointments where the client did not attend
- **Formula:** `COUNT(Appointment WHERE status = no_show) / COUNT(Appointment WHERE status IN (completed, no_show)) × 100`
- **Data Sources:** Appointment.status
- **Refresh Cadence:** Daily
- **Drilldowns:** Client type (new/returning), source channel, practice area, attorney, time of day
- **Feeds into:** Underwriting scoring model, scheduling analytics
- **Trend:** Direction positive = down

---

### KPI: Capacity Utilization per Attorney

- **Code:** `ATTORNEY_UTILIZATION`
- **Definition:** Complexity-weighted hours utilized vs. target hours for each attorney
- **Formula:** `CapacityComputation.utilization_percentage`
- **Data Sources:** CapacityComputation (precomputed)
- **Refresh Cadence:** Real-time (recomputed on matter/appointment changes)
- **Drilldowns:** Office, practice area, individual attorney
- **Thresholds:** Under (< 60%), Good (60–80%), Near (80–90%), Over (> 90%)
- **Trend:** Target range = 70–85%

---

### KPI: Firmwide Capacity Utilization

- **Code:** `FIRMWIDE_UTILIZATION`
- **Definition:** Average capacity utilization across all active attorneys
- **Formula:** `AVG(CapacityComputation.utilization_percentage) across all attorneys`
- **Data Sources:** CapacityComputation
- **Refresh Cadence:** Real-time
- **Drilldowns:** Office, practice area
- **Trend:** Target range = 70–85%

---

### KPI: Capacity Gap

- **Code:** `CAPACITY_GAP`
- **Definition:** Difference between available open appointment slots and consult demand
- **Formula:** `COUNT(OpenAppointmentSlot WHERE status = open AND date IN period) - COUNT(Lead WHERE stage IN (qualified, fee_pending) AND practice_area_id = X)`
- **Data Sources:** OpenAppointmentSlot, Lead
- **Refresh Cadence:** Real-time
- **Drilldowns:** Office, practice area
- **Interpretation:** Positive = surplus capacity; Negative = constrained; triggers expansion request alerts
- **Trend:** Target = 0 to +5 (slight surplus is healthy)

---

## 6.5 Marketing KPIs

---

### KPI: Content Marketing ROI Proxy

- **Code:** `CONTENT_MARKETING_ROI`
- **Definition:** Estimated ROI of content marketing based on attributed leads → engagements → revenue
- **Formula:** `SUM(Matter.agreed_fee_amount WHERE Matter.lead_id → Lead.campaign_id → Campaign.channel = content_marketing) / SUM(Campaign.budget_amount WHERE channel = content_marketing)`
- **Data Sources:** ContentAttributionEvent, Lead, Matter, Campaign
- **Refresh Cadence:** Monthly
- **Drilldowns:** Channel, content type, practice area
- **Trend:** Direction positive = up

---

### KPI: Lead Attribution by Source

- **Code:** `LEAD_ATTRIBUTION`
- **Definition:** Lead count, qualified count, engaged count, and revenue by source channel
- **Formula:** Per source channel: `COUNT(Lead), COUNT(Lead WHERE stage >= qualified), COUNT(Lead WHERE stage = engaged), SUM(attributed revenue)`
- **Data Sources:** Lead.source_channel, ContentAttributionEvent, Matter.agreed_fee_amount
- **Refresh Cadence:** Daily
- **Drilldowns:** Source channel, practice area, office, period
- **Trend:** Varies by channel

---

### KPI: Campaign ROI

- **Code:** `CAMPAIGN_ROI`
- **Definition:** Revenue attributed to a campaign divided by campaign spend
- **Formula:** `SUM(attributed Matter.agreed_fee_amount) / Campaign.budget_amount`
- **Data Sources:** ContentAttributionEvent, Campaign, Matter
- **Refresh Cadence:** Monthly
- **Drilldowns:** Campaign, channel, practice area
- **Trend:** Direction positive = up

---

## 6.6 KPI Computation Approach

### Real-Time KPIs
Computed on demand via materialized views or event-driven aggregation. No scheduled batch jobs.
- Examples: Queue SLA, capacity utilization, call metrics, active lead counts

### Daily KPIs
Computed via nightly batch process that creates KPIComputation snapshots.
- Examples: Conversion rates, AR aging, cycle time, SLA adherence, revenue

### Monthly KPIs
Computed at month-end close process.
- Examples: Cost per case, contribution margin, retainer replenishment frequency, content marketing ROI

### All KPIs
- Stored as KPIComputation records with period, scope (office/practice/attorney), and value
- Historical computations preserved for trend analysis
- Dashboard widgets read from the latest KPIComputation for their metric + scope
- Trend is calculated by comparing current period to prior period of same type
