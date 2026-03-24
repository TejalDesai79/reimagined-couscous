# AC-08: Embedded UCaaS Console

## Feature Overview
Unified in-app communications: phone calls via per-office IVR, firmwide directory, live queue monitoring, call handling, and communication timeline integration.

---

## AC-08.1 — Access Control

| Role | Access |
|------|--------|
| CLO | Full supervisor access across all offices; IVR Config tab visible and editable; can silent-monitor calls |
| COO | Full read access across all offices; IVR Config tab visible and editable; cannot silent-monitor |
| Intake Specialist | Full Queue Monitor access for their office; directory; cannot access IVR Config |
| Attorney | Softphone widget for outbound/transferred calls; directory; own call history; Queue Monitor read-only |
| Paralegal | Softphone widget; directory; own call history |
| Billing Specialist | Softphone widget; access to Billing queue if configured; directory; own call history |
| Marketing | No access |
| Client | No access — clients interact via their phone; they experience the IVR |

**Given** a user navigates to the IVR Config tab
**Then** it is only visible to CLO and COO; other users see only 3 tabs

---

## AC-08.2 — Queue Monitor (Default for Intake)

**Given** the UCaaS Console loads for an Intake Specialist
**Then** the Queue Monitor tab is the default view

**Given** the Queue Monitor loads
**Then** it shows per IVR branch:
- Branch name (New Client Intake, Existing Client, Billing)
- Each caller: phone number, wait time
- Caller ID match: linked client name and matter (if match found)

**Given** caller ID is matched to an existing client
**Then** the client name and active matter(s) are shown as a context card

**Given** the data is real-time
**Then** queue entries update via WebSocket without page refresh

---

## AC-08.3 — Queue Actions

**Given** a caller is in the queue
**Then** an intake specialist can: [Answer], [Transfer], [Callback], [Send to voicemail]

**Given** a user clicks [Answer]
**Then** the call connects on the softphone and the caller is removed from the queue

**Given** a user clicks [Callback]
**Then** a callback task is created and the caller is removed from the queue

**Given** a user's status is DND or Away
**Then** the queue does not route calls to that agent; the softphone widget clearly shows the status

---

## AC-08.4 — Active Call / Softphone

**Given** a call is active
**Then** the softphone interface shows: caller info (number, matched client, matched matter), call duration, and live notes field

**Given** available call controls are displayed
**Then** they include: [🔇 Mute], [⏸ Hold], [➡ Transfer], and [🔚 End Call]

**Given** a user initiates a warm transfer
**Then** a transfer modal opens showing the firmwide directory; the user selects a recipient and adds transfer notes; the recipient sees the notes before accepting

**Given** a user takes notes during the call
**Then** notes are saved and associated with the call record

---

## AC-08.5 — Post-Call Summary (AI-Generated)

**Given** a call ends
**Then** an AI-generated call summary is shown immediately in the post-call panel with a 🤖 chip

**Given** the AI-generated summary is displayed
**Then** the available actions are: [Edit] and [Approve & Save to Matter]

**Given** the user clicks "Approve & Save to Matter"
**Then** the summary is saved to the matched matter's timeline (requires explicit human approval — per FSD)

**Given** the user does not approve the summary
**Then** it is NOT saved to the official matter record

**Given** AI task suggestions are shown post-call
**Then** suggested tasks are listed with checkboxes; the user must click [Create selected tasks] explicitly

**Given** the AI summary service is unavailable
**Then** the message displays: "Summary generation unavailable. Please write notes manually."

---

## AC-08.6 — Firmwide Directory

**Given** a user views the Directory tab
**Then** it shows all firm personnel with: name, office, role, extension, and real-time availability status

**Given** availability statuses are displayed
**Then** they are: 🟢 Available, 📞 On call, 🟡 Away, 🔴 DND, OOO

**Given** a user clicks a name in the directory
**Then** an internal call is initiated to that person

**Given** the user applies a filter by office
**Then** only personnel from that office are shown

---

## AC-08.7 — IVR Configuration (CLO/COO Only)

**Given** a CLO or COO opens the IVR Config tab
**Then** they see per-office: main phone number, IVR menu options with routing destinations, business hours, after-hours behavior, and holiday schedule

**Given** the IVR Config is edited and saved
**Then** the configuration updates for the selected office

**Given** the user clicks "Test IVR flow"
**Then** a test call simulation is initiated to verify routing

**Given** after-hours behavior is configured
**Then** calls outside business hours route to voicemail → auto-email to the configured inbox

---

## AC-08.8 — Softphone Widget (Persistent Mini-Mode)

**Given** UCaaS is logged in
**Then** a floating softphone widget is visible in the bottom-right corner of all screens

**Given** a call is active
**Then** the widget shows: client name and call duration with controls: [Mute] [Hold] [End] [Expand]

**Given** a user clicks [Expand]
**Then** the full UCaaS Console opens

**Given** the widget is visible on all staff screens
**Then** it does not obscure critical UI elements

---

## AC-08.9 — Agent Status

**Given** an intake supervisor views the Queue Monitor
**Then** an Agent Status section shows each agent's name and current status: 🟢 Available, 📞 On call, 🟡 Away

---

## AC-08.10 — Error States

**Given** the telephony service is unavailable
**Then** a prominent red banner displays: "Phone system is offline. [Check status] [Retry connection]"

**Given** a call drops unexpectedly
**Then** the interface shows: "Call disconnected unexpectedly. [Redial] [Log incomplete call]"

**Given** the WebRTC connection to the softphone drops
**Then** the widget shows "⚠ Connection lost" and attempts automatic reconnection (3 retries, 5-second intervals)

**Given** reconnection is unrecoverable
**Then** the message displays: "Phone system offline. Incoming calls will be handled by fallback routing."

---

## AC-08.11 — Queue Overflow Alert

**Given** more than 5 callers are waiting in any queue for > 2 minutes
**Then** an alert is sent to the intake lead and COO

---

## AC-08.12 — Call Recording Compliance

**Given** call recording is enabled
**Then** a recording indicator is visible to the agent during the call

**Given** call recording is configured per office
**Then** the recording toggle is available per call (if permitted)

---

## AC-08.13 — Simultaneous Calls

**Given** an intake agent is on a call
**Then** a second incoming call routes to the next available agent (not the same agent)

**Given** an attorney is on a transferred call
**Then** a second call placed to them goes to hold with a notification

**Given** conference calling behavior
**Then** conference calling is NOT available in v1

---

## AC-08.14 — Loading State

**Given** the UCaaS Console loads
**Then** the directory loads with skeleton rows; the softphone widget shows "Connecting..." on initial load; queue data streams in real-time via WebSocket

---

## AC-08.15 — Empty States

**Given** all queues are empty
**Then** the Queue Monitor shows: "All queues clear. 0 callers waiting." with a green indicator

**Given** no call history exists for the day
**Then** the Call History tab shows: "No calls logged yet today."
