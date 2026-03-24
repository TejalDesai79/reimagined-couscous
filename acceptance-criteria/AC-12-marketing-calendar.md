# AC-12: Marketing & Content Calendar

## Feature Overview
Marketing campaign management, AI-assisted content creation with approval workflows, referral/contact CRM tracking, funnel analytics with attribution, and capacity-aware demand shaping.

---

## AC-12.1 — Access Control

| Feature | Marketing | CLO | CEO | Attorney |
|---------|-----------|-----|-----|----------|
| Content Calendar | Full R/W | Read + Approve (final) | Read-only | Review assigned content only |
| Content creation/editing | ✅ | — | — | — |
| Approval (final) | — | ✅ | — | ✅ (review step only) |
| Funnel & Attribution | Full R/W | Read-only | Read-only | — |
| Demand Shaping | View + request | Approve/deny | Read-only | — |
| Public Signal Monitoring | View + flag | Approve outreach | — | — |
| AI Topic Suggestions | View + use | Read-only | — | — |

**Given** a user with role Intake, Paralegal, Billing, or Client
**Then** they have no access to this screen

---

## AC-12.2 — Content Calendar Tab (Default)

**Given** the Content Calendar loads
**Then** it shows content items (blog posts, newsletters, social media posts) on a monthly calendar with status indicators

**Given** content statuses are displayed
**Then** they are: 🤖 AI Draft, 🔵 In Draft, 🟡 In Review, ✅ Approved, 📅 Scheduled, ✅ Published

**Given** a user clicks a content item
**Then** a content detail slide-over opens

**Given** the content detail slide-over opens
**Then** it shows: title, status, author, reviewer, publish date, topic basis (with 🤖 indicator if AI-suggested), content preview, and approval workflow steps

**Given** the approval workflow steps are displayed
**Then** the stages are:
1. AI draft generated
2. Marketing review & edit
3. Attorney review (assigned attorney)
4. CLO approval
5. Publish

**Given** available actions in the slide-over (role-appropriate)
**Then** they include: [Edit], [Send for review], [Approve], [Reject w/ notes], [Schedule]

---

## AC-12.3 — Content Approval Workflow (Gating)

**Given** content has not completed all approval steps
**Then** the Publish button is disabled — content cannot be published without completing the full approval workflow

**Given** an attorney reviews content
**Then** the review step explicitly asks: "Does this content contain legal conclusions?" — if yes, the content must be revised before proceeding

**Given** all approval steps are complete
**Then** the content can be scheduled or published

**Given** CLO gives final approval
**Then** the workflow updates with a checkmark on the CLO approval step

---

## AC-12.4 — AI Topic Suggestions

**Given** AI topic suggestions are available
**Then** they are displayed with a 🤖 indicator and an explanation of the basis (seasonality, practice demand, regulatory changes, intake trends)

**Given** a marketing coordinator uses an AI topic suggestion
**Then** the content item is pre-populated with the suggested topic and marked as 🤖 AI Draft

---

## AC-12.5 — Campaigns Tab

**Given** the Campaigns tab is active
**Then** active campaigns are listed with their status and performance data

---

## AC-12.6 — Funnel & Attribution Tab

**Given** the Funnel & Attribution tab is active
**Then** it shows per lead source: lead count, qualified count (and % conversion), engaged count, revenue attributed, and ROI

**Given** available filters are used
**Then** drill-downs by practice area and office are available

**Given** a user clicks "Export"
**Then** a data export is triggered

**Given** available lead sources include
**Then** at minimum: Attorney Referral, Website/SEO, Google Ads, Newsletter, Social Media, Public Signal

---

## AC-12.7 — Demand Shaping Tab

**Given** the Demand Shaping tab is active
**Then** it shows per practice area: capacity status (🔴 Over / 🟢 Under), demand level, and AI recommendation (⬇ THROTTLE or ⬆ INCREASE)

**Given** AI recommendations are shown
**Then** they are labeled: "⚠ Recommendations only — changes require CLO approval"

**Given** a marketing coordinator clicks "Request CLO approval for recommendations"
**Then** an approval request is sent to CLO

**Given** CLO approves demand shaping changes
**Then** the marketing coordinator receives an approval notification and can execute the campaign changes

**Given** demand shaping changes are executed without CLO approval
**Then** the system prevents this — marketing can request but cannot self-execute throttle/boost changes

**Given** capacity data used for recommendations is stale
**Then** a notice displays: "Capacity data last updated: [timestamp]. Recommendations may not reflect current state."

---

## AC-12.8 — Public Signal Monitoring

**Given** the Public Signal Monitoring section is visible
**Then** a permanent compliance notice displays at the top: "Public signal monitoring is for informational purposes only. All outreach must comply with applicable bar rules. Opt-out requests are immediately honored."

**Given** public signals are listed
**Then** each entry shows: date, source (obituary / court record), summary, and available actions

**Given** available actions per signal entry
**Then** they are: [Flag for review], [Dismiss], [Opt-out]

**Given** a signal entry is dismissed
**Then** it is removed from the active list

**Given** an opt-out is recorded
**Then** the opt-out is immediate and permanent; the associated contact is added to the opt-out list

**Given** a user clicks "Manage opt-outs →"
**Then** the full opt-out list is accessible

**Given** a user clicks "Configure sources →"
**Then** they can manage which reputable sources are monitored

**Given** any outreach based on public signals
**Then** it is blocked by default — requires explicit flag-for-review and compliance check; no automated outreach is triggered

---

## AC-12.9 — AI-Generated Content Review

**Given** AI-generated content is marked for legal review
**Then** the attorney review step explicitly flags: "Does this content contain legal conclusions?"

**Given** AI-generated content contains legal conclusions
**Then** the content must be revised and re-reviewed before it can proceed through the approval workflow

---

## AC-12.10 — Empty State

**Given** no content is scheduled
**Then** the message displays: "No content scheduled. [Create first content item] or [Review AI topic suggestions]"

---

## AC-12.11 — Success States

**Given** content is published
**Then** a toast displays: "Blog post published successfully"

**Given** an approval is received
**Then** the workflow step updates with a checkmark
