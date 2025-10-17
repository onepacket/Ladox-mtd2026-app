# 🧾 Ladox MTD 2026 App  
**Simplifying Making Tax Digital for Self-Employed & Small Businesses**

> **Ladox** helps UK freelancers, contractors, and small business owners automate their Self Assessment submissions and stay compliant with HMRC’s upcoming **Making Tax Digital (MTD) for Income Tax Self Assessment (ITSA)** requirements due in **April 2026**.

---

## 🚀 Project Overview

**Ladox MTD** provides a unified web-based platform to:
- Capture and categorize business expenses  
- Generate quarterly summaries automatically  
- Submit tax data directly to **HMRC (sandbox / production)** via OAuth 2.0  
- Stay compliant with MTD ITSA through secure record-keeping and API integration  

### 🎯 Mission
> “To make tax compliance effortless for the self-employed.”

---

## 🧩 Core Features

| Feature | Description |
|----------|--------------|
| **Expense Tracking** | Capture receipts and expenses through PWA (mobile-friendly) or CSV uploads. |
| **Auto Categorization** | AI-assisted tagging using HMRC expense categories. |
| **Quarterly Submissions** | Generate and send structured submissions to HMRC API endpoints. |
| **Final Declaration** | Automatically calculate income, expenses, and profit for end-of-year submission. |
| **Audit Logging** | Every declaration and status update stored securely under `/logs/audit.log`. |
| **Metrics Dashboard** | Auto-refreshing endpoint (`/metrics?format=html`) for instant submission stats. |
| **Postman Collection** | Mock API hosted via `https://mock.mtd2026.postman.co` for developers. |

---

## ⚙️ Architecture Overview

```plaintext
┌──────────────────────────┐
│        Ladox PWA         │
│ (React + Tailwind Front) │
└────────────┬─────────────┘
             │ REST API
┌────────────▼─────────────┐
│  Express.js HMRC Mock    │
│  server_v5.js            │
│  • Auth (OAuth2)         │
│  • Expenses + Submissions│
│  • Final Declarations    │
│  • Metrics Dashboard     │
└────────────┬─────────────┘
             │
┌────────────▼─────────────┐
│   HMRC Sandbox API       │
│   (MTD 2026 Integration) │
└──────────────────────────┘

## ⚙️ Stack

- **Backend:** Node.js (Express)
- **Frontend:** React PWA (Progressive Web App)
- **Database:** JSON (local) → future MongoDB integration
- **Testing:** Postman + Newman + CI (GitHub Actions)
- **Hosting:** Mock + Local Dev
- **Version Control:** GitHub

---

## 🧠 HMRC API Integration

### OAuth2 Flow (Sandbox)

| Step | Endpoint | Method | Description |
|------|-----------|--------|-------------|
| 1 | `/oauth/token` | **POST** | Exchange `userId` for bearer token |
| 2 | `/expenses` | **GET** | Retrieve expense records for test data |
| 3 | `/income-tax-mtd/submissions/quarterly` | **POST** | Send quarterly summary |
| 4 | `/income-tax-mtd/submissions/:id/status` | **GET** | Check submission status |
| 5 | `/income-tax-mtd/final-declaration` | **POST** | Submit end-of-year figures |

---

## 📜 Making Tax Digital (MTD) 2026 Alignment

The **Ladox HMRC Mock Suite** is built to support the upcoming **Making Tax Digital for Income Tax (MTD-ITSA)** rollout scheduled for **April 2026**.  
Key features for compliance include:

- ✅ **Digital Record Keeping** — supports consistent transaction capture via PWA interface  
- ✅ **Quarterly Updates** — aligns with HMRC submission frequency  
- ✅ **End-of-Period Statement (EOPS)** — the `final-declaration` endpoint simulates this workflow  
- ✅ **OAuth2.0 Sandbox Auth** — matches HMRC's test environment auth model  
- ✅ **Data Validation & Error Codes** — mimics HMRC response structure (E221, E322, E410, E550, etc.)

---

## 🧩 Project Structure

```bash
Ladox_HMRC_MockSuite_v5_Full/
│
├── server_v5.js                # Express server with mock HMRC endpoints
├── data.json                   # Mock data (expenses, submissions, users)
├── package.json                # Dependencies and scripts
├── logs/
│   ├── audit.log               # Audit trail (accepted/rejected submissions)
│   └── access-YYYY-MM-DD.log   # HTTP request logs per date
├── docs/
│   ├── README.md               # This documentation
│   ├── wiki/
│   │   ├── architecture.md     # High-level architecture overview
│   │   └── api-reference.md    # Detailed endpoint documentation
├── tests/
│   ├── newman_collection.json  # Postman collection for automated tests
│   └── run_newman_tests.sh     # Shell script to run Newman tests in CI
└── exports/
    ├── visuals/                # Wireframes, investor visuals
    └── reports/                # Generated compliance or PDF docs

## 🚀 Running the Mock Server

### Local (Dev Mode)

