# Screen 8: Embedded UCaaS Console (In-App Communications)

## Purpose
Provide a unified communications interface within the platform: phone calls (via per-office main numbers with IVR), firmwide directory, live queue views, call handling, and communication timeline integration. Eliminates context switching between the legal operations platform and external phone/messaging systems.

## Primary Users
Intake Specialist (call handling), Attorney (calls), Paralegal (calls), Billing Specialist (collections calls), CLO/COO (supervisor monitoring)

## When This Screen Is Used (Workflow Stage)
All workflow stages. Persistent softphone accessible from global nav; full console for queue management and supervisor oversight.

---

## LAYOUT (TEXTUAL WIREFRAME)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ [Global Nav Shell — see Screen 1]                                       │
├─────────────────────────────────────────────────────────────────────────┤
│ UCaaS HEADER                                                            │
│ Communications Console    [Office: Main St ▾]     [My Status: 🟢 ▾]   │
│ [Queue Monitor] [Directory] [Call History] [IVR Config]                 │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│ TAB: QUEUE MONITOR (default for Intake)                                 │
│                                                                         │
│ ┌────────────────────────────┐ ┌──────────────────────────────────────┐│
│ │ LIVE QUEUES                │ │ ACTIVE CALL / SOFTPHONE              ││
│ │                            │ │                                      ││
│ │ 1 - New Client Intake      │ │ ┌──────────────────────────────────┐││
│ │ ┌────────────────────────┐│ │ │ Incoming: (555) 234-5678         │││
│ │ │ 📞 (555) 234-5678     ││ │ │ Queue: New Client Intake         │││
│ │ │ Waiting: 0:42          ││ │ │ Wait time: 0:42                  │││
│ │ │ [Answer] [Transfer]    ││ │ │ Caller ID match: No match        │││
│ │ ├────────────────────────┤│ │ │                                  │││
│ │ │ 📞 (555) 345-6789     ││ │ │ [🟢 Answer]  [➡ Transfer ▾]     │││
│ │ │ Waiting: 0:18          ││ │ │ [📝 Callback] [❌ Send to VM]   │││
│ │ │ Match: Garcia, Maria   ││ │ └──────────────────────────────────┘││
│ │ └────────────────────────┘│ │                                      ││
│ │                            │ │ ── ON CALL ──                       ││
│ │ 2 - Existing Client        │ │ ┌──────────────────────────────────┐││
│ │ ┌────────────────────────┐│ │ │ 📞 Active: (555) 345-6789       │││
│ │ │ 📞 (555) 456-7890     ││ │ │ Client: Maria Garcia             │││
│ │ │ Waiting: 1:05          ││ │ │ Matter: #2026-EP-0147            │││
│ │ │ Match: Williams, R.    ││ │ │ Duration: 3:42                   │││
│ │ │ Matter: #2026-TX-0089  ││ │ │                                  │││
│ │ └────────────────────────┘│ │ │ [🔇 Mute] [⏸ Hold] [➡ Transfer]│││
│ │                            │ │ │ [📝 Notes:                      │││
│ │ 3 - Billing                │ │ │  Client confirming she will     │││
│ │ ┌────────────────────────┐│ │ │  send deed copies by Friday.    │││
│ │ │ (empty)                ││ │ │  _________________________]      │││
│ │ └────────────────────────┘│ │ │                                  │││
│ │                            │ │ │ [🔚 End Call]                   │││
│ │ ── Agent Status ──         │ │ └──────────────────────────────────┘││
│ │ A.Johnson  🟢 Available   │ │                                      ││
│ │ B.Taylor   📞 On call     │ │ ── POST-CALL ──                     ││
│ │ C.Martinez 🟡 Away        │ │ ┌──────────────────────────────────┐││
│ │                            │ │ │ Call Summary         🤖 AI      │││
│ │                            │ │ │                                  │││
│ │                            │ │ │ Client confirmed deed copies     │││
│ │                            │ │ │ will be sent by Friday, Mar 28.  │││
│ │                            │ │ │ Discussed trust provisions for   │││
│ │                            │ │ │ minor children — no changes.     │││
│ │                            │ │ │                                  │││
│ │                            │ │ │ [Edit] [Approve & Save to Matter]│││
│ │                            │ │ │                                  │││
│ │                            │ │ │ Suggested tasks:     🤖 AI      │││
│ │                            │ │ │ □ Follow up if deeds not         │││
│ │                            │ │ │   received by Mar 28             │││
│ │                            │ │ │ □ Update trust draft re: minors  │││
│ │                            │ │ │                                  │││
│ │                            │ │ │ [Create selected tasks]          │││
│ │                            │ │ └──────────────────────────────────┘││
│ └────────────────────────────┘ └──────────────────────────────────────┘│
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: DIRECTORY                                                          │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Firmwide Directory    [Search: ________________🔍]               │  │
│ │                                                                   │  │
│ │ Name          │ Office    │ Role      │ Extension │ Status      │  │
│ │ ────────────────────────────────────────────────────────────────│  │
│ │ K. Park       │ Main St   │ Attorney  │ x2201     │ 🟢 Avail   │  │
│ │ L. Chen       │ Main St   │ Attorney  │ x2202     │ 📞 On call │  │
│ │ J. Smith      │ Main St   │ Attorney  │ x2203     │ 🔴 DND     │  │
│ │ A. Johnson    │ Main St   │ Intake    │ x1101     │ 🟢 Avail   │  │
│ │ S. Lee        │ Main St   │ Paralegal │ x3301     │ 🟡 Away    │  │
│ │ ...                                                               │  │
│ │                                                                   │  │
│ │ [Click name to call] [Click office to filter]                    │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: IVR CONFIG (CLO/COO only)                                          │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Office: Main Street    [Main Number: (555) 100-1000]              │  │
│ │                                                                   │  │
│ │ IVR Menu Options:                                                 │  │
│ │ 1. "Press 1 for New Client Intake"  → Queue: New Client          │  │
│ │ 2. "Press 2 for Existing Client"    → Queue: Existing Client     │  │
│ │ 3. "Press 3 for Billing"            → Queue: Billing             │  │
│ │ 0. "Press 0 for operator"           → Queue: General             │  │
│ │                                                                   │  │
│ │ Business Hours: Mon–Fri 8:00 AM – 6:00 PM                       │  │
│ │ After Hours: Voicemail → auto-email to intake@firm.com           │  │
│ │ Holiday Schedule: [Configure →]                                   │  │
│ │                                                                   │  │
│ │ [Edit IVR] [Test IVR flow] [Save]                                │  │
│ └───────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘

