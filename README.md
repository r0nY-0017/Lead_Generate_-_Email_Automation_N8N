# 🔥 Lead Generation & Email Automation System


An end-to-end automated lead generation and email follow-up system built with **n8n**, **Google Sheets**, **Gmail**, and **OpenAI**. When a potential client submits the lead form, the system automatically scores them, saves their data, sends a personalized email, and handles all future replies using AI.
---

=======
![alt text](<Screenshot 2026-03-14 034021.png>)
![alt text](<Screenshot 2026-03-14 034106.png>)
![alt text](<Screenshot 2026-03-14 034037.png>)

## 🚀 Features

- **Lead Capture Form** — Beautiful, responsive HTML form with real-time budget slider, timeline selection, and validation
- **Automatic Lead Scoring** — Scores each lead out of 100 based on budget, timeline, company size, and engagement
- **Smart Categorization** — Automatically classifies leads as 🔥 Hot, ⚡ Warm, or ❄️ Cold
- **Personalized Emails** — Sends category-specific HTML emails with lead score, services, and pricing
- **AI-Powered Reply Handler** — Detects incoming replies and generates context-aware responses using OpenAI GPT-4o-mini
- **Google Sheets CRM** — All lead data, scores, statuses, and thread IDs saved and updated automatically
- **Thread-Aware Conversations** — Replies are matched by Thread ID or Email to maintain conversation context

---

## 🗂️ Project Structure

```
fire-ai-lead-system/
│
├── lead-form.html              # Lead capture form (Light theme, Poppins font)
│
├── emails/
│   ├── hot_lead_email.html     # Email template for Hot leads (Score ≥ 75)
│   ├── warm_lead_email.html    # Email template for Warm leads (Score ≥ 50)
│   └── cold_lead_email.html    # Email template for Cold leads (Score < 50)
│
├── workflows/
│   ├── lead-workflow.json      # Phase 1 — Lead capture & email sending
│   └── phase2_workflow.json    # Phase 2 — AI reply automation
│
├── LeadGenerator_Sheets.xlsx   # Google Sheets template
│
└── README.md
```

---

## ⚙️ How It Works

### Phase 1 — Lead Capture & Email

```
Lead Form Submission
        ↓
Calculate Lead Score (0–100)
        ↓
Save to Google Sheets
        ↓
Hot (≥75) → Send Hot Email
Warm (≥50) → Send Warm Email
Cold (<50) → Send Cold Email
```

### Phase 2 — AI Reply Automation

```
Gmail Trigger (polls every 1 min)
        ↓
Extract & Validate Email
        ↓
Match by Thread ID  →  Lead Found ✅
        ↓ (if not found)
Match by Email      →  Lead Found ✅
        ↓ (if not found)
Ignore ❌
        ↓
Prepare AI Prompt with Lead Context
        ↓
Generate Reply (OpenAI GPT-4o-mini)
        ↓
Send Reply in Same Thread
        ↓
Update Lead Status in Google Sheets
```

---

## 📊 Lead Scoring Logic

| Factor | Criteria | Points |
|---|---|---|
| **Budget** | ≥ $20k | 30 |
| | ≥ $10k | 22 |
| | ≥ $5k | 15 |
| | ≥ $1k | 8 |
| **Timeline** | Immediate | 30 |
| | 1–3 Months | 18 |
| | 6+ Months | 5 |
| **Company Size** | 500+ | 25 |
| | 201–500 | 20 |
| | 51–200 | 14 |
| | 11–50 | 8 |
| **Engagement** | Needs > 150 chars | 15 |
| | Needs > 80 chars | 10 |
| | Needs > 30 chars | 6 |

**Categories:**
- 🔥 **Hot** — Score ≥ 75 (HIGH priority)
- ⚡ **Warm** — Score ≥ 50 (MEDIUM priority)
- ❄️ **Cold** — Score < 50 (LOW priority)

---

## 🛠️ Setup Guide

