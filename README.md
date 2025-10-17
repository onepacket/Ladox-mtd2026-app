# 🧾 Ladox MTD 2026 App

**Ladox HMRC MockSuite v5** is a fully featured mock server and integration toolkit that simulates the **UK HMRC “Making Tax Digital” (MTD)** submission flow for self-employed individuals and small businesses.

This repository contains both:
- the **mock backend** (Node.js/Express) used for testing MTD API submissions, and  
- the **internal developer documentation suite** under the [`/docs`](./docs/) directory.

---

## 🚀 Key Features
- ✅ Simulates HMRC’s income tax submission APIs for MTD 2026.  
- 🔒 Implements OAuth-style token authentication (`/oauth/token`).  
- 💰 Auto-generates realistic income & expense data (MTD-compatible categories).  
- ⚙️ Tracks audit logs and metrics per user.  
- 🧠 Supports validation and random rejection modes for testing.  
- 📊 Postman + Newman test suite for automation.  
- 🧾 Fully documented with setup instructions and developer wiki.

---

## 🧱 Repository Structure

Ladox-mtd2026-app/
├── server_v5.js
├── package.json
├── /data/
├── /logs/
├── /docs/ ← internal documentation
│ ├── README.md
│ ├── Ladox_HMRC_MockSuite_v5_Full.md
│ ├── README_summary.md
│ ├── changelog.txt
│ ├── /postman/
│ └── /assets/
└── /tests/


---

## 🧩 Documentation
Full setup and developer guides are available in the [`/docs`](./docs/README.md) folder.

📘 **Direct links:**
- [Internal Developer Wiki](./docs/Ladox_HMRC_MockSuite_v5_Full.md)  
- [Quick Start Summary](./docs/README_summary.md)  
- [Change Log](./docs/changelog.txt)

---

## 💡 Quick Start

```bash
# Clone and install
git clone https://github.com/onepacket/Ladox-mtd2026-app.git
cd Ladox-mtd2026-app
npm install

# Run the mock server
npm start

# Verify health
curl http://localhost:3000/health


📬 Contact

Ladox Internal DevOps • devops@ladox.internal
© 2025 Ladox — We Live To Serve