── SOFTPHONE WIDGET (persistent mini-mode, bottom-right) ──
┌────────────────────────────┐
│ 📞 UCaaS     [🟢 Available ▾]│
│ Active: Garcia (3:42)      │
│ [Mute] [Hold] [End] [Expand]│
└────────────────────────────┘
```

---

## KEY COMPONENTS

### 1. Live Queues
- **Description:** Real-time view of callers waiting in each IVR-routed queue
- **Data displayed:** Queue name (IVR branch), caller phone number, wait time, caller ID match (existing client/matter lookup)
- **Actions available:** Answer (picks up call on softphone), Transfer (warm transfer with notes), Callback (creates callback task, removes from queue), Send to voicemail
- **Per FSD:** IVR branches are per-office main number; queues for New Client, Existing Client Updates, Billing

### 2. Active Call / Softphone
- **Description:** In-call interface with client context, call controls, and live notes
- **Data displayed:** Caller info (number, matched client, matched matter), call duration, call notes (editable during call)
- **Actions available:** Mute, Hold, Transfer (warm with notes to any directory entry), End call, Take notes
- **Warm transfer:** Opens transfer modal: select recipient from directory, add transfer notes that the recipient sees before accepting

### 3. Post-Call Summary
- **Description:** AI-generated call summary and task suggestions shown immediately after call ends
- **Data displayed:** AI summary of call content, suggested follow-up tasks
- **Actions available:**
  - Edit AI summary
  - "Approve & Save to Matter" (logs summary in matter timeline — per FSD human-in-the-loop for client-facing records)
  - Select and create suggested tasks
- **Per FSD:** Call summary + task suggestion requires human approval

### 4. Firmwide Directory
- **Description:** Searchable directory of all firm personnel with office affiliation and real-time status
- **Data displayed:** Name, office, role, extension, availability status (Available, On call, Away, DND, OOO)
- **Actions available:** Click to call (internal), Filter by office, Search by name/role

### 5. IVR Configuration
- **Description:** Per-office IVR menu and routing configuration
- **Data displayed:** Office main number, IVR menu options with routing destinations, business hours, after-hours behavior, holiday schedule
- **Actions available:** Edit IVR menu, Configure business hours, Set holiday schedule, Test IVR flow
- **Per FSD:** Per-office main number with IVR options only (not per-user DDI)

### 6. Softphone Widget (Persistent Mini-Mode)
- **Description:** Floating widget in bottom-right corner showing active call status; accessible from any screen
- **Data displayed:** Status (available/busy/away), active call info (client name, duration), basic controls
- **Actions available:** Mute, Hold, End, Expand (opens full UCaaS Console)
- **Persistent across all screens** when UCaaS is active

---

## INTERACTIONS & STATES

### Default State
Queue Monitor tab for intake. Directory tab for attorneys/paralegals. Softphone widget visible when logged in. Queues populate in real-time.

### Empty State
- **No callers in queue:** "All queues clear. 0 callers waiting." with green indicator
- **No call history:** "No calls logged yet today."

### Loading State
Queue data streams in real-time (WebSocket). Directory loads with skeleton rows. Softphone widget shows "Connecting..." on initial load.

### Error State
- Telephony service down: "Phone system is offline. [Check status] [Retry connection]" — major red banner
- Call dropped: "Call disconnected unexpectedly. [Redial] [Log incomplete call]"
- AI summary unavailable: "Summary generation unavailable. Please write notes manually."

### Blocked/Gated State
- **Status set to DND/Away:** Queue does not route calls to this agent; widget shows status clearly
- **IVR Config tab:** Only visible to CLO/COO; other users see 3 tabs only

### Success/Confirmation State
- Call answered: queue entry removed, softphone activates with caller info
- Transfer completed: toast "Call transferred to [name]"; call removed from softphone
- Summary saved: toast "Call summary saved to matter [#]. Timeline updated."
- Tasks created: toast "[N] tasks created from call follow-ups"

---

## AUTOMATIONS & AI ASSISTS

### What the system suggests
- **Caller ID matching:** System auto-matches incoming number to client database; displays client name and active matters
- **Call summary generation:** AI produces summary after call ends; based on call audio/notes
- **Task suggestions:** AI suggests follow-up tasks based on call content
- **Smart transfer suggestion:** If caller matched to existing client, system suggests transferring to assigned attorney/paralegal

### What requires human approval
- **Call summaries** must be approved before saving to matter timeline (per FSD: human-in-the-loop for client-facing records)
- **Task creation** from suggestions requires explicit user action
- **All client-facing communications** initiated from this screen need approval

### How suggestions are surfaced visually
- 🤖 AI chip on generated summaries and task suggestions
- Client match shown as inline context card when answering call
- Transfer suggestion shown as highlighted option in transfer modal

---

## ROLE-BASED VISIBILITY

### CLO
- **Full read access** to all queues across offices (supervisor mode)
- Can listen to live calls (silent monitoring) — **[ASSUMPTION]** with appropriate legal compliance notice
- IVR Configuration tab visible and editable
- **Editable:** IVR config, business hours, holiday schedule

### COO
- **Full read access** to all queues across offices
- IVR Configuration tab visible and editable
- Cannot silently monitor calls
- **Editable:** IVR config, business hours, holiday schedule

### Intake Specialist
- **Full access** to Queue Monitor for their office
- Can answer, transfer, take notes, approve summaries
- Directory access (firmwide)
- Cannot access IVR Config
- **Editable:** Call handling, notes, summary approval, task creation

### Attorney
- **Softphone widget** for receiving transferred calls and making outbound calls
- Directory access (firmwide)
- Can view their own call history
- Queue Monitor: read-only (can see queue depth but cannot answer from queue)
- **Editable:** Own call notes, summary approval for their calls

### Paralegal
- **Softphone widget** for calls
- Directory access
- Own call history
- **Editable:** Own call notes, summary approval

### Billing Specialist
- **Softphone widget** for collections calls
- Access to Billing queue (if configured as queue agent)
- Directory access
- **Editable:** Own call notes, summary approval

### Marketing
- **No access** to UCaaS Console (no phone responsibilities)

### Client
- **No access.** Clients interact via their phone; they experience the IVR and are routed to queues.

---

## EDGE CASES & GUARDRAILS

### Capacity exceeded
- If all intake agents are on calls: new callers hear hold music/estimated wait time
- Queue depth alert: if >5 callers waiting in any queue for >2 minutes: alert sent to intake lead and COO

### Payment fails
- Not directly applicable. Billing queue handles payment-related calls.

### Required documents missing
- Not directly applicable. If a client calls about a matter, the matched matter context shows risk indicators in the call context card.

### Deadlines at risk
- Not directly applicable. If client call relates to a matter with deadline risk, call context card shows "⚠ Active deadline risk" for the matched matter.

### Call recording compliance
- **[ASSUMPTION]** Call recording governed by jurisdiction-specific consent laws
- System displays recording indicator to agent; agent responsible for compliance notification
- Recording toggle available per call (if permitted by office configuration)

### Network/telephony failures
- Softphone widget shows "⚠ Connection lost" if WebRTC/telephony connection drops
- Automatic reconnection attempt (3 retries, 5-second intervals)
- If unrecoverable: "Phone system offline. Incoming calls will be handled by fallback routing." — **[ASSUMPTION]** fallback routes to office's physical phone system or voicemail

### Simultaneous calls
- Intake agents handle one call at a time; second incoming goes to next available agent
- Attorneys can receive one transferred call while already on a call — second call goes to hold with notification
- **[ASSUMPTION]** No conference calling in v1
