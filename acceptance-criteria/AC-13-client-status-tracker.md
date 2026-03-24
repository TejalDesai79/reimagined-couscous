# AC-13: Client Portal — Matter Status Tracker

## Feature Overview
"Domino's-style" visual status tracker for clients to see where their matter stands, what's been completed, what's next, and what actions they need to take — without exposing internal complexity.

---

## AC-13.1 — Access Control

**Given** any internal staff user navigates to this screen
**Then** they do NOT see this screen — internal staff preview client view via "Preview client view" on Screen 7

**Given** an authenticated client logs into the portal
**Then** the My Matters page is their default landing screen

**Given** the client has not been issued portal credentials
**Then** they cannot access this screen

---

## AC-13.2 — Client Portal Shell

**Given** a client is viewing the portal
**Then** the top bar shows: firm logo, client name (with dropdown), message badge with unread count, and Sign Out

**Given** the horizontal navigation is displayed
**Then** the tabs are: My Matters (active), Documents, Messages, Payments, Resources

---

## AC-13.3 — Status Tracker (Visual)

**Given** a client views their active matter
**Then** the status tracker shows a horizontal progress bar with milestone dots:
- Completed milestones: filled dot ●
- Current milestone: open dot with ring ◉
- Upcoming milestones: empty dot ○

**Given** the tracker displays milestone labels
**Then** they use client-friendly language (NOT internal stage names):
- Consult Completed → "Consultation"
- Engaged → "Engaged"
- Scope/Price complete → "Planning"
- Drafting → "Drafting Your Documents"
- Client Review → "Review"
- Execution → "Signing"
- Delivery/Close → "Complete"

**Given** the practice area is not estate planning
**Then** the milestone labels adapt to that practice area's client-friendly terminology

**Given** a matter type is displayed
**Then** the tracker shows a maximum of 7 milestones to maintain visual clarity

**Given** the internal posture tracker is updated by staff
**Then** the client-facing status tracker updates automatically (no client action required)

---

## AC-13.4 — Current Step Detail

**Given** the current step is displayed
**Then** it shows: step name (client-friendly), a plain-language explanation of what's happening, and an estimated completion date

**Given** the current step shows sub-step progress (e.g., drafting documents)
**Then** individual document sub-items show: completed ✅, in progress 🔵, upcoming ○

**Given** the current step detail is shown
**Then** NO legal strategy, work product, attorney notes, or internal information is exposed

---

## AC-13.5 — Action Needed from You

**Given** the client has pending actions
**Then** the "Action Needed From You" section is prominently displayed with amber/yellow highlighting

**Given** pending actions are listed
**Then** each item shows: the action required (e.g., document upload) and an action button ([Upload →])

**Given** a client uploads a required document
**Then** the document status updates and a success message displays: "Document received! Your team will review it shortly." ✅

**Given** no client actions are pending
**Then** the "Action Needed" section collapses and is not displayed

---

## AC-13.6 — What's Been Completed

**Given** the completed milestones section is displayed
**Then** it shows a chronological list of completed milestones with date and client-friendly description

**Given** internal task details, staff hours, or team member actions are referenced
**Then** they are NOT shown to the client — only client-visible milestones appear

---

## AC-13.7 — What's Next

**Given** the "What's Next" section is displayed
**Then** it shows a numbered list of upcoming steps with plain-language descriptions and an estimated remaining timeline

---

## AC-13.8 — Client Actions Available

**Given** a client is viewing the portal
**Then** they can: upload documents, send messages, view resources, navigate to payments

**Given** a client attempts to edit matter status or the tracker
**Then** these actions are not available — clients cannot modify any matter data

---

## AC-13.9 — Multiple Matters

**Given** a client has multiple matters
**Then** all matters are listed with the most recent/active matter first

**Given** a matter is closed
**Then** it is shown below active matters in a collapsed state with: "[Matter Name] — Completed ✅" and a [View details] link

---

## AC-13.10 — Educational Resources Section

**Given** the resources section is displayed
**Then** practice-area-relevant educational articles are shown, curated based on the matter's practice area and current stage

**Given** general resources are displayed
**Then** they include general guides on working with legal teams

---

## AC-13.11 — Help/Contact Section

**Given** the help section is displayed
**Then** it shows: [Send a message →], office phone number, and [Schedule call →]

---

## AC-13.12 — Edge Cases: Capacity & Delays

**Given** capacity issues cause a delay in the matter
**Then** the estimated completion date adjusts; the message displays: "Your estimated timeline has been updated." — internal "capacity exceeded" language is NOT shown to the client

---

## AC-13.13 — Edge Cases: Deadlines

**Given** a statutory deadline is at risk internally
**Then** the client does NOT see "deadline at risk" language — instead, estimated dates adjust and the message says: "Your team is working to keep your matter on schedule"

---

## AC-13.14 — Edge Cases: Matter on Hold

**Given** a matter is paused (retainer depleted, client unresponsive, etc.)
**Then** the tracker shows the current step with a "Paused" indicator and the message: "Your matter is temporarily on hold. [Learn why →]"

---

## AC-13.15 — Edge Cases: Payment Due

**Given** a payment is past due on the client's account
**Then** a subtle banner shows: "A payment is due on your account. [View payments →]" — the status tracker remains fully visible and functional

---

## AC-13.16 — Loading State

**Given** the portal is loading
**Then** the tracker skeleton shows with milestone dots shimmering; content loads progressively

---

## AC-13.17 — Error State

**Given** the portal fails to load
**Then** the message displays: "We're having trouble loading your matter status. Please try again in a few moments. If the issue persists, call us at [office number]."

---

## AC-13.18 — Empty States

**Given** the client has no matters
**Then** the message displays: "Welcome, [Name]. You don't have any active matters yet. If you believe this is an error, please contact us."

**Given** all client matters are closed
**Then** the message displays: "All your matters are complete. [View history] [Contact us for new matters]"

---

## AC-13.19 — Mobile Responsiveness

**Given** the client accesses the portal on a mobile device
**Then** the status tracker converts to a vertical timeline layout

**Given** the vertical mobile layout is displayed
**Then** all actions (upload, message, pay) remain accessible from mobile
