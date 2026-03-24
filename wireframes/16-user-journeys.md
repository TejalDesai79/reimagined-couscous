# User Journeys

## Journey 1: New Client → Intake → Consult → Engagement

**Scenario:** Maria Garcia submits a website form expressing interest in estate planning. She has no professional referral and has never been a client. She needs to go from lead to engaged client.

### Step-by-Step Flow

| Step | Actor | Screen | Action | System Response |
|------|-------|--------|--------|-----------------|
| 1 | Maria (external) | Website form | Submits inquiry: estate planning, married, 2 children, $500K–$1M assets | Form data captured; lead created in system |
| 2 | System | — | Auto-runs conflict check; auto-calculates underwriting score | Conflict: ✅ Clear. Score: HIGH (7/10). Factors: asset range, family complexity, multiple concerns |
| 3 | System | — | Determines consultation fee requirement | Fee required: $250 (no professional referral + not an engaged client in prior 12 months) |
| 4 | System | — | Generates AI suggestions | Suggested strategies: (1) Comprehensive plan with trust (recommended), (2) Basic will + guardianship. Suggested pricing: Fixed $3,500–$5,500 |
| 5 | A. Johnson (Intake) | Screen 1 (Home) | Sees new lead alert in "My Work Today" and Alert Banner | "New lead: Maria Garcia — Estate Planning — HIGH score" |
| 6 | A. Johnson | Screen 4 (Intake Console) | Opens lead; reviews contact info, questionnaire answers, conflict check, underwriting score, AI suggestions | Full lead detail loaded in right panel |
| 7 | A. Johnson | Screen 4 | Marks lead as "Qualified" | Stage updates to Qualified; lead card turns green |
| 8 | A. Johnson | Screen 4 | Clicks "Collect Payment →" (consultation fee panel) | Payment modal opens: $250, card/ACH |
| 9 | A. Johnson | Screen 4 (Payment modal) | Enters Maria's payment info (card, Visa *4521) | Payment processes... ✅ Paid. Fee panel updates. |
| 10 | System | Screen 4 | Scheduling section unlocks | Available slots populate from Capacity Engine (only capacity-approved slots for Estate Planning attorneys) |
| 11 | A. Johnson | Screen 4 → Screen 5 | Selects slot: Mon Mar 24, 9:00 AM, K. Park | Slot selected; confirmation preview shown |
| 12 | A. Johnson | Screen 5 | Confirms appointment | Appointment created. System triggers automated confirmations. |
| 13 | System | — | Sends confirmation email + SMS to Maria | Email: date/time/location/attorney/purpose/cancel-reschedule link. SMS: same. Both logged in timeline. |
| 14 | System | — | Schedules reminders | Same-day reminder at 7:00 AM (if configured). 24-hr reminder if booking was >24hrs ahead. |
| 15 | K. Park (Attorney) | Screen 1 (Home) | Sees upcoming consult in "My Work Today" | "Consult: Maria Garcia — Estate Planning — 9:00 AM. Prep summary available." |
| 16 | K. Park | Screen 7 (Matter WS, consult prep view) | Reviews consult prep: underwriting HIGH, key concerns (guardianship, tax), suggested strategies, suggested pricing | Attorney is prepared for meeting; can add notes |
| 17 | Maria | In person / video | Attends consultation with K. Park | Discussion of estate planning needs |
| 18 | A. Johnson | Screen 5 | Marks appointment as "Completed" | Appointment status updates; matter stage → "Consult Completed" |
| 19 | K. Park | Screen 7 | Reviews AI-generated call/consult summary; edits and approves | Summary saved to matter timeline |
| 20 | K. Park | Screen 7 | Creates engagement: accepts/adjusts scope and pricing; sends engagement letter for signature | Engagement letter generated; sent to Maria via portal |
| 21 | Maria | Screen 14 (Client Portal) | Receives notification; reviews and signs engagement letter | E-signature captured; document status → Signed |
| 22 | System | — | Stage advances to "Engaged" | Matter created; posture tracker initialized; all team members notified |
| 23 | Maria | Screen 13 (Client Portal) | Logs into portal; sees Status Tracker | "Consultation ✅ → Engaged ✅ → Planning 🔵 → Drafting ○ → Review ○ → Signing ○ → Complete ○" |

