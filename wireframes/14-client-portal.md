# Screen 14: Client Portal — Documents, Messages & Payments

## Purpose
Provide clients with self-service access to document upload/download, secure messaging with the firm (with SLA-based response expectations), invoice viewing, and payment processing. Complements the Status Tracker (Screen 13) as the transactional side of the client portal.

## Primary Users
Client (portal user) — primary and sole user

## When This Screen Is Used (Workflow Stage)
Throughout engagement. Clients access for document exchange, communication, and payments at any stage from Engaged through Closed.

---

## LAYOUT (TEXTUAL WIREFRAME)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ CLIENT PORTAL TOP BAR                                                   │
│ [Firm Logo]                    [Maria Garcia ▾]  [📩 Messages (1)]     │
├─────────────────────────────────────────────────────────────────────────┤
│ CLIENT PORTAL NAV                                                       │
│ [My Matters] [Documents ●] [Messages] [Payments] [Resources]          │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│ ═══ DOCUMENTS TAB ═══                                                   │
│                                                                         │
│ Matter: [Estate Plan #EP-0147 ▾]                                       │
│                                                                         │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ 📤 DOCUMENTS WE NEED FROM YOU                                     │  │
│ │                                                                   │  │
│ │ Document             │ Status    │ Action                        │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ Deed copies          │ ⚠ Needed  │ [Upload]                      │  │
│ │ Beneficiary forms    │ ⚠ Needed  │ [Upload]                      │  │
│ │ Photo ID             │ ✅ Received│ Uploaded Mar 24               │  │
│ │ Asset inventory      │ ✅ Received│ Uploaded Mar 24               │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ 📥 DOCUMENTS FROM YOUR ATTORNEY                                   │  │
│ │                                                                   │  │
│ │ Document             │ Date      │ Action                        │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ Engagement letter    │ Mar 24    │ [View] [Download] [Sign ✍]    │  │
│ │ (No documents shared yet for review phase)                       │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ UPLOAD MODAL (on [Upload] click)                                        │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Upload: Deed Copies                                               │  │
│ │                                                                   │  │
│ │ ┌─────────────────────────────────────────────────────────────┐  │  │
│ │ │            Drag files here or [Browse files]                │  │  │
│ │ │            PDF, JPG, PNG up to 25MB per file               │  │  │
│ │ └─────────────────────────────────────────────────────────────┘  │  │
│ │                                                                   │  │
│ │ Selected: deed_property1.pdf (2.1 MB)  [Remove]                  │  │
│ │                                                                   │  │
│ │ [Upload] [Cancel]                                                 │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ═══ MESSAGES TAB ═══                                                    │
│                                                                         │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Secure Messages — Estate Plan #EP-0147                            │  │
│ │                                                                   │  │
│ │ [+ New Message]                                                   │  │
│ │                                                                   │  │
│ │ ┌─────────────────────────────────────────────────────────────┐  │  │
│ │ │ 📩 From: Your Team (S. Lee)                    Mar 25, 10AM│  │  │
│ │ │                                                             │  │  │
│ │ │ Hi Maria, we're making good progress on your estate plan.  │  │  │
│ │ │ Could you please upload the deed copies for your            │  │  │
│ │ │ properties? This will help us finalize the trust document. │  │  │
│ │ │                                                             │  │  │
│ │ │ [Reply]                                                     │  │  │
│ │ └─────────────────────────────────────────────────────────────┘  │  │
│ │                                                                   │  │
│ │ ┌─────────────────────────────────────────────────────────────┐  │  │
│ │ │ 📤 From: You                                   Mar 25, 11AM│  │  │
│ │ │                                                             │  │  │
│ │ │ Hi, I'll get those to you by Friday. Quick question —      │  │  │
│ │ │ do you need all properties or just the primary residence?  │  │  │
│ │ │                                                             │  │  │
│ │ │ Expected response: within 1 business day                   │  │  │
│ │ └─────────────────────────────────────────────────────────────┘  │  │
│ │                                                                   │  │
│ │ ┌─────────────────────────────────────────────────────────────┐  │  │
│ │ │ New message:                                                │  │  │
│ │ │ [__________________________________________________]       │  │  │
│ │ │ [Attach file] [Send]                                       │  │  │
│ │ └─────────────────────────────────────────────────────────────┘  │  │
│ │                                                                   │  │
│ │ ℹ Messages are typically responded to within 1 business day.     │  │
│ │   For urgent matters, please call (555) 100-1000.                │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ═══ PAYMENTS TAB ═══                                                    │
│                                                                         │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Account Summary                                                   │  │
│ │                                                                   │  │
│ │ Outstanding balance: $0.00  ✅                                    │  │
│ │ Retainer on file: $2,000.00                                      │  │
│ │ Total paid to date: $2,250.00                                    │  │
│ │                                                                   │  │
│ │ ── Invoices ──                                                    │  │
│ │                                                                   │  │
│ │ Invoice   │ Date    │ Amount  │ Status   │ Action                │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ INV-0892 │ Mar 28  │ $1,850  │ ⏳ Draft  │ (Not yet sent)       │  │
│ │ Consult  │ Mar 24  │ $250    │ ✅ Paid   │ [View receipt]        │  │
│ │                                                                   │  │
│ │ ── Payment Methods ──                                             │  │
│ │                                                                   │  │
│ │ 💳 Visa ending in 4521  [Default]  [Remove]                      │  │
│ │ [+ Add payment method]                                           │  │
│ │                                                                   │  │
│ │ ── Payment History ──                                             │  │
│ │                                                                   │  │
│ │ Date    │ Description           │ Amount  │ Method              │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ Mar 24  │ Consultation fee      │ $250    │ Visa *4521          │  │
│ │ Mar 24  │ Retainer deposit      │ $2,000  │ ACH                 │  │
│ │                                                                   │  │
│ │ [Download statement]                                              │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ MAKE A PAYMENT (shown when balance due)                           │  │
│ │                                                                   │  │
│ │ Invoice: INV-XXXX — $X,XXX.XX                                   │  │
│ │ Pay with: [Visa *4521 ▾]                                        │  │
│ │ Amount: [$X,XXX.XX    ]  (Full amount / Custom amount)          │  │
│ │                                                                   │  │
│ │ [Pay Now]                                                        │  │
│ │                                                                   │  │
│ │ 🔒 Payments are processed securely.                              │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ═══ RESOURCES TAB ═══                                                   │
│                                                                         │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Educational Materials                                             │  │
│ │                                                                   │  │
│ │ Related to your matters:                                         │  │
│ │ 📄 What to Expect During Estate Planning           [Read →]      │  │
│ │ 📄 Understanding Your Trust Document               [Read →]      │  │
│ │ 📄 FAQ: Powers of Attorney                         [Read →]      │  │
│ │ 📄 Guide to Estate Planning with Minor Children    [Read →]      │  │
│ │                                                                   │  │
│ │ General:                                                         │  │
│ │ 📄 How to Work with Your Legal Team                [Read →]      │  │
│ │ 📄 Secure Document Sharing Best Practices          [Read →]      │  │
│ │                                                                   │  │
│ │ 📞 Need help? Call (555) 100-1000 or [Send a message]           │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ── SCHEDULING LINK (persistent footer or Resources) ──                 │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ 📅 Need to schedule a call or meeting?                            │  │
│ │ [Schedule with your attorney →]  [Schedule with your team →]     │  │
│ └───────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## KEY COMPONENTS

### 1. Documents — Needed From Client
- **Data:** Checklist of required documents with upload status
- **Actions:** Upload files (drag-and-drop or browse), View uploaded file
- **Accepted formats:** PDF, JPG, PNG; max 25MB per file — **[ASSUMPTION]**

### 2. Documents — From Attorney
- **Data:** Documents shared by the firm for client review/signature
- **Actions:** View (in-browser preview), Download, Sign (e-signature integration)
- **[ASSUMPTION]** E-signature embedded or via redirect to signing service

### 3. Secure Messaging
- **Data:** Threaded conversation per matter; sender name, timestamp, message text
- **Actions:** New message, Reply, Attach file
- **SLA indicator:** "Expected response: within [X] business day(s)" per FSD requirement
- **Per FSD:** Q&A messaging with SLAs
- **All messages are client-facing and go through internal review before delivery** (staff-side approval)

### 4. Payments & Invoices
- **Data:** Outstanding balance, retainer on file, invoice list, payment methods, payment history
- **Actions:** Pay invoice, Add payment method, View receipt, Download statement
- **Payment methods:** Card, ACH — **[ASSUMPTION]** PCI-compliant embedded payment form
- **"Make a Payment" section only appears when there is a balance due**

### 5. Educational Resources
- **Per FSD:** Educational materials, scheduling links
- **Data:** Practice-area-relevant articles and guides; general portal help content
- **Actions:** Read articles, Contact firm

### 6. Scheduling Links
- **Per FSD:** Scheduling links accessible from portal
- **Actions:** Opens scheduling interface for available meeting slots with attorney or team

---

## INTERACTIONS & STATES

### Default State
Documents tab showing current matter's document checklist. Unread message badge on Messages tab.

### Empty State
- **No documents requested yet:** "No documents are needed from you at this time."
- **No messages:** "No messages yet. [Send a message to your team →]"
- **No invoices:** "No invoices have been issued yet."

### Loading State
Simple spinner with firm branding. Content loads within 2 seconds.

### Error State
- Upload failure: "Unable to upload file. Please check file size (<25MB) and format (PDF, JPG, PNG). [Try again]"
- Payment failure: "Payment could not be processed. Please check your payment method and try again. [Try different method] [Contact us]"
- Message send failure: "Message could not be sent. [Try again]"

### Blocked/Gated State
- **Past-due payment:** Banner on Payments tab: "You have a past-due balance. [Pay now →]" — does NOT block access to other portal features
- **Portal access revoked:** "Your portal access has been updated. Please contact the office at (555) 100-1000."

### Success State
- Document uploaded: "✅ Document received! Your team will be notified."
- Payment processed: "✅ Payment of $[amount] received. [View receipt]"
- Message sent: "✅ Message sent. Expected response: within 1 business day."

---

## AUTOMATIONS & AI ASSISTS

### Automated
- Document request notifications (email/SMS) when new document is needed
- Invoice notifications when new invoice is posted
- Payment receipt confirmation (email)
- Reminder messages for outstanding document requests (per configured schedule)

### Human-in-the-loop
- All messages from the firm are reviewed/approved by staff before delivery to client (per FSD)
- Document review occurs internally after upload (client sees "Received" status)
- **No AI-generated content is shown directly to clients** without staff approval

---

## ROLE-BASED VISIBILITY

### Client
- **Full access** to their own documents, messages, invoices, payments, resources
- Can see only their own matter(s)
- Cannot see: internal status, other clients, staff details beyond name, billing details beyond invoices
- **Editable:** Upload documents, send messages, make payments, manage payment methods

### All Internal Roles
- **Do not access this screen directly**
- Documents uploaded by clients appear in Matter Workspace (Screen 7) documents panel
- Messages from clients appear in Matter Workspace communications and as notifications
- Payments appear in Billing Workspace (Screen 9) and Matter Workspace billing tab

---

## EDGE CASES & GUARDRAILS

### Payment fails
- Clear error message with guidance; suggest alternative payment method
- Failed payment logged internally; billing specialist notified
- After 3 failures: "Please contact our office to complete your payment. [Call (555) 100-1000]"

### Required documents missing
- Persistent "needed" status in documents list
- Client receives reminder via email/SMS per configured schedule
- Portal "Action Needed" section on Status Tracker (Screen 13) cross-references

### Large file upload
- Progress bar during upload
- If file exceeds 25MB: immediate validation error before upload begins
- **[ASSUMPTION]** Max 5 files per upload batch

### Client with multiple matters
- Matter selector dropdown on Documents and Messages tabs
- Each matter has separate message threads and document lists
- Payments may be cross-matter (view shows which matter each invoice relates to)

### Portal session security
- **[ASSUMPTION]** Multi-factor authentication required for portal access
- Session timeout: 30 minutes inactivity
- Sensitive actions (payment, e-signature) may require re-authentication

### Message SLA tracking
- Client sees "Expected response: within [X] business day(s)"
- If SLA is exceeded: client does NOT see an alert (to avoid anxiety), but internal team receives SLA breach notification
- **[ASSUMPTION]** Default SLA: 1 business day for general messages, 4 hours for urgent (client cannot set urgency; internal routing determines)
