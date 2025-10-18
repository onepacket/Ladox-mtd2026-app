<p align="center">
  <img src="assets/ladox_logo_400x100.png" alt="Ladox Logo" width="180">
</p>

# 🧾 Ladox HMRC Developer & Operations Handbook  
### *Simplifying Making Tax Digital for 2026*  

**Maintained by:** Kaydee Dzvuke — CTO, Ladox Ltd  
🌐 [www.ladox.co.uk](https://www.ladox.co.uk)  
🕓 *Last updated: October 2025 — Version v5.0.0*  

> 💜 *We live to save*

---

## 📖 Table of Contents
1. [⚙️ Executive Summary](#-executive-summary)
2. [🧰 Why We Built Ladox](#-why-we-built-ladox).
   - [Software that Creates Digital Records](#software-that-creates-digital-records).
   - [Software that Connects to Your Records \(Bridging Software\)](#software-that-connects-to-your-records-(bridging-software)).
4. [🧱 System Architecture](#-system-architecture)
   - [Legacy Mock Server (v4)](#legacy-mock-server-v4)
   - [Modern Backend (v5 – FastAPI & MongoDB)](#modern-backend-v5--fastapi--mongodb)
   - [Performance Benchmark](#performance-benchmark)
5. [🧩 API & Endpoints](#-api--endpoints)
6. [🧪 Testing & Validation](#-testing--validation)
   - [Postman & Newman Workflows](#postman--newman-workflows)
   - [Curl Commands](#curl-commands)
7. [📈 Metrics & Debugging](#-metrics--debugging)
8. [🚀 Deployment Readiness Checklist](#-deployment-readiness-checklist)
9. [🏢 About Ladox Ltd](#-about-ladox-ltd)
10. [🤝 Contributing](#-contributing)

---

## ⚙️ Executive Summary

Ladox Ltd is building a digital-first platform that simplifies **Making Tax Digital (MTD)** submissions for self-employed individuals and small businesses in the UK.  
This handbook provides an internal technical reference for developers, testers, and operations engineers implementing the **Ladox HMRC MockSuite v5** for MTD 2026 compliance.

The mock suite is designed to simulate real HMRC endpoints, OAuth flows, and submission journeys for **quarterly updates**, **final declarations**, and **digital record** testing.

---
## 🧰 Why We Built Ladox
The UK’s Making Tax Digital (MTD) initiative requires businesses and the self-employed to
maintain digital financial records and submit tax information electronically.  
To support this, HMRC recognises two main software categories:  


### Software that Creates Digital Records


### Software that Connects to Your Records (Bridging Software)

Ladox was designed to do both — enabling full digital record creation **and** bridging for
those still using spreadsheets or legacy tools.


## 🧱 System Architecture

<p align="center">
  <img src="assets/architecture_diagram.png" alt="Ladox MTD Architecture Overview" width="720">
</p>
<p align="center"><em>Figure placeholder — replace with final visual.</em></p>

The system architecture integrates OCR-based receipt capture, backend data validation, and HMRC-compliant API submission.

### Legacy Mock Server (v4)

- Built on **Node.js + Express**  
- Purpose: local mock of MTD submission endpoints for early testing  
- Features:
  - `/expenses`, `/income-tax-mtd/final-declaration`, `/metrics`
  - Simulated OAuth token issuance
  - Randomized submission outcomes and error codes
  - Audit log persistence under `/logs/audit.log`

👉 [View Legacy Mock (server_v4.js)](https://github.com/onepacket/Ladox-mtd2026-app/blob/main/app/legacy/server_v4.js)

### Modern Backend (v5 – FastAPI & MongoDB)

- Located at `/app/backend/server.py`  
👉 [View Source](https://github.com/onepacket/Ladox-mtd2026-app/blob/main/app/backend/server.py)
- Tech Stack:
  - **FastAPI** — async web framework  
  - **Motor** — MongoDB async driver  
  - **Pydantic** — data validation  
  - **Uvicorn** — ASGI server  
  - **MongoDB** — document store for users, expenses, and submissions  
- Default port: `8001`
- Async endpoints for submissions, validation, and metrics  
- Built for compliance with MTD 2026 digital record requirements  

### Performance Benchmark

<p align="center">
  <img src="assets/perf_chart.png" alt="FastAPI vs Express Performance" width="720">
</p>
<p align="center"><em>Figure placeholder — replace with final visual.</em></p>

| Framework | Avg. Req/s | Avg. Latency | Notes |
|------------|-------------|--------------|--------|
| Express v4 | 450 | 78 ms | Legacy mock baseline |
| FastAPI v5 | 880 | 41 ms | Async improvements & caching |
| Gain | +95% | -47% | Doubling throughput |

---

## 🧩 API & Endpoints

### Authentication
OAuth 2.0 flow simulation for sandbox testing.

| Method | Endpoint | Description |
|---------|-----------|-------------|
| `POST` | `/oauth/token` | Issues mock access token |
| `GET` | `/expenses` | Retrieves user expenses |
| `POST` | `/income-tax-mtd/final-declaration` | Submits final declaration |
| `GET` | `/income-tax-mtd/submissions/:id/status` | Checks submission status |
| `GET` | `/metrics` | Retrieves performance metrics |

Example:
```bash
curl -X POST http://localhost:3000/oauth/token \
     -H "Content-Type: application/json" \
     -d '{"userId": "user_001"}'

Response:

{
  "access_token": "user_001_acf824e9-80fe-4465-96f6-be8278c0c847",
  "scope": "read write",
  "expires_in": 3600
}
```
## 🧪 Testing & Validation
###Postman & Newman Workflows

1. Import Postman collection:
👉 Postman Collection

2. Test suite includes:

- Token generation

- Expense retrieval

- Quarterly submission

- Final declaration simulation

3. Automate with Newman:
```bash
  newman run app/tests/postman_collection.json -e app/tests/postman_environment.json
```
### Curl Commands

Sample validation for final declaration:

``` bash
curl -X POST http://localhost:3000/income-tax-mtd/final-declaration \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer user_001_acf824e9-80fe-4465-96f6-be8278c0c847" \
  -d '{
    "userId": "user_001",
    "taxYear": "2025-2026",
    "finalFigures":{"income":52000.00,"expenses":13750.50,"netProfit":38249.50}
  }'
```
## 📈 Metrics & Debugging

The /metrics endpoint provides insights into submission activity and server health.


| Metric | Description |
|---------|-----------|
| acceptedCount | Number of successful submissions |
| rejectedCount | Number of rejected submissions |
| avgProcessingTime | Average latency per submission |
| uptime| Server uptime (seconds) |


Audit logs are stored under /logs/audit.log and track all API activity:
```bash
[2025-10-17T00:36:59.472Z] POST /income-tax-mtd/final-declaration mode=random 200
[2025-10-17T00:41:47.314Z] POST /income-tax-mtd/final-declaration mode=random 400 E322
```

##🚀 Deployment Readiness Checklist

| Item | Status |
|---------|-----------|
| Environment variables configured (.env)|	✅ |
| MongoDB running |✅ |
| OAuth keys (sandbox) generated|	✅ |
| Server port accessible|	✅ |
| Postman tests passing	| ✅ |
Logs writing to /logs/	✅

## 🏢 About Ladox Ltd

Ladox Ltd is a UK-based fintech startup dedicated to making digital tax compliance simple, secure, and accessible.
We believe that by embracing automation, AI, and intuitive design — we empower small businesses and self-employed individuals to thrive.

💬 “We live to save.”

## 🤝 Contributing

We welcome improvements, fixes, and suggestions from the Ladox developer community.

1. Fork the repository:
👉 Ladox MTD2026 App on GitHub

2. Create a new branch:
``` bash
git checkout -b docs/update-ladox-handbook
```
3. Commit and push your updates.

4. Open a Pull Request on GitHub.

🕓 All documentation contributions are reviewed within 3 business days.

## 🧾 Version Control

- **Current version:** v5.0.0 — October 2025
- **Maintainer:** Kaydee Dzvuke, CTO — Ladox Ltd
- **Format:** Enterprise Markdown, GitHub Wiki–ready

<p align="center"> <img src="assets/qr_ladox.png" alt="Ladox QR" width="120"><br> <em>Scan to visit <a href="https://www.ladox.co.uk">ladox.co.uk</a></em><br> 💜 <strong>We live to save</strong> </p> ```
