# Screen 12: Marketing & Content Calendar

## Purpose
Manage marketing campaigns, content creation (blog/newsletter posts with AI-assisted drafting), referral/contact CRM tracking, funnel analytics with attribution, and demand shaping. Integrates with capacity data to throttle or amplify marketing based on firm capacity.

## Primary Users
Marketing Coordinator (primary), CLO (approval, demand shaping oversight), CEO/Managing Partner (read access)

## When This Screen Is Used (Workflow Stage)
Marketing & Business Development workflow. Ongoing content planning, campaign management, and performance analysis.

---

## LAYOUT (TEXTUAL WIREFRAME)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ [Global Nav Shell — see Screen 1]                                       │
├─────────────────────────────────────────────────────────────────────────┤
│ MARKETING HEADER                                                        │
│ Marketing & Business Dev  [+ New Content] [+ New Campaign]       [⟳]  │
│ [Content Calendar] [Campaigns] [Funnel & Attribution] [Demand Shaping] │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│ TAB: CONTENT CALENDAR (default)                                         │
│                                                                         │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ [◀ March 2026 ▶]  [Month] [Week] [List]  Filter: [Type ▾] [Status▾]│
│ │                                                                   │  │
│ │ Mon       │ Tue       │ Wed       │ Thu       │ Fri              │  │
│ │ ──────────┼───────────┼───────────┼───────────┼────────────────  │  │
│ │ 23        │ 24        │ 25        │ 26        │ 27               │  │
│ │           │ 📝 Blog   │           │ 📧 News-  │                  │  │
│ │           │ "Estate   │           │ letter    │                  │  │
│ │           │ Planning  │           │ Q1 Recap  │                  │  │
│ │           │ for Young │           │ ✅ Approved│                  │  │
│ │           │ Families" │           │ Sched: 9AM│                  │  │
│ │           │ 🟡 Review │           │           │                  │  │
│ │ ──────────┼───────────┼───────────┼───────────┼────────────────  │  │
│ │ 30        │ 31        │ Apr 1     │ 2         │ 3                │  │
│ │ 📝 Blog   │           │ 📱 Social │           │ 📝 Blog          │  │
│ │ "Elder Law│           │ Campaign  │           │ "Tax Season     │  │
│ │ Basics"   │           │ Launch    │           │ Checklist"      │  │
│ │ 🔵 Draft  │           │ ⏳ Planned│           │ 🤖 AI Draft     │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ CONTENT DETAIL (slide-over on click)                                    │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Blog Post: "Estate Planning for Young Families"                   │  │
│ │ Status: 🟡 Pending Review                                        │  │
│ │ Author: Marketing (AI-assisted draft)                             │  │
│ │ Reviewer: K. Park (Attorney)                                     │  │
│ │ Publish date: Mar 24, 2026                                       │  │
│ │                                                                   │  │
│ │ Topic basis: 🤖 AI Suggestion                                    │  │
│ │ Reason: High intake volume for estate planning +                 │  │
│ │   young families; seasonal (spring planning season)              │  │
│ │                                                                   │  │
│ │ ┌─────────────────────────────────────────────────────────────┐  │  │
│ │ │ CONTENT PREVIEW                                             │  │  │
│ │ │                                                             │  │  │
│ │ │ [Rich text preview of blog post...]                        │  │  │
│ │ │                                                             │  │  │
│ │ │ 🤖 AI-drafted | Last edited: Mar 23 by S. Torres           │  │  │
│ │ └─────────────────────────────────────────────────────────────┘  │  │
│ │                                                                   │  │
│ │ Approval workflow:                                                │  │
│ │ 1. ✅ AI draft generated (Mar 22)                                │  │
│ │ 2. ✅ Marketing review & edit (Mar 23, S. Torres)                │  │
│ │ 3. ⏳ Attorney review (assigned: K. Park) ← CURRENT             │  │
│ │ 4. ⬜ CLO approval                                               │  │
│ │ 5. ⬜ Publish                                                    │  │
│ │                                                                   │  │
│ │ [Edit] [Send for review] [Approve] [Reject w/ notes] [Schedule] │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: FUNNEL & ATTRIBUTION                                               │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Lead Attribution (Trailing 90 days)                                │  │
│ │                                                                   │  │
│ │ Source           │ Leads │ Qualified │ Engaged │ Revenue  │ ROI  │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ Attorney Referral│ 42    │ 35 (83%)  │ 28      │ $134K    │ —    │  │
│ │ Website/SEO      │ 68    │ 31 (46%)  │ 18      │ $72K     │ 6.0x │  │
│ │ Google Ads       │ 34    │ 12 (35%)  │ 7       │ $28K     │ 2.3x │  │
│ │ Newsletter       │ 15    │ 8 (53%)   │ 5       │ $24K     │ 8.0x │  │
│ │ Social Media     │ 22    │ 6 (27%)   │ 3       │ $12K     │ 1.5x │  │
│ │ Public Signal    │ 8     │ 5 (63%)   │ 4       │ $19K     │ —    │  │
│ │                                                                   │  │
│ │ [View by practice area] [View by office] [Export]                │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: DEMAND SHAPING                                                     │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Capacity-Aware Marketing Recommendations         🤖 AI           │  │
│ │                                                                   │  │
│ │ Practice Area    │ Capacity │ Demand  │ Recommendation           │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ Estate Planning  │ 🔴 Over  │ High    │ ⬇ THROTTLE marketing    │  │
│ │                  │          │         │ Pause Google Ads for EP  │  │
│ │ Elder Law        │ 🟢 Under │ Moderate│ ⬆ INCREASE marketing    │  │
│ │                  │          │         │ Boost social + content   │  │
│ │ Tax Planning     │ 🟢 Under │ Low     │ ⬆ INCREASE marketing    │  │
│ │                  │          │         │ Seasonal push recommended│  │
│ │ Estate Admin     │ 🔴 Over  │ High    │ ⬇ THROTTLE marketing    │  │
│ │                  │          │         │ Reduce ad spend          │  │
│ │                                                                   │  │
│ │ ⚠ Recommendations only — changes require CLO approval            │  │
│ │ [Request CLO approval for recommendations] [Customize]           │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ PUBLIC SIGNAL MONITORING (sub-section of Demand Shaping)                │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ ⚠ Ethics/compliance notice: Public signal monitoring is for       │  │
│ │   informational purposes only. All outreach must comply with      │  │
│ │   applicable bar rules. Opt-out requests are immediately honored. │  │
│ │                                                                   │  │
│ │ Recent signals (obituaries/news — reputable sources only):       │  │
│ │                                                                   │  │
│ │ Date  │ Source    │ Summary                │ Action              │  │
│ │ ────────────────────────────────────────────────────────────────│  │
│ │ Mar 23│ Obituary  │ [Name], [County] — est.│ [Flag for review]  │  │
│ │       │           │ surviving family noted  │ [Dismiss] [Opt-out]│  │
│ │ Mar 22│ Court rec │ Probate filing — [Name] │ [Flag for review]  │  │
│ │                                                                   │  │
│ │ [Configure sources →] [Manage opt-outs →]                        │  │
│ └───────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## KEY COMPONENTS