### Key Decision Points
- **Step 3:** Fee determination is automatic based on rules (no referral + not recent client = fee required)
- **Step 10:** Slots are ONLY those approved by the Capacity Engine — intake cannot override
- **Step 16:** AI suggestions are decision support — attorney makes final scope/pricing decisions
- **Step 20:** Engagement cannot proceed until engagement letter is signed (gating)

### What Could Go Wrong
| Risk | System Response |
|------|-----------------|
| Payment fails at Step 9 | "Payment failed. [Try again] [Different method]" — scheduling stays locked |
| No available slots at Step 10 | "No available capacity for Estate Planning. [Request expansion → CLO]" |
| Conflict found at Step 2 | "Conflict detected. Scheduling blocked. [Escalate to CLO]" |
| Maria no-shows at Step 17 | Intake marks no-show; logged in analytics; no-show data feeds underwriting |
| Maria doesn't sign engagement at Step 21 | Matter stays at "Consult Completed"; reminder sent; if >7 days, alert to attorney |

---

## Journey 2: Estate Administration Matter → Deadlines → Accounting → Close

**Scenario:** Robert Davis is an existing client whose mother has passed. He needs estate administration services. The matter involves probate, statutory deadlines, trust accounting, distributions, and final close.

### Step-by-Step Flow

| Step | Actor | Screen | Action | System Response |
|------|-------|--------|--------|-----------------|
| 1 | Robert | Phone call | Calls office main number; IVR → "Press 2 for Existing Client" | Routed to Existing Client queue |
| 2 | B. Taylor (Intake) | Screen 8 (UCaaS) | Answers call; system matches caller ID to Robert Davis | Caller context card: Robert Davis, existing client, prior matter TX-0089 (closed) |
| 3 | B. Taylor | Screen 8 | Takes notes during call: mother passed, estate administration needed | Call notes recorded |
| 4 | System | Screen 8 | Post-call: AI generates summary + suggests tasks | Summary: "Robert Davis reports mother's passing. Needs estate administration. Has copy of will and death certificate." Suggested tasks: Create new matter, Request documents |
| 5 | B. Taylor | Screen 8 | Approves summary; creates suggested tasks | Summary saved to client record; tasks created |
| 6 | B. Taylor | Screen 4 | Creates new lead for estate admin; consultation fee: WAIVED (engaged client within 12 months — prior matter TX-0089 closed Jan 2026) | Fee waived automatically. Scheduling unlocked. |
| 7 | B. Taylor | Screen 5 | Schedules consult with M. Jones (Estate Admin attorney) | Confirmation sent to Robert. Consult prep generated. |
| 8 | M. Jones (Attorney) | Screen 7 | After consult: determines pathway = PROBATE (per FSD: pathway decision step) | Matter created with Probate pathway template |
| 9 | System | Screen 7 | Initializes Estate Admin posture tracker with statutory deadlines | Deadlines auto-populated based on jurisdiction and probate type |
| 10 | S. Lee (Paralegal) | Screen 7 (Posture Tracker) | Reviews posture: Pathway Decision ✅ → Petition Filing → Notice to Creditors → Inventory → Claims Period → Distribution → Final Accounting → Close | All phases visible with statutory deadlines |
| 11 | System | Screen 7 (Risk Panel) | Statutory deadlines populated | "Petition filing: due within 30 days (Apr 23). Notice to creditors: due within 10 days of appointment. Inventory: due within 60 days." |
| 12 | S. Lee | Screen 7 | Requests documents from Robert via portal: death certificate, will, asset list, bank statements | Document checklist created; Robert notified via email + portal |
| 13 | Robert | Screen 14 (Client Portal) | Uploads death certificate and will | Documents received; marked in checklist |
| 14 | M. Jones | Screen 7 | Reviews documents; prepares petition for probate filing | Task: "File petition" — due Apr 23 |
| 15 | S. Lee | Screen 7 | Files petition with court; updates posture tracker | Stage: Petition Filing ✅. Next: Notice to Creditors |
| 16 | System | Screen 7 | Deadline countdown updates | "Notice to creditors: due [date]. Inventory: due [date]." |
| 17 | D. Kim (Billing) | Screen 9 | Generates initial invoice per fee arrangement (hourly for estate admin) | Invoice INV-0889 created: $4,500 for initial phase |
| 18 | System | Screen 11 | Trust accounting: Robert deposits $15,200 into trust for estate expenses | Trust ledger entry created for EA-0045 |
| 19 | D. Kim | Screen 11 | Requests transfer: $4,500 from trust → operating for INV-0889 | Transfer request submitted; pending CFO approval |
| 20 | D. Wilson (CFO) | Screen 11 | Approves trust transfer | Transfer executed; trust balance: $10,700. Invoice marked paid. Audit trail entry created. |
| **— Months pass: creditor claims period, asset collection, inventory filing —** |
| 21 | S. Lee | Screen 7 | Updates posture through Claims Period, Inventory phases | Each phase: tasks completed, deadlines met, documents filed |
| 22 | System | Screen 7 | 🔴 Deadline alert: "Final accounting due in 7 days" | Alert on Home screen, Matter Workspace risk panel, and Firmwide Dashboard |
| 23 | M. Jones | Screen 7 | Prepares final accounting; reviews distributions | Distribution plan prepared |
| 24 | M. Jones | Screen 11 | Approves trust disbursements to beneficiaries | Disbursement requests created: $8,000 to beneficiary |
| 25 | D. Wilson (CFO) | Screen 11 | Approves disbursements | Trust balance: $2,700 after distributions |
| 26 | M. Jones | Screen 7 | Files final accounting with court | Posture: Final Accounting ✅ |
| 27 | D. Kim | Screen 9 | Final invoice: remaining fees | Invoice generated from trust balance |
| 28 | D. Kim | Screen 11 | Final trust transfer + return remaining balance to Robert | Transfer request → CFO approves → Trust balance: $0.00 |
| 29 | M. Jones | Screen 7 | Closes matter | System checks: Trust balance $0 ✅, All tasks complete ✅, All deadlines met ✅, Documents archived ✅ → MATTER CLOSED |
| 30 | Robert | Screen 13 (Client Portal) | Sees completed status tracker | All milestones ✅. "Your estate administration is complete." |

