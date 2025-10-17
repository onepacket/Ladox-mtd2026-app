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