```bash
npm install
npm start
Server runs on http://localhost:3000

Health check:

bash
Copy code
curl http://localhost:3000/health
Generate OAuth Token
bash
Copy code
curl -X POST http://localhost:3000/oauth/token \
  -H "Content-Type: application/json" \
  -d '{"userId":"user_001"}'
Fetch Test Expenses
bash
Copy code
curl http://localhost:3000/expenses \
  -H "Authorization: Bearer <your_token_here>"
Submit Final Declaration
bash
Copy code
curl -X POST "http://localhost:3000/income-tax-mtd/final-declaration?mode=random" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <your_token_here>" \
  -d '{
    "userId":"user_001",
    "taxYear":"2025-2026",
    "finalFigures":{"income":52000.00,"expenses":13750.50,"netProfit":38249.50}
  }'
markdown
Copy code

---

🔍 **Explanation:**
- The ```bash ... ``` fenced code blocks tell Markdown to render as code with Bash syntax highlighting.
- The `[http://localhost:3000](http://localhost:3000)` makes the link clickable.
- Each section header (`##`, `###`) creates the same formatting hierarchy you saw in your screenshot.  

So yes — everything from the *Running the Mock Server* heading downward in your image **is already Markdown** and renders exactly as you’re seeing it.  

Would you like me to show you a **preview-safe GitHub rendering** of this section (so you can see exactly how it’ll appear on GitHub before you commit it)?






You said:
markdown everything please you only did Running the Mock server. what about the rest that follows?


ChatGPT said:
Perfect — let’s do this properly and completely ✅

Below is the entire second half of your docs/README.md (starting from “🚀 Running the Mock Server” and including everything that follows it).
It’s written in pure GitHub Markdown, so you can paste it directly into your repo.

When you view it on GitHub, it will look exactly like your previous screenshots — code blocks, colored syntax, headings, tables, and clickable links.

markdown
Copy code
## 🚀 Running the Mock Server

### Local (Dev Mode)

```bash
npm install
npm start
Server runs on http://localhost:3000

Health check:

bash
Copy code
curl http://localhost:3000/health
Generate OAuth Token
bash
Copy code
curl -X POST http://localhost:3000/oauth/token \
  -H "Content-Type: application/json" \
  -d '{"userId":"user_001"}'
Fetch Test Expenses
bash
Copy code
curl http://localhost:3000/expenses \
  -H "Authorization: Bearer <your_token_here>"
Submit Final Declaration
bash
Copy code
curl -X POST "http://localhost:3000/income-tax-mtd/final-declaration?mode=random" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <your_token_here>" \
  -d '{
    "userId":"user_001",
    "taxYear":"2025-2026",
    "finalFigures":{"income":52000.00,"expenses":13750.50,"netProfit":38249.50}
  }'
📊 Metrics & Logging
View live metrics dashboard:
👉 http://localhost:3000/metrics?format=html

Metrics include:

Total submissions processed

Accepted vs Rejected count

HMRC error code frequency

Rejection rate percentage

Auto-refresh every 5 seconds

Audit logs are stored in:

bash
Copy code
logs/audit.log
Access logs (per request) are rotated daily:

bash
Copy code
logs/access-YYYY-MM-DD.log
🧱 Continuous Integration (CI)
GitHub Actions workflow automatically:

Installs dependencies (npm install)

Starts mock server against Postman mock endpoints

Executes Newman test suite

Uploads test results and logs as artifacts

Manual command to run locally:

bash
Copy code
./tests/run_newman_tests.sh
🎨 Visual & Branding (Ladox Design System)
Color	Purpose	Hex
Primary Purple	Buttons, Headers, Key UI	#5300BC
Mint Green	Accent Highlights	#CAFFC1
Orange	Separators & Alerts	#FF8C42
Light Purple	Secondary Background	#E7DDF3
Lightest Purple	Card Background	#F1EEF4
Base Background	Main Page	#F6F6F6

Typography

Type	Font	Size
Heading 1	Space Grotesk	90px
Heading 2	Space Grotesk	64px
Heading 3	Space Grotesk	44px
Heading 4	Space Grotesk	32px
Heading 5	Space Grotesk	24px
Paragraph 1	Space Grotesk	18px
Paragraph 2	Space Grotesk	16px
Paragraph 3	Space Grotesk	14px

🧩 Project Structure
bash
Copy code
Ladox_HMRC_MockSuite_v5_Full/
│
├── server_v5.js                # Express server with mock HMRC endpoints
├── data.json                   # Mock data (expenses, submissions, users)
├── package.json                # Dependencies and scripts
├── logs/
│   ├── audit.log               # Audit trail (accepted/rejected submissions)
│   └── access-YYYY-MM-DD.log   # HTTP request logs per date
├── docs/
│   ├── README.md               # This documentation
│   ├── wiki/
│   │   ├── architecture.md     # High-level architecture overview
│   │   └── api-reference.md    # Detailed endpoint documentation
├── tests/
│   ├── newman_collection.json  # Postman collection for automated tests
│   └── run_newman_tests.sh     # Shell script to run Newman tests in CI
└── exports/
    ├── visuals/                # Wireframes, investor visuals
    └── reports/                # Generated compliance or PDF docs
📜 Making Tax Digital (MTD) 2026 Alignment
The Ladox HMRC Mock Suite supports HMRC’s Making Tax Digital for Income Tax (MTD-ITSA) rollout (April 2026).

Key compliance features:

✅ Digital Record Keeping — structured JSON dataset

✅ Quarterly Updates — /income-tax-mtd/submissions/quarterly endpoint

✅ End-of-Period Statement (EOPS) — /final-declaration endpoint

✅ OAuth2.0 Sandbox Auth — mirrors HMRC test environment

✅ Realistic Validation & Error Codes — responses simulate HMRC’s live behaviour

📄 License
This project is licensed under the MIT License — free for educational, sandbox, and testing use.

💬 Contact
For feedback, contributions, or API collaboration:

Project: Ladox HMRC Mock Suite (MTD 2026)

Maintainer: Ladox Team

Email: support@ladox.co.uk

yaml
Copy code

---

✅ **That’s 100% Markdown.**
- Every section uses correct GitHub Markdown syntax (`##`, `###`, tables, and fenced code blocks).
- Copy it directly into your `docs/README.md` — it will render with headings, code boxes, and all formatting.