### 1. Content Calendar
- **Data:** Content items (blog, newsletter, social) on a calendar with status indicators
- **Statuses:** 🤖 AI Draft, 🔵 In Draft, 🟡 In Review, ✅ Approved, 📅 Scheduled, ✅ Published
- **Actions:** Create content, Edit, Submit for review, Schedule, Publish

### 2. Content Approval Workflow
- **Per FSD:** Automated drafting with approval workflow and audit trail
- **Steps:** AI draft → Marketing edit → Attorney review → CLO approval → Publish
- **All steps logged for audit trail**

### 3. AI Topic Suggestions
- **Per FSD:** Based on seasonality, practice demand, regulatory changes, intake trends
- **Displayed as suggested content items with 🤖 indicator and explanation**

### 4. Funnel & Attribution Analytics
- **Data:** Lead source → qualification → engagement → revenue with ROI per channel
- **Actions:** Drill down by practice/office, export, compare periods

### 5. Demand Shaping
- **Per FSD:** Increase marketing when capacity exists; throttle when constrained
- **Data:** Practice area capacity status, demand level, AI recommendation
- **Actions:** Request CLO approval for throttle/boost changes
- **All changes require CLO approval**

### 6. Public Signal Monitoring
- **Per FSD:** Reputable obituaries/news with ethics/opt-out/compliance safeguards
- **Prominent ethics notice displayed permanently**
- **Actions:** Flag for review, Dismiss, Opt-out; Configure sources, Manage opt-outs

---

## INTERACTIONS & STATES

### Default State
Content calendar showing current month. Content items populated.

### Empty State
"No content scheduled. [Create first content item] or [Review AI topic suggestions]"

### Blocked/Gated States
- Content cannot be published without completing approval workflow
- Demand shaping changes cannot execute without CLO approval
- Public signal outreach is blocked by default — requires explicit flag-for-review and compliance check

### Success State
- Content published: toast "Blog post published successfully"
- Approval received: workflow step updates with checkmark

---

## ROLE-BASED VISIBILITY

| Feature | Marketing | CLO | CEO | Attorney |
|---------|-----------|-----|-----|----------|
| Content Calendar | Full R/W | Read + Approve | Read-only | Review assigned content |
| Content creation/editing | ✅ | — | — | — |
| Approval (final) | — | ✅ | — | ✅ (review step) |
| Funnel & Attribution | Full R/W | Read-only | Read-only | — |
| Demand Shaping | View + request | Approve/deny | Read-only | — |
| Public Signal Monitoring | View + flag | Approve outreach | — | — |
| Topic suggestions | View + use | Read-only | — | — |

All other roles: **No access.**

---

## EDGE CASES & GUARDRAILS

- **Content published without approval:** System prevents this — publish button disabled until all approval steps complete
- **Demand shaping applied without CLO:** System prevents — marketing can request but CLO must approve
- **Public signal — ethics violation risk:** Permanent compliance banner; no automated outreach; opt-out is immediate and permanent
- **AI-generated content flagged as legal advice:** Content review workflow explicitly asks attorney reviewer: "Does this content contain legal conclusions?" If yes, content must be revised.
- **Capacity data stale:** Demand shaping shows "Capacity data last updated: [timestamp]. Recommendations may not reflect current state."
