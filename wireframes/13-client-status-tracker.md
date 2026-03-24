# Screen 13: Client Portal — Matter Status Tracker

## Purpose
Provide clients with a "Domino's-style" visual status tracker for their matter(s). Clients can see where their case stands, what's been completed, what's next, and what's expected of them — without exposing internal complexity, work product, or strategy details.

## Primary Users
Client (portal user) — primary and sole user of this screen

## When This Screen Is Used (Workflow Stage)
From Engaged through Closed. Clients access after engagement is formalized and portal credentials are issued.

---

## LAYOUT (TEXTUAL WIREFRAME)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ CLIENT PORTAL TOP BAR                                                   │
│ [Firm Logo]                    [Maria Garcia ▾]  [📩 Messages (1)]     │
│                                [Sign Out]                               │
├─────────────────────────────────────────────────────────────────────────┤
│ CLIENT PORTAL NAV (horizontal)                                          │
│ [My Matters ●] [Documents] [Messages] [Payments] [Resources]          │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│ MY MATTERS                                                              │
│                                                                         │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Estate Plan — #EP-0147                                            │  │
│ │ Attorney: K. Park  │  Opened: March 24, 2026                     │  │
│ │                                                                   │  │
│ │ ═══════════════════ STATUS TRACKER ═══════════════════            │  │
│ │                                                                   │  │
│ │  ●────────●────────●────────◉────────○────────○────────○         │  │
│ │  Consult  Engaged  Planning DRAFTING Review  Signing  Complete   │  │
│ │  ✅       ✅       ✅       🔵       ○       ○        ○          │  │
│ │                                                                   │  │
│ │ ┌─────────────────────────────────────────────────────────────┐  │  │
│ │ │ 📍 CURRENT STEP: Drafting Your Documents                    │  │  │
│ │ │                                                             │  │  │
│ │ │ Your attorney and team are preparing your estate planning   │  │  │
│ │ │ documents. This typically takes 2–3 weeks.                  │  │  │
│ │ │                                                             │  │  │
│ │ │ Estimated completion: April 7, 2026                        │  │  │
│ │ │                                                             │  │  │
│ │ │ What's happening:                                           │  │  │
│ │ │ ✅ Will — drafted                                           │  │  │
│ │ │ 🔵 Trust document — in progress                            │  │  │
│ │ │ ○  Power of Attorney — upcoming                            │  │  │
│ │ └─────────────────────────────────────────────────────────────┘  │  │
│ │                                                                   │  │
│ │ ┌─────────────────────────────────────────────────────────────┐  │  │
│ │ │ 📋 ACTION NEEDED FROM YOU                                   │  │  │
│ │ │                                                             │  │  │
│ │ │ ⚠ Please upload the following documents:                   │  │  │
│ │ │                                                             │  │  │
│ │ │ □ Deed copies for your properties                          │  │  │
│ │ │   [Upload →]                                               │  │  │
│ │ │                                                             │  │  │
│ │ │ □ Beneficiary designation forms                            │  │  │
│ │ │   [Upload →]                                               │  │  │
│ │ │                                                             │  │  │
│ │ │ These documents are needed to continue drafting.           │  │  │
│ │ │ If you have questions: [Send a message →]                  │  │  │
│ │ └─────────────────────────────────────────────────────────────┘  │  │
│ │                                                                   │  │
│ │ ┌─────────────────────────────────────────────────────────────┐  │  │
│ │ │ 📜 WHAT'S BEEN COMPLETED                                   │  │  │
│ │ │                                                             │  │  │
│ │ │ Mar 24 — Initial consultation with K. Park ✅              │  │  │
│ │ │ Mar 24 — Engagement letter signed ✅                       │  │  │
│ │ │ Mar 25 — Planning scope finalized ✅                       │  │  │
│ │ │ Mar 28 — Will drafted ✅                                   │  │  │
│ │ └─────────────────────────────────────────────────────────────┘  │  │
│ │                                                                   │  │
│ │ ┌─────────────────────────────────────────────────────────────┐  │  │
│ │ │ 📅 WHAT'S NEXT                                             │  │  │
│ │ │                                                             │  │  │
│ │ │ After drafting is complete, you will:                      │  │  │
│ │ │ 1. Review all documents with your attorney                 │  │  │
│ │ │ 2. Schedule a signing ceremony                             │  │  │
│ │ │ 3. Receive your completed estate plan                      │  │  │
│ │ │                                                             │  │  │
│ │ │ Estimated timeline: 3–4 weeks remaining                    │  │  │
│ │ └─────────────────────────────────────────────────────────────┘  │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ── OTHER MATTERS (if applicable) ──                                    │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Tax Planning — #TX-0201 (Closed Jan 2026)                        │  │
│ │ ●────────●────────●────────●────────●  COMPLETED ✅              │  │
│ │ [View details]                                                    │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ 💡 RESOURCES                                                      │  │
│ │ • What to expect during estate planning [Read →]                 │  │
│ │ • Understanding your trust document [Read →]                     │  │
│ │ • FAQ: Powers of Attorney [Read →]                               │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ 📞 NEED HELP?                                                    │  │
│ │ [Send a message →]  │  Call: (555) 100-1000  │  [Schedule call →]│  │
│ └───────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## KEY COMPONENTS

