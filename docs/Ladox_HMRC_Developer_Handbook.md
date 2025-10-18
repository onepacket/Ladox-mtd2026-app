# Ladox HMRC Mock Suite v5 — Developer & Operations Handbook

**Document version:** 1.0  
**Suite:** `Ladox_HMRC_MockSuite_v5_Full.zip`  
**Audience:** Developers, QA, Operations

---

## 1) Introduction

### 1.1 Making Tax Digital (MTD) 2026 Overview

Making Tax Digital (MTD) for Income Tax Self-Assessment (ITSA) mandates digital record keeping and quarterly updates via approved software by April 2026.  
This suite simulates HMRC endpoints to help developers and QA teams test integrations safely before production onboarding.

### 1.2 What is Ladox_HMRC_MockSuite_v5_Full?

A complete mock environment that replicates HMRC API behaviors for local/offline testing.

**Key features:**
- OAuth2 mock authorization
- Income & expense submission simulation
- Randomized acceptance/rejection logic
- Audit logs & metrics dashboard
- Postman/Newman-ready testing collection

---

## 2) Environment Requirements

| Component | Version | Notes |
|------------|----------|--------|
| OS | Linux/macOS/Windows (via WSL2) | Cross-platform |
| Node.js | v18+ (v20 recommended) | Required for Express server |
| npm | v8+ | Dependency manager |
| curl | latest | For CLI testing |
| Postman | latest | GUI API testing |
| Newman | latest | CLI testing |

---

## 3) Folder Structure

```
Ladox_HMRC_MockSuite_v5_Full/
│
├── server_v5.js              # Express server with all mock endpoints
├── package.json              # Dependencies and npm scripts
├── data/mock_data.json       # Persistent test data store
├── logs/audit.log            # Records of ACCEPT/REJECT actions
├── logs/access-YYYY-MM-DD.log# Access logs
├── tests/newman_collection.json # Postman/Newman test collection
├── docs/                     # Internal developer documentation
└── README.md                 # Overview
```

---

## 4) Installation

```bash
cd ~/Documents
unzip Ladox_HMRC_MockSuite_v5_Full.zip
cd Ladox_HMRC_MockSuite_v5_Full
npm install
npm start
```

Verify service:
```bash
curl http://localhost:3000/health
# Expected: {"status":"ok"}
```

---

## 5) Configuration

| Variable | Default | Description |
|-----------|----------|--------------|
| `PORT` | 3000 | HTTP port |
| `DATA_PATH` | ./data/mock_data.json | Persistent data file |
| `LOG_DIR` | ./logs | Log directory |
| `DEFAULT_REJECT_RATE` | 15 | Random rejection % |
| `TOKEN_TTL_SECS` | 3600 | Token expiry |

---

## 6) API Endpoints

### 6.1 Token Generation
```bash
curl -X POST http://localhost:3000/oauth/token   -H "Content-Type: application/json"   -d '{"userId":"user_001"}'
```
**Response:**
```json
{"access_token":"user_001_token","expires_in":3600}
```

### 6.2 Get Expenses
```bash
curl http://localhost:3000/expenses   -H "Authorization: Bearer user_001_token"
```

### 6.3 Submit Quarterly Update
```bash
curl -X POST http://localhost:3000/income-tax-mtd/submissions/quarterly   -H "Authorization: Bearer user_001_token"   -H "Content-Type: application/json"   -d '{"userId":"user_001","quarter":"Q1","year":"2025-2026","income":25000,"expenses":5000}'
```

### 6.4 Final Declaration
```bash
curl -X POST "http://localhost:3000/income-tax-mtd/final-declaration?mode=random"   -H "Authorization: Bearer user_001_token"   -H "Content-Type: application/json"   -d '{"userId":"user_001","taxYear":"2025-2026","finalFigures":{"income":52000,"expenses":13750,"netProfit":38250}}'
```

### 6.5 Metrics
```bash
curl http://localhost:3000/metrics -H "Authorization: Bearer user_001_token"
```

---

## 7) Testing with Postman/Newman

### 7.1 Import into Postman
1. Open Postman
2. Import `tests/newman_collection.json`
3. Set environment variable `baseUrl=http://localhost:3000`
4. Add token pre-request script to obtain OAuth token

### 7.2 Run with Newman
```bash
npm install -g newman
newman run tests/newman_collection.json --env-var baseUrl=http://localhost:3000
```

Expected: All tests pass; some rejections in random mode.

---

## 8) Logging & Auditing

**Audit log sample:**
```
[2025-10-17T01:21:13Z] ACCEPT user=user_001 submission=final_1234
[2025-10-17T01:21:25Z] REJECT user=user_001 code=E550 reason="Tax year not open"
```

**Access log sample:**
```
[2025-10-17T00:41:47Z] POST /final-declaration 400 15.2ms
```

---

## 9) Metrics JSON Response
```json
{
  "service": "Ladox HMRC Mock Suite v5",
  "uptime": "123.4s",
  "totalRequests": 50,
  "accepted": 40,
  "rejected": 10,
  "rejectionRate": "20%",
  "errorBreakdown": [{"code":"E322","count":3}]
}
```

---

## 10) HMRC Error Codes

| Code | Meaning |
|------|----------|
| E104 | Missing fields |
| E105 | Incomplete figures |
| E221 | Invalid period |
| E322 | Income < expenses |
| E410 | Duplicate submission |
| E550 | Tax year closed |

---

## 11) Integration Guidelines

- **OCR Frontend:** Ensure output JSON schema: `{id,userId,date,category,description,amount}`.
- **Third-Party Apps:** Integrate via RESTful API POST `/expenses` or direct file import.
- **HMRC Sandbox:** Replace mock URLs with live sandbox endpoints.

---

## 12) Troubleshooting

| Issue | Cause | Fix |
|--------|-------|------|
| No server response | Port blocked | Change PORT env var |
| Always accepted | Random mode off | Add `?mode=random` to URL |
| Missing logs | Folder absent | `mkdir -p logs` |
| Token expired | TTL exceeded | Request new token |

---

## 13) Commands Summary

```bash
npm install              # install dependencies
npm start                # start mock server
curl /health             # verify running
newman run tests/...     # run automated tests
```

---

## 14) Expected Results
- All endpoints reachable
- Randomly mixed acceptance/rejection responses
- Logs and metrics updating in real-time

---

**Ladox HMRC Mock Suite v5 — Simplifying MTD Testing for Developers**