### Prerequisites
- [n8n](https://n8n.io) account (Cloud or Self-hosted)
- Google account with Gmail and Google Sheets
- OpenAI API key

---

### Step 1 — Google Sheets

1. Upload `LeadGenerator_Sheets.xlsx` to Google Drive
2. Open as Google Sheets
3. Copy the Sheet ID from the URL:
   ```
   https://docs.google.com/spreadsheets/d/[SHEET_ID]/edit
   ```

**Required columns in the `Leads` sheet:**
```
SL | Timestamp | First Name | Last Name | Full Name | Email | Phone |
Company | Industry | Company Size | Budget (USD) | Timeline | Needs |
Lead Score | Category | Priority | Score Breakdown |
<<<<<<< Updated upstream
Email Sent | Status | Thread ID | Last Reply | Reply Sent At
=======
Email Sent | Status | Thread ID
>>>>>>> Stashed changes
```

> **Note:** The `SL` column uses this formula — paste in cell A2 and drag down:
> ```
> =IF(E2="","",ROW()-1)
> ```

---

### Step 2 — Import Workflows into n8n

1. Open n8n → **Workflows** → **Import from file**
2. Import `lead-workflow.json` (Phase 1)
3. Import `phase2_workflow.json` (Phase 2)

---

### Step 3 — Configure Credentials

In n8n, set up the following credentials:

- **Google Sheets OAuth2** — Connect your Google account
- **Gmail OAuth2** — Connect your Gmail account
- **OpenAI API Key** — Add your key from [platform.openai.com](https://platform.openai.com)

---

### Step 4 — Update Placeholders

Search and replace these values in both workflows:

| Placeholder | Replace With |
|---|---|
| `YOUR_GOOGLE_SHEET_ID_HERE` | Your actual Sheet ID |
| `YOUR_OPENAI_API_KEY_HERE` | Your OpenAI API key |
<<<<<<< Updated upstream
| `mehedi0017.fireai@gmail.com` | Your Gmail address |
=======
| `example@gmail.com` | Your Gmail address |
>>>>>>> Stashed changes

---

### Step 5 — Update Webhook URL in Form

Open `lead-form.html` and update:
```js
const N8N_WEBHOOK_URL = "https://your-n8n-instance.com/webhook/lead-capture";
```

---

### Step 6 — Activate Workflows

In n8n, toggle both workflows from **Inactive → Active**.

That's it! The system will now run automatically. 🎉

---

## 📧 Email Templates

| Template | Trigger | Color Theme |
|---|---|---|
| `hot_lead_email.html` | Score ≥ 75 | 🔴 Orange/Red |
| `warm_lead_email.html` | Score ≥ 50 | 🟡 Amber/Yellow |
| `cold_lead_email.html` | Score < 50 | ⚫ Slate/Grey |

All templates include:
- Lead score display
- 4 core services overview
- 3 pricing packages (Starter / Growth / Enterprise)
- Personalized greeting using lead data

---

## 🤖 AI Reply System

The Phase 2 workflow uses **OpenAI GPT-4o-mini** to generate replies. The AI is given full lead context including:

- Lead's name, company, industry
- Their original needs/requirements
- Lead category (Hot/Warm/Cold)
- Their latest message snippet

The tone of the AI reply automatically adjusts based on lead category:
- 🔥 **Hot** — Urgent, action-oriented, push for immediate call
- ⚡ **Warm** — Friendly, suggest discovery call
- ❄️ **Cold** — Polite, nurturing, low-commitment next step

---

## 📌 Notes

- Phase 1 and Phase 2 can run as **separate workflows** or on the **same canvas** in n8n
- The system uses **Thread ID matching** first, then falls back to **Email matching** for reply detection
- Emails are sent via Gmail — make sure your Gmail account has sending permissions enabled

---

## 🔥 Built By
**Md Mehedi Hasan Rony**

📧 hasan15-5976@diu.edu.bd
