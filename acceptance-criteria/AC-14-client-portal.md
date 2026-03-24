# AC-14: Client Portal — Documents, Messages & Payments

## Feature Overview
Self-service client access to document upload/download, secure messaging with SLA-based response expectations, invoice viewing, and payment processing.

---

## AC-14.1 — Access Control

**Given** an authenticated client accesses the portal
**Then** they can see only their own documents, messages, invoices, and payments

**Given** internal staff need to access client-uploaded documents
**Then** they do so via the Matter Workspace (Screen 7), not this screen

---

## AC-14.2 — Documents Tab — Documents Needed from Client

**Given** the Documents tab is active for a matter
**Then** a checklist shows each required document with: document name, status (⚠ Needed / ✅ Received), and an upload action button

**Given** a client clicks [Upload]
**Then** an upload modal opens with a drag-and-drop area and "Browse files" option

**Given** the upload modal shows file constraints
**Then** it displays: accepted formats (PDF, JPG, PNG), max file size (25MB per file), and max batch size (5 files per upload)

**Given** a file exceeds 25MB
**Then** an immediate validation error displays before upload begins: "File too large. Maximum file size is 25MB."

**Given** a file is successfully uploaded
**Then** the success message displays: "✅ Document received! Your team will be notified." and the document status changes to ✅ Received

**Given** a document upload fails
**Then** the error displays: "Unable to upload file. Please check file size (<25MB) and format (PDF, JPG, PNG). [Try again]"

**Given** a progress bar is shown during upload
**Then** it updates in real-time until completion

---

## AC-14.3 — Documents Tab — Documents from Attorney

**Given** the firm shares documents with the client
**Then** they appear in the "Documents from Your Attorney" section with: document name, date, and available actions

**Given** available actions for received documents
**Then** they include: [View], [Download], and [Sign ✍] (if signature is required)

**Given** a client views a document
**Then** it opens in an in-browser preview

**Given** e-signature is available
**Then** it is embedded or via redirect to a signing service

---

## AC-14.4 — Documents Tab — Multiple Matters

**Given** a client has multiple matters
**Then** a matter selector dropdown is shown and each matter has separate document lists

---

## AC-14.5 — Messages Tab

**Given** the Messages tab is active
**Then** a threaded conversation is shown per matter with: sender name, timestamp, and message text

**Given** unread messages exist
**Then** a badge count is shown on the Messages tab in the navigation

**Given** a client sends a new message
**Then** the success message displays: "✅ Message sent. Expected response: within 1 business day."

**Given** a message is sent
**Then** the SLA response expectation is shown: "Expected response: within [X] business day(s)"

**Given** the SLA is exceeded on the client's message
**Then** the client does NOT see an alert — the internal team receives the SLA breach notification

**Given** a client can attach a file to a message
**Then** the [Attach file] button is available in the message composer

**Given** a message send fails
**Then** the error displays: "Message could not be sent. [Try again]"

**Given** all messages from the firm are sent to the client
**Then** they have been reviewed and approved by internal staff before delivery (per FSD)

**Given** the urgent contact guidance is shown
**Then** it displays: "For urgent matters, please call [office number]."

---

## AC-14.6 — Payments Tab — Account Summary

**Given** the Payments tab is active
**Then** the account summary shows: outstanding balance, retainer on file, and total paid to date

---

## AC-14.7 — Payments Tab — Invoices List

**Given** the invoices list is shown
**Then** each invoice displays: invoice number/description, date, amount, status, and action

**Given** an invoice is not yet sent (draft)
**Then** it shows "(Not yet sent)" and no payment action is available

**Given** an invoice is ready for payment
**Then** the [Pay now] action is shown

---

## AC-14.8 — Payments Tab — Making a Payment

**Given** there is an outstanding balance
**Then** the "Make a Payment" section is displayed

**Given** the "Make a Payment" section is shown
**Then** it shows: invoice reference, amount due, payment method selector, amount field (full/custom), and [Pay Now] button

**Given** the "Make a Payment" section is not needed (no balance due)
**Then** the section is NOT displayed

**Given** a payment is processed successfully
**Then** the success message displays: "✅ Payment of $[amount] received. [View receipt]"

**Given** a payment fails
**Then** the error displays: "Payment could not be processed. Please check your payment method and try again. [Try different method] [Contact us]"

**Given** payment fails 3 times
**Then** the message displays: "Please contact our office to complete your payment. [Call [office number]]"

**Given** failed payments are recorded
**Then** the billing specialist is notified internally

---

## AC-14.9 — Payments Tab — Payment Methods

**Given** the Payment Methods section is shown
**Then** the client can: view saved payment methods, set a default, remove a method, and add a new method

**Given** accepted payment methods
**Then** they include: credit/debit card and ACH (PCI-compliant embedded form)

---

## AC-14.10 — Payments Tab — Payment History

**Given** the payment history section is shown
**Then** it lists past payments with: date, description, amount, and payment method used

**Given** a client clicks "Download statement"
**Then** a payment statement is downloaded

---

## AC-14.11 — Resources Tab

**Given** the Resources tab is active
**Then** it shows practice-area-relevant educational articles and general portal help content

**Given** the scheduling link is displayed
**Then** it shows options to schedule with attorney or team and opens available meeting slots

---

## AC-14.12 — Automated Notifications

**Given** a new document is requested from the client
**Then** the client receives an email and SMS notification automatically

**Given** a new invoice is posted
**Then** the client receives a notification automatically

**Given** a payment is completed
**Then** a payment receipt confirmation email is sent automatically

**Given** an outstanding document request exists past the configured reminder schedule
**Then** a reminder email and SMS are sent automatically

---

## AC-14.13 — Human-in-the-Loop on Client Communications

**Given** any message is sent from the firm to the client via the portal
**Then** it has been reviewed and approved by internal staff before delivery

**Given** the client uploads a document
**Then** it shows "Received" status on the client side while the internal review occurs separately

**Given** AI-generated content is being considered for display to the client
**Then** it requires staff approval first — no AI content is shown directly to clients without approval

---

## AC-14.14 — Portal Access & Security

**Given** the client portal requires login
**Then** multi-factor authentication is required

**Given** 30 minutes of inactivity pass
**Then** the session times out

**Given** sensitive actions (payment, e-signature) are taken
**Then** re-authentication may be required

**Given** portal access is revoked
**Then** the message displays: "Your portal access has been updated. Please contact the office at [phone number]."

---

## AC-14.15 — Loading State

**Given** the portal content is loading
**Then** a simple spinner with firm branding is shown; content loads within 2 seconds

---

## AC-14.16 — Empty States

**Given** no documents are requested
**Then** the message displays: "No documents are needed from you at this time."

**Given** no messages exist
**Then** the message displays: "No messages yet. [Send a message to your team →]"

**Given** no invoices have been issued
**Then** the message displays: "No invoices have been issued yet."

---

## AC-14.17 — Persistent Footer

**Given** the client is on any portal tab
**Then** a scheduling link footer/section is accessible: [Schedule with your attorney →] [Schedule with your team →]
