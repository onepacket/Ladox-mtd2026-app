# MTD 2026 — One‑Page Compliance Spec & Pain‑Point Journey Map (PWA‑first)

> **Purpose:** Single-page developer/designer brief you can hand to engineers and product owners + a user journey map showing where your PWA solves pain points at each stage.

---

## Part A — One‑Page MTD (Making Tax Digital for Income Tax) 2026 Compliance Spec

### 1) Quick summary (what this doc mandates)

* Your product must **create, store, correct and submit digital records** for income and expenses. Users in scope must send **quarterly updates** and a **final period declaration** using MTD‑compatible software connected to HMRC APIs.
* Records must maintain **digital links** (no manual copy/paste between systems), an **immutable audit trail**, and retain records for the statutory period.

### 2) Who/When (scope)

* Sole traders and landlords who meet HMRC thresholds and other rules from **6 April 2026** (check latest HMRC guidance for thresholds/exemptions). Build for the general case: a record store for monthly/quarterly reporting and final declaration flows.

### 3) Mandatory features (developer checklist)

* **Digital record model**: structured records for income & expense lines (timestamped, source doc linked).
* **Quarterly submission flow**: create, validate and send periodic updates (periodic/income/expense totals per obligation period).
* **Final declaration**: end‑of‑period summary and declaration endpoint.
* **Digital links**: importers/bridging must preserve automated links (no manual copy/paste).
* **HMRC API integration**: OAuth2 flows (Authorization Code + Client Credentials), Obligations, Income & Expenses, Agent Authorisation.
* **Agent flows**: agent/client authorisation and role management.
* **Audit trail & versioning**: immutable logs with user, timestamp, change reason, version hashes.
* **Retention/export**: export records (PDF/CSV/JSON) and retain for at least the statutory period.
* **Security**: TLS, encryption at rest, RBAC, 2FA, GDPR compliance.

### 4) HMRC endpoints & flows (implementation notes)

* **Developer registration**: register app on HMRC Developer Hub; create sandbox credentials.
* **OAuth**

  * Authorization Code flow for user‑authorised actions (users sign in to HMRC, grant access).
  * Client Credentials for server‑to‑server app actions where allowed.
* **Key APIs to integrate (examples)**

  * Obligations API — list filing obligations and due dates.
  * Income & Expenses (MTD ITSA) endpoints — create/update period data.
  * Agent Authorisation API — check/request agent permissions.
  * Submission & Status endpoints — post periodic updates/final declarations and poll for status.

> *Note:* Use HMRC sandbox for all E2E tests before going live.

### 5) Data model (JSON snippets)

#### A. Minimal `expense` record (canonical digital record)

```json
{
  "expenseId": "exp_2026_0001",
  "userId": "user_123",
  "date": "2025-11-15",
  "amount": 45.60,
  "currency": "GBP",
  "supplier": "Stationery Ltd",
  "category": "Office Costs",
  "vatAmount": 9.12,
  "sourceDocumentUrl": "https://storage.example.com/docs/exp_2026_0001.jpg",
  "createdAt": "2025-11-15T09:24:00Z",
  "createdBy": "mobile_upload",
  "version": 1,
  "notes": "Receipt for printer paper",
  "immutableHash": "sha256:..."
}
```

#### B. Quarterly update payload (simplified)

```json
{
  "periodId": "2026-Q4",
  "from": "2025-10-01",
  "to": "2025-12-31",
  "totalIncome": 12500.00,
  "totalExpenses": 3200.45,
  "categories": {
    "officeCosts": 1200.00,
    "travel": 800.45,
    "equipment": 1200.00
  },
  "attachedRecords": ["exp_2026_0001","exp_2026_0002"],
  "submittedAt": "2026-01-15T12:10:00Z",
  "softwareSupplierId": "your-software-id"
}
```

#### C. Final declaration (end of period)

```json
{
  "taxYear": "2025-2026",
  "userId": "user_123",
  "periodSummaries": ["2026-Q1","2026-Q2","2026-Q3","2026-Q4"],
  "finalTotals": {"income": 48000.00, "expenses": 15200.00},
  "declaration": {
    "signedAt": "2026-04-20T10:23:00Z",
    "signatory": "user_123",
    "declarationText": "I declare this to be correct to the best of my knowledge"
  }
}
```

### 6) Acceptance tests (HMRC sandbox scenarios — automated checks)

* **Auth flow test**: perform OAuth Authorization Code flow; verify token exchange and refresh tokens work.
* **Obligations test**: request obligations for user; verify correct periods returned and map to UI calendars.
* **Quarterly submission test**: POST a quarterly update payload -> expect `202 Accepted`/`200 OK` with submission ID. Poll status until `processed`.
* **Final declaration test**: Submit final declaration after all quarterly updates; verify acceptance and that totals reconcile to the sum of period data.
* **Digital links test**: Import a spreadsheet with formula-derived totals; export a digital‑link manifest proving connection between spreadsheet rows and submitted payload (no manual copy/paste in flow).
* **Agent flow test**: Agent requests client authorisation and successfully submits on behalf of client (where permitted).

### 7) MVP acceptance criteria