### Key Decision Points
- **Step 6:** Fee waiver is automatic (engaged client within 12 months)
- **Step 8:** Pathway decision (probate vs. trust admin vs. small estate) per FSD — drives entire posture template
- **Step 9:** Statutory deadlines are jurisdiction-specific and auto-populated — cannot be deleted by non-CLO
- **Step 29:** Matter cannot close if trust balance >$0 (system enforced)

### What Could Go Wrong
| Risk | System Response |
|------|-----------------|
| Statutory deadline approaching | Escalating alerts: 🟡 7 days → 🔴 48 hours → 🔴🔴 OVERDUE |
| Robert doesn't upload required docs | Reminder sequence; after 2 reminders, escalated as risk |
| Trust balance insufficient for disbursement | "Insufficient trust balance" — blocks transfer |
| Beneficiary dispute | Attorney creates ethical wall if needed; matter placed on hold |
| Invoice unpaid / retainer depleted | Dunning sequence activates; matter work may pause |

---

## Journey 3: CLO Responding to Capacity Overload and Intake Throttling

**Scenario:** The CLO notices that Estate Planning intake demand is significantly outpacing attorney capacity. Multiple intake leads are requesting capacity expansion. Marketing is actively running campaigns for estate planning. The CLO needs to rebalance.

### Step-by-Step Flow

