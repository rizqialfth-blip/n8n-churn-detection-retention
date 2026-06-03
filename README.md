# 🔍 Churn Detection & Retention Alert System
### Built with n8n · OpenAI · Google Sheets · Gmail

> **Automatically scans your entire customer base weekly, scores each customer's churn risk with AI, and sends personalized retention emails — before they cancel.**

📹 **[Watch Demo →](https://loom.com/[LOOM_LINK])**

---

## 📌 The Problem

Most businesses only find out a customer is leaving **after** they cancel.

By then, it's too late.

Customer success teams spend hours manually reviewing spreadsheets trying to identify who needs attention — and they still miss people. High-risk customers slip through. Revenue walks out the door silently.

---

## ✅ The Solution

This system runs automatically every week.

It pulls your full customer list, sends each record to OpenAI for risk analysis, scores every customer from 0–100, and automatically sends personalized retention emails to anyone at risk — all without a single manual step.

Your team wakes up Monday morning with at-risk customers already identified and already contacted.

---

## ⚡ ROI & Impact

| Metric | Manual Process | This System |
|--------|---------------|-------------|
| Time to identify at-risk customers | 3–5 hours/week | 0 minutes |
| Coverage | Partial (top accounts only) | 100% of customer base |
| Time to first retention outreach | 24–72 hours | Automatic, same run |
| Customers missed per week | 30–40% | 0% |
| Estimated time saved | — | ~4 hours/week |
| Estimated value protected | — | ~$400–800/month* |

> *Based on average SaaS churn cost of $200–400/churned customer × 2–4 prevented churns/month

---

## 🔄 Workflow Architecture

```
Schedule Trigger (Every Monday 9:00 AM)
        ↓
Get All Rows — Google Sheets
(Full customer database)
        ↓
Loop Over Items
(Process each customer individually)
        ↓
OpenAI — Churn Risk Scoring
(Score 0-100 + recommended action + reason)
        ↓
Code Node — Parse AI Response
(Extract score, action, reason from JSON)
        ↓
Update Row — Google Sheets
(Log score + action + reason per customer)
        ↓
IF Node — Risk Filter (Score ≥ 60)
        ↓
   ┌─────────────────────┐
   │                     │
 ✅ AT-RISK            ⏭️ SAFE
 (Score ≥ 60)         (Score < 60)
   │                     │
Gmail Retention        Skip
Email → Customer
   ↓
Update Row — Google Sheets
(Log email_sent: Yes)
```

---

## 🧩 Nodes Breakdown

| Node | Tool | Function |
|------|------|----------|
| Schedule Trigger | n8n | Fires every Monday 9:00 AM automatically |
| Get row(s) in sheet | Google Sheets | Pulls full customer database |
| Loop Over Items | n8n | Iterates through each customer record |
| Message a Model | OpenAI GPT-4o-mini | Scores churn risk + generates recommendation |
| Code in JavaScript | n8n | Parses AI JSON response into clean fields |
| Update row in sheet (1) | Google Sheets | Logs churn_score, risk_action, risk_reason |
| IF | n8n Logic | Filters customers with score ≥ 60 |
| Send a message | Gmail | Sends personalized retention email |
| Update row in sheet (2) | Google Sheets | Logs email_sent: Yes |

---

## 🤖 AI Scoring Logic

OpenAI evaluates each customer based on:

- **Login recency** — days since last active session
- **Usage rate** — monthly engagement percentage
- **Support friction** — number of open/recent tickets
- **Subscription tenure** — how long they've been a customer
- **Plan type** — Basic vs Pro vs Premium signals

**Output format (JSON):**
```json
{
  "churn_score": 75,
  "risk_action": "Offer personalized onboarding call",
  "risk_reason": "Low usage combined with multiple support tickets indicates friction"
}
```

**Risk tiers:**
| Score | Tier | Action |
|-------|------|--------|
| 80–100 | 🔴 Critical | Immediate outreach + retention offer |
| 60–79 | 🟡 At-Risk | Personalized email + check-in |
| 0–59 | 🟢 Healthy | Log only, no action |

---

## 📊 Google Sheets Structure

| Column | Type | Description |
|--------|------|-------------|
| customer_name | Text | Full name |
| email | Text | Contact email |
| plan_type | Text | Basic / Pro / Premium |
| subscription_start | Date | Signup date |
| last_login | Date | Most recent activity |
| support_tickets | Number | Open ticket count |
| monthly_usage | Percentage | Feature engagement rate |
| churn_score | Number | AI-generated risk score (0–100) |
| risk_action | Text | AI-recommended retention action |
| risk_reason | Text | One-sentence AI explanation |
| email_sent | Text | Yes / blank |

---

## 🛠️ Tech Stack

- **n8n** — Workflow automation engine
- **OpenAI GPT-4o-mini** — AI churn risk scoring
- **Google Sheets** — Customer database + logging
- **Gmail** — Automated retention email delivery

---

## 🚀 Setup Guide

### Prerequisites
- n8n instance (cloud or self-hosted)
- OpenAI API key
- Google Sheets with customer data
- Gmail account connected to n8n

### Steps

**1. Import Workflow**
```
n8n → Workflows → Import from File
→ upload churn-detection-retention.json
```

**2. Configure Credentials**
- OpenAI: Add API key under n8n Credentials
- Google Sheets: OAuth2 connection
- Gmail: OAuth2 connection

**3. Set Up Google Sheet**
Create sheet with columns listed in the structure table above.

**4. Set Schedule**
Default: Every Monday 9:00 AM
Adjust in Schedule Trigger node to match your timezone.

**5. Activate & Monitor**
- Toggle workflow to Active
- Check Executions tab each week
- Review Sheets for updated scores

---

## 📂 Repository Structure

```
n8n-churn-detection-retention/
├── README.md
├── workflow/
│   └── churn-detection-retention.json
├── assets/
│   └── workflow-screenshot.png
└── sample-data/
    └── churn_customers_sample.csv
```

---

## 📸 Workflow Screenshot

![Churn Detection & Retention Alert Workflow](./assets/workflow-screenshot.png)

---

## 👤 Built By

**Rizqi Alfatah**
n8n Automation Specialist

- 🌐 [Portfolio](https://rizqialfatah.com)
- 💼 [Upwork](https://upwork.com/freelancers/rizqialfatah)
- 🔗 [LinkedIn](https://linkedin.com/in/rizqialfatah)

---

## 📄 License

MIT — free to use and adapt with attribution.

---

*Built to demonstrate production-ready n8n automation for customer retention pipelines.*