### 1. Status Tracker (Domino's-Style)
- **Description:** Horizontal progress bar with labeled milestones. Client-friendly labels (NOT internal stage names).
- **Data displayed:** Milestone dots (completed ●, current ◉, upcoming ○), milestone labels, current step highlighted
- **Milestone mapping (internal → client-facing):**
  - Consult Completed → "Consultation" ✅
  - Engaged → "Engaged" ✅
  - Scope/Price complete → "Planning" ✅
  - Drafting → "Drafting Your Documents" 🔵
  - Client Review → "Review" ○
  - Execution → "Signing" ○
  - Delivery/Close → "Complete" ○
- **Practice-specific:** Milestone labels adapt per matter type (estate admin has different labels than estate planning)
- **[ASSUMPTION]** Maximum 7 milestones per tracker to maintain visual clarity

### 2. Current Step Detail
- **Description:** Plain-language explanation of what's happening at the current stage
- **Data displayed:** Step name, human-friendly description, estimated completion date, sub-step progress
- **NO legal strategy, work product, or internal notes shown** — this is status only

### 3. Action Needed From You
- **Description:** Prominent call-to-action for items the client needs to complete
- **Data displayed:** Pending document uploads, required responses, scheduling needs
- **Actions:** Upload document, Send message, Schedule appointment (via link)
- **Styling:** Yellow/amber highlight to draw attention; collapses when no actions pending

### 4. What's Been Completed
- **Description:** Chronological list of completed milestones with dates
- **Data displayed:** Date, milestone description (client-friendly), checkmark
- **No internal task details, hours, or team member actions shown**

### 5. What's Next
- **Description:** Forward-looking explanation of upcoming steps
- **Data displayed:** Numbered list of upcoming milestones with plain-language descriptions; estimated timeline

### 6. Educational Resources
- **Per FSD:** Educational materials accessible from portal
- **Data displayed:** Relevant resources linked to the matter's practice area
- **[ASSUMPTION]** Content managed by Marketing Coordinator; curated by practice area

---

## INTERACTIONS & STATES

### Default State
Active matter(s) displayed with current tracker position. Most recent matter first.

### Empty State
- **No matters:** "Welcome, [Name]. You don't have any active matters yet. If you believe this is an error, please contact us."
- **All matters closed:** "All your matters are complete. [View history] [Contact us for new matters]"

### Loading State
Tracker skeleton with milestone dots shimmer. Content loads progressively.

### Error State
"We're having trouble loading your matter status. Please try again in a few moments. If the issue persists, call us at (555) 100-1000."

### Blocked/Gated State (Client Perspective)
- **Action needed:** Amber banner: "We need something from you to continue. See details below."
- **Fee overdue:** "A payment is due on your account. [View payments →]" — status tracker continues to show but note indicates potential delay

### Success State
- Document uploaded: "Document received! Your team will review it shortly." ✅
- Message sent: "Message sent. We typically respond within [X] business hours."

---

## AUTOMATIONS & AI ASSISTS

### What the system does automatically
- Status tracker updates when internal posture changes (no client action needed)
- Client-friendly milestone descriptions auto-generated from internal stage transitions
- Educational resources auto-suggested based on matter type and current stage

### What requires human approval
- All status updates originate from internal staff actions — tracker reflects those
- System does NOT show AI-generated legal summaries to clients

### How information is surfaced
- Client sees curated, simplified view only
- Internal complexity is completely hidden
- **Per FSD "Clients: status and expectations, not internal complexity"**

---

## ROLE-BASED VISIBILITY

### Client
- **Only user** of this screen
- Sees: status tracker, action items, completed milestones, next steps, resources, help options
- **Does NOT see:** Internal task details, AI suggestions, attorney notes, work product, strategy, pricing details, underwriting scores, capacity data
- **Can:** Upload documents, send messages, view resources, navigate to payments
- **Cannot:** Edit any status, modify the tracker, access internal systems

### All Internal Roles
- **Do not access this screen.** Internal staff view client-facing status on the Matter Workspace (Screen 7) and can preview what the client sees via a "Preview client view" button (on Screen 7).

---

## EDGE CASES & GUARDRAILS

### Capacity exceeded
- Not visible to client. If capacity issues cause delays, the "estimated completion" date adjusts and a message appears: "Your estimated timeline has been updated."

### Payment fails
- If payment is past due: subtle banner "A payment is due on your account. [View payments →]" — does NOT block status tracker visibility

### Required documents missing
- "Action Needed" section prominently lists missing documents
- If client hasn't uploaded after reminder: additional emphasis ("These documents are needed to continue" messaging)

### Deadlines at risk
- Client does NOT see "deadline at risk" language (internal concept)
- Instead: estimated dates adjust and messaging says "Your team is working to keep your matter on schedule"

### Matter on hold
- If matter is paused (retainer depleted, client unresponsive): tracker shows current step with "Paused" indicator and explanation: "Your matter is temporarily on hold. [Learn why →]"

### Multiple matters
- All client matters listed; most recent/active first; closed matters shown below in collapsed state

### Mobile responsiveness
- **[ASSUMPTION]** Client portal is mobile-first; status tracker converts to vertical layout on small screens
- Milestone dots become a vertical timeline
- All actions (upload, message, pay) accessible from mobile