| Step | Actor | Screen | Action | System Response |
|------|-------|--------|--------|-----------------|
| 1 | System | Screen 1 (CLO Home) | Alerts fire | "⚠ Estate Planning capacity gap: -8 slots across 2 offices. 2 expansion requests pending." |
| 2 | CLO | Screen 2 (Firmwide Dashboard) | Reviews capacity heat map | Estate Planning: 4 attorneys, 84% avg utilization. Open appts: 6. Demand: 14. Gap: -8. 🔴 |
| 3 | CLO | Screen 2 | Reviews pipeline | 42 Estate Planning leads in pipeline. Conversion rate: 35%. Forecasted: 15 new engagements this month vs. capacity for 8. |
| 4 | CLO | Screen 6 (Capacity Control) | Opens Capacity Overview | Sees per-attorney breakdown: J. Smith 92% (🔴), K. Park 78% (🟢), L. Chen 45% (🔵), M. Jones 88% (🟡) |
| 5 | CLO | Screen 6 | Reviews expansion request from Main Street | "Requested by A. Johnson: Demand exceeds capacity by 4 consults. System suggests: Open 2 slots for K. Park, 1 for L. Chen." |
| 6 | CLO | Screen 6 | Evaluates: L. Chen is Tax Planning, not Estate Planning specialist | Modifies suggestion: Open 2 slots for K. Park only. L. Chen stays on Tax matters. |
| 7 | CLO | Screen 6 | Clicks "Modify & approve" for Main Street request | Approves 2 additional slots for K. Park. Intake notified. Audit trail created. |
| 8 | CLO | Screen 6 | Reviews Downtown expansion request | M. Jones at 88% with 1 deadline. System suggests: "Not recommended — M. Jones approaching capacity threshold." |
| 9 | CLO | Screen 6 | Decides to defer Downtown expansion | Clicks "Defer" with note: "Hold until next week review. M. Jones has statutory deadline priority." |
| 10 | CLO | Screen 6 → What-If Simulator | Builds scenario: What if we hire a new EP attorney starting April 7? | Adds adjustment: "New hire, EP, 40 hrs target, start Apr 7" |
| 11 | CLO | Screen 6 (Simulator) | Runs simulation | Result: Utilization drops from 84% to 72%. Open appts increase from 6 to 14. Gap eliminated. Revenue impact: +$24K/month. |
| 12 | CLO | Screen 6 | Saves scenario for leadership meeting | Scenario saved: "EP hiring scenario — Apr 2026" |
| 13 | CLO | Screen 12 (Marketing) → Demand Shaping | Reviews demand shaping recommendations | 🔴 Estate Planning: THROTTLE recommended. Pause Google Ads for EP. 🟢 Elder Law: INCREASE. 🟢 Tax: INCREASE (seasonal). |
| 14 | CLO | Screen 12 | Approves demand shaping actions | Approves: (1) Pause EP Google Ads, (2) Increase Elder Law social + content, (3) Boost Tax seasonal push |
| 15 | Marketing Coordinator | Screen 12 | Receives approval notification; executes campaign changes | EP ads paused. EL campaign launched. Tax content scheduled. |
| 16 | CLO | Screen 2 (Dashboard) | Monitors results over next week | Pipeline: EP leads decrease from 14 to 9. EL leads increase from 3 to 7. Overall capacity gap narrows to -3. |
| 17 | System | Screen 6 | Capacity engine recalculates | K. Park: 2 additional consults completed; utilization holds at 82%. Manageable. |
| 18 | CLO | Screen 1 (Home) | Next Monday review | Capacity alert resolved: "Estate Planning gap: -3 (improved from -8). Downtown expansion still deferred." |

### Key Decision Points
- **Step 6:** CLO overrides system suggestion — L. Chen is not EP-qualified. This is exactly the human judgment the system is designed to support.
- **Step 9:** CLO defers rather than denies — acknowledges demand but prioritizes deadline risk for M. Jones.
- **Step 11:** What-if simulator provides data for hiring decision without committing changes.
- **Step 14:** Demand shaping requires CLO approval — marketing cannot self-throttle/boost.

### What Could Go Wrong
| Risk | System Response |
|------|-----------------|
| K. Park overloads with 2 extra slots | System monitors; if K. Park hits 90%+, alerts CLO immediately |
| Marketing throttle is too aggressive (EP leads drop too far) | CLO monitors pipeline; can re-enable ads at any time |
| New hire doesn't materialize | CLO re-runs simulation; explores alternatives (contractor, cross-training) |
| Client complaints about wait times | Intake tracks SLA; if average wait >5 days for consultation, alert fires |
| Downtown intake lead frustrated by deferral | Intake communicates next available date; offers to add to waitlist |

### CLO Workflow Pattern
```
Monitor (Dashboard) → Analyze (Capacity Control) → Decide (Approve/Modify/Defer)
→ Simulate (What-If) → Shape Demand (Marketing) → Monitor (Dashboard)
```

This cyclical pattern is the CLO's primary operational loop for capacity management. The platform supports it as a continuous, data-driven process rather than a reactive, ad-hoc scramble.