* User can register/login and upload receipts (photo/PDF). OCR extracts basic fields and creates canonical expense records.
* User can import a bank CSV and reconcile within the app.
* User can create a quarterly update and export an HMRC‑compatible payload (sandbox-tested) and view submission status.
* Audit trail exists for edits; records are exportable.

### 8) Security & retention

* TLS everywhere, AES‑256 or equivalent at rest for attachments, HSM or KMS for key management.
* Role based access: owner, accountant/agent, read‑only viewer.
* Data retention policy: keep canonical records for minimum statutory period (implement exports & deletion controls to comply with GDPR subject access requests).

### 9) Operational notes

* Build server-side queuing for submissions to HMRC to handle retry/backoff.
* Implement monitoring and alerts for submission failures and API changes.
* Document a rollback and support playbook for users when submissions fail.

---

## Part B — Visual Pain‑Point Journey Map (PWA‑first)

> The map below is laid out as a timeline from `Year‑Round` → `2–3 months pre‑tax-end` → `1–2 months pre` → `2–4 weeks pre` → `Filing` → `Post‑Filing`. For each phase: user goals, pain points, your PWA interventions, and product KPIs.

### Timeline (visual layout in text)

```
[Year-Round] --- [2-3 months pre] --- [1-2 months pre] --- [2-4 weeks pre] --- [Filing (Jan31/Apr5)] --- [Post-Filing]
```

### Phase: Year‑Round

* **User goals:** Keep tidy records with minimal admin time.
* **Pain points:** Receipts scattered, forget to log, spreadsheets out of date.
* **PWA interventions:**

  * Mobile camera receipt upload (browser camera API) -> immediate OCR extraction.
  * Lightweight dashboard (home screen PWA) showing running totals.
  * Offline capture queue + background sync.
* **KPIs:** % receipts captured within 48h of purchase; monthly active users.

### Phase: 2–3 months pre (awareness)

* **User goals:** Understand what’s missing and start gathering.
* **Pain points:** Panic, missing receipts, unsure categories.
* **PWA interventions:**

  * Push/web notification (or email/SMS) reminders.
  * "Missing receipts" detector via bank-feed reconciliation suggestions.
  * Simple explainers on allowable expenses and common traps.
* **KPIs:** Reminder open rate; reduction in missing-receipt tickets.

### Phase: 1–2 months pre (gathering)

* **User goals:** Convert paper receipts to digital and categorise.
* **Pain points:** Bulk scanning time; manual spreadsheet summation; duplicates.
* **PWA interventions:**

  * Bulk photo upload and batch OCR processing on server.
  * Auto-categorisation with manual override.
  * Bank CSV import and reconcile UI with suggested matches.
* **KPIs:** Time to assemble period bundle; % auto-categorised accuracy.

### Phase: 2–4 weeks pre (compilation)

* **User goals:** Produce final totals and verify accuracy.
* **Pain points:** Fear of mistakes, VAT confusion, last‑minute accountant costs.
* **PWA interventions:**

  * Pre‑submission validation rules (duplicate detection, VAT checks, personal-use flags).
  * One‑click export to HMRC payload (sandbox-tested) or download for accountant.
  * Option to invite an accountant (agent flow) with scoped access.
* **KPIs:** Pre-submission error rate; time from validation -> submission-ready.

### Phase: Filing (deadlines)

* **User goals:** Submit quarterly updates or final declaration on time.
* **Pain points:** Status uncertainty, submission failures.
* **PWA interventions:**

  * Submit via HMRC API (OAuth) or provide HMRC‑compatible export + step‑by‑step guidance.
  * Submission status dashboard with retry logic & support flow.
* **KPIs:** Successful submissions; average time to resolve failed submissions.

### Phase: Post‑Filing

* **User goals:** Understand tax position and prepare for next year.
* **Pain points:** Regret about poor bookkeeping; worry about audits.
* **PWA interventions:**

  * Yearly summary report, exportable archive.
  * Tips for next year & pro‑rata tax forecasting.
  * Continuous nudges to capture receipts.
* **KPIs:** Renewed subscriptions; decreased time to next-year readiness.

---

## PWA‑First Rationale (annotated box — include in investor slides)

* **Lower initial cost & faster time-to-market** vs building native apps for iOS/Android.
* **Installable, app‑like UX** (home‑screen, full‑screen, offline capture) gives 90% of required functionality for early adopters.
* **Easier to iterate** — single codebase for web & mobile browsers; later wrap in native shell (Capacitor) if store presence is needed.
* **Caveats:** test camera/file APIs across popular browsers (Chrome, Safari iOS), implement cloud storage for durable doc retention, and build robust offline sync and queued submission handling to meet MTD requirements.

---

## Deliverables you can hand to engineering/design teams

1. This one‑page spec (hand to devs). ✅
2. JSON payload examples above (use in API contract mocks). ✅
3. Acceptance test list (automated tests against HMRC sandbox). ✅
4. Journey map to guide UX flows and PWA features (use in product/UX sprints). ✅

---

If you’d like, next I can:

* Convert the journey map into a single **PNG or PDF visual** for pitches (I can produce a simple layout you can drop into slides), or
* Produce a **detailed API contract** (OpenAPI/Swagger YAML) derived from the JSON snippets and HMRC sandbox endpoints so your engineers can start integration tasks immediately.

Which of those do you want next?

