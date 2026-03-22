# 📋 Manual Setup Guide - Inbox Manager v2

**Follow this step-by-step guide to set up the workflow in your n8n instance.**

> **What this workflow does:**
> - Polls Gmail for unread emails every 5 minutes
> - Uses Google Gemini AI to classify emails (category, risk, priority, action)
> - Automatically sends alerts for high-risk emails
> - Logs everything to Google Sheets
> - Routes emails to 4 actions: auto-reply, draft, notify owner, or ignore

---

## ✅ Pre-Flight Checklist

Before you start, have these ready:

- [ ] Google account with Gmail
- [ ] Access to Google Cloud Console
- [ ] n8n instance (self-hosted or cloud.n8n.io)
- [ ] Google Sheet (empty or new)
- [ ] Text editor to copy/paste values

---

## 📝 STEP 1: Get Your Google Credentials

### 1.1 Gmail OAuth Token

1. Open n8n → **Credentials** → **Create new**
2. Choose: **Gmail OAuth2**
3. Click **Sign in with Google**
4. Select your Gmail account
5. Grant permissions
6. **Save** with name: `Gmail Account`

✅ **Done:** Gmail OAuth connected

### 1.2 Google Gemini API Key

1. Go to: https://aistudio.google.com/app/apikey
2. Click **Create API Key**
3. Copy the key (starts with `AIza...`)
4. In n8n → **Credentials** → **Create new**
5. Choose: **Google PaLM API**
6. Paste API key
7. **Save** with name: `Gemini API`

✅ **Done:** Gemini API connected

### 1.3 Google Sheets OAuth

1. In n8n → **Credentials** → **Create new**
2. Choose: **Google Sheets OAuth2**
3. Click **Sign in with Google**
4. Grant permissions
5. **Save** with name: `Google Sheets`

✅ **Done:** All 3 credentials ready

---

## 🔧 STEP 2: Prepare Your Google Sheet

1. Go to: https://sheets.google.com/
2. Click **+ Create new spreadsheet**
3. Name it: `Email Classification Log`
4. In the first row, add these headers:

```
A: Timestamp
B: Sender
C: Subject
D: Category
E: Risk
F: Priority
G: Action
H: Reasoning
```

5. Copy the sheet URL and extract the ID:
   ```
   https://docs.google.com/spreadsheets/d/[THIS_ID]/edit
   ```
   **Save this ID** (you'll need it later)

✅ **Done:** Google Sheet ready

---

## 📥 STEP 3: Import Workflow into n8n

1. Download: `Inbox_Manager_v2_Improved.json`
2. In n8n → **Workflows**
3. Click **Import from file**
4. Select the JSON file
5. Click **Import**

✅ **Done:** Workflow imported

---

## ⚙️ STEP 4: Connect Credentials in Workflow

### 4.1 Gmail Trigger

1. Double-click **Gmail Trigger** node (leftmost)
2. In **Credentials** dropdown → Select `Gmail Account`
3. **Save node** (press Escape or click outside)

### 4.2 Gemini Models (2 nodes to update)

1. Find **Google Gemini Chat Model** node (scroll right)
2. Double-click
3. In **Credentials** dropdown → Select `Gemini API`
4. **Save node**

Repeat for **Google Gemini Chat Model1** (find the second one)

### 4.3 Google Sheets

1. Find **Append or update row in sheet** node (scroll far right)
2. Double-click
3. In **Credentials** dropdown → Select `Google Sheets`
4. **Save node**

✅ **Done:** All credentials connected

---

## 🎯 STEP 5: Update Configuration Values

### 5.1 Update Your Email Address

1. Find node: **Send to owner**
2. Double-click
3. Look for field: **sendTo**
4. Change value from `yourname@gmail.com` → Your actual email
5. **Save node**

### 5.2 Update Google Sheet ID

1. Find node: **Append or update row in sheet**
2. Double-click
3. In dropdown: **Spreadsheet** → Click refresh icon
4. Select your sheet: `Email Classification Log`
5. In dropdown: **Sheet** → Select `Sheet1`
6. **Save node**

✅ **Done:** Configuration updated

---

## 🧪 STEP 6: Test the Workflow

### Before Activating:

1. Click **Execute Workflow** (▶️ button, top right)
2. Send yourself a test email with subject: `Test: Support Request`
3. Wait 10 seconds
4. Check Gmail:
   - [ ] Did you receive an alert email?
   - [ ] Does it show classification details?
5. Check Google Sheet:
   - [ ] Is there a new row with timestamp, sender, subject?

### Test Multiple Scenarios:

Send these test emails to verify different paths:

**Test 1: Support Email**
```
Subject: Need help with my account
Expected: Draft created or auto-reply sent
```

**Test 2: Invoice**
```
Subject: Invoice #12345 - Payment Due
Expected: Alert email sent to you
```

**Test 3: Newsletter**
```
Subject: Weekly Newsletter from Company
Expected: Marked low-priority, no alert
```

**Test 4: Suspicious**
```
Subject: Confirm your password immediately
Expected: Alert (detected as phishing)
```

---

## ✨ STEP 7: Activate the Workflow

When all tests pass:

1. Click **Activate** button (toggle in top-right corner)
2. Confirm when prompted
3. Status shows: **Active** (green)

✅ **Done:** Workflow running 24/7

---

## 📊 Daily Monitoring

**What to check:**
- Open **Google Sheet** and review new rows
- Verify classifications look correct
- Check for any suspicious patterns

**Weekly:**
- Click **Execution History** (in workflow)
- Look for ⚠️ errors
- Note any patterns

---

## 🐛 Troubleshooting

### Problem: "Gmail OAuth failed"
**Fix:**
1. Delete the Gmail credential
2. Create new Gmail OAuth
3. Re-authenticate with Google
4. Reconnect to Gmail Trigger

### Problem: "Gemini API key invalid"
**Fix:**
1. Go to https://aistudio.google.com/app/apikey
2. Create a new key
3. Delete old credential in n8n
4. Create new with fresh key
5. Reconnect Gemini nodes

### Problem: "Google Sheet not updating"
**Fix:**
1. Find **Append or update row in sheet** node
2. Double-click
3. Click refresh icon in Spreadsheet dropdown
4. Re-select your sheet

### Problem: "No emails being classified"
**Fix:**
1. Check Gmail Trigger is **Active** (blue)
2. Send yourself a test email
3. Click **Execute Workflow**
4. Check **Execution History** for errors

### Problem: "Alert email not arriving"
**Fix:**
1. Find **Send to owner** node
2. Verify email is correct
3. Check Gmail spam folder
4. Re-test with **Execute Workflow**

---

## 🔄 How to Make Changes

### Change Poll Interval (currently 5 min):
1. Click **Gmail Trigger**
2. Change **Poll Times** value
3. Save

### Change Who Gets Alerts:
1. Click **Send to owner**
2. Update email address
3. Save

### Add Custom Email Categories:
1. Click **Email Classification Agent**
2. Edit the prompt text
3. Update categories list
4. Save and test

---

## 📞 Quick Reference

### Email Categories:
```
Support      → Help requests, bugs
Financial    → Invoices, payments
Personal     → Casual, greetings
Sales        → Offers, partnerships
Legal        → Contracts, disputes
HR           → Hiring, employment
Scam         → Phishing, fraud
Spam         → Newsletters, bulk
Low_Priority → Notifications, updates
Other        → Doesn't fit above
```

### Risk Levels:
```
Low     → Safe to auto-reply
Medium  → Create draft (needs review)
High    → Alert owner (forced)
```

### Actions:
```
auto_send → Send reply immediately (rare)
draft     → Create Gmail draft (medium risk)
notify    → Send alert to owner (high risk)
ignore    → Do nothing (low priority)
```

---

## ✅ Setup Verification Checklist

- [ ] Gmail OAuth connected
- [ ] Gemini API key added
- [ ] Google Sheets credential connected
- [ ] Your email address in alert node
- [ ] Google Sheet created with headers
- [ ] Google Sheet ID configured
- [ ] Test emails sent and verified
- [ ] New rows appear in Sheet
- [ ] Workflow activated
- [ ] Ready to use!

**You're done!** 🎉

The workflow will now automatically process your emails. Check your Google Sheet daily to see classifications.

---

**Version:** 2.0 (Improved)
**Last Updated:** 2026-03-22

### 1️⃣ Gmail Trigger

Polls your inbox every **5 minutes** for unread emails.

### 2️⃣ Email Classification Agent

Uses **Google Gemini AI** with a Gmail tool to:
- Retrieve the **full email content** (not just snippet)
- Analyze sender, subject, and body
- Classify into 10 categories
- Assess risk and priority
- Recommend an action

Output: Structured JSON classification

```json
{
  "sender": "client@example.com",
  "subject": "Invoice #12345",
  "body": "Full email content...",
  "category": "Financial",
  "risk": "Medium",
  "priority": "High",
  "action": "draft",
  "reasoning": "Requires human approval before response"
}
```

### 3️⃣ Parse AI JSON Output

A JavaScript node safely parses the AI output with error handling.
- If parse fails → defaults to `notify` with high risk

### 4️⃣ Safety Layer

**Critical safety check:**
- If `risk === "High"` → **Force action to `notify`**
- Prevents dangerous emails from being auto-replied
- Ensures human review for sensitive emails

### 5️⃣ Route by Action

Switch node routes to 4 different paths:

| Action | Destination | When Used |
|--------|-------------|-----------|
| `auto_send` | Send reply immediately | Low-risk support only |
| `draft` | Create Gmail draft | Medium-risk, needs review |
| `notify` | Alert owner via email | High-risk, sensitive, malicious |
| `ignore` | Do nothing | Low-priority, spam |

### 6️⃣ Execute Actions

**AUTO_SEND Path:**
- Sends a pre-generated reply directly

**DRAFT Path:**
- Creates a Gmail draft for manual review before sending

**NOTIFY Path:**
- Generates a formatted alert email
- Sends to inbox owner with full classification details

**IGNORE Path:**
- No operation (email remains unread)

---

## 🧠 Classification Output Format

The AI Classification Agent returns a deterministic JSON response:

```json
{
  "sender": "email@address.com",
  "subject": "Original email subject",
  "body": "Full email body text",
  "category": "Support|Financial|Personal|Sales|Legal|HR|Scam|Spam|Low_Priority|Other",
  "risk": "Low|Medium|High",
  "priority": "Low|Medium|High",
  "action": "auto_send|draft|notify|ignore",
  "reasoning": "One sentence explaining the classification"
}
```

### Categories Explained

| Category | Examples | Typical Action |
|----------|----------|----------------|
| **Support** | Help requests, bug reports | auto_send or draft |
| **Financial** | Invoices, payments, refunds | notify or draft |
| **Personal** | Casual messages, greetings | auto_send (rare) |
| **Sales** | Offers, partnerships, deals | draft or ignore |
| **Legal** | Contracts, disputes | notify (always) |
| **HR** | Employment, hiring | draft or notify |
| **Scam** | Phishing, fraud, impersonation | notify (forced) |
| **Spam** | Newsletters, bulk emails | ignore |
| **Low_Priority** | Notifications, updates | ignore |
| **Other** | Doesn't fit above | draft or notify |

### Risk Levels

- **Low** - Safe to auto-reply (e.g., casual friend)
- **Medium** - Needs human review (e.g., work request)
- **High** - ALWAYS forced to notify (e.g., financial, legal, scam)

### Action Types

| Action | Outcome |
|--------|---------|
| `auto_send` | Reply sent automatically (rare) |
| `draft` | Gmail draft created for review |
| `notify` | Alert sent to inbox owner |
| `ignore` | No action (email stays unread) |

---

## 🛠 Requirements

To run this workflow you need:

- **n8n** (self-hosted or cloud)
- **Gmail OAuth credentials** (with send/read permissions)
- **Google Gemini API key** (free tier available)
- **Google Sheets document** (for audit logging)

---

## 🔧 Setup Instructions

### Step 1: Prepare Credentials

**Gmail OAuth:**
1. Go to https://console.cloud.google.com/
2. Create/select a project
3. Enable Gmail API
4. Create OAuth 2.0 credentials (Desktop app)
5. Download credentials JSON

**Google Gemini API:**
1. Visit https://aistudio.google.com/app/apikey
2. Create an API key
3. Copy the key

**Google Sheets:**
1. Create a new Google Sheet
2. Copy the spreadsheet ID from URL: `https://docs.google.com/spreadsheets/d/{SHEET_ID}/edit`

### Step 2: Import Workflow into n8n

1. Open your n8n instance
2. **Workflows** → **Import from file**
3. Select `Inbox_Manager_v2_Improved.json`
4. Click **Import**

### Step 3: Connect Credentials in n8n

In the workflow, connect the following nodes:

**Gmail Trigger node:**
- Click **Credentials** dropdown
- Select or create: `Gmail OAuth2`
- Authenticate with your Google account

**Gemini Model nodes:**
- Click **Credentials** dropdown
- Select or create: `Google PaLM API`
- Paste your Gemini API key

**Log to Google Sheets node:**
- Click **Credentials** dropdown
- Select or create: `Google Sheets OAuth2`
- Authenticate with your Google account

### Step 4: Update Configuration

**In "Send Alert to Owner" node:**
```json
"sendTo": "=your-email@gmail.com"
```
Replace with your actual email address.

**In "Log to Google Sheets" node:**
```json
"documentId": {
  "value": "YOUR_SHEET_ID_HERE",
  "mode": "list"
}
```
Replace `YOUR_SHEET_ID_HERE` with your Google Sheet's ID.

**Create Google Sheet columns:**
- Timestamp
- Sender
- Subject
- Category
- Risk
- Priority
- Action
- Reasoning

### Step 5: Test & Activate

1. Send yourself a test email
2. Click **Execute Workflow** in n8n
3. Check:
   - Gmail drafts/replies created
   - Alert email sent
   - Google Sheets updated

4. If all tests pass: Click **Activate**

---

## 💡 Example Use Cases

This automation can be used for:

📬 Personal AI email assistant
💼 Client communication automation
🤖 Customer auto-reply system
⚙️ Inbox productivity workflow
📊 VIP sender prioritization

---

## 📂 Workflow Architecture

```
Gmail Trigger (every 5 min)
        ↓
Email Classification Agent
  + Gmail Tool (retrieves full email)
  + Gemini Model
        ↓
    Parse AI JSON
        ↓
 Safety Layer (IF)
    ↙       ↘
High Risk   Normal
   ↓           ↓
Force → notify  Keep action
   ↓           ↓
   └─────┬─────┘
         ↓
   Route by Action
   ↙  ↓  ↓  ↘
  A   B  C   D
  ↓   ↓  ↓   ↓

A: auto_send → Send Reply
B: draft → Create Gmail Draft
C: notify → Generate Alert → Send to Owner
D: ignore → Do Nothing

   ↓   ↓  ↓   ↓
   └───┴──┴───┘
       ↓
   Merge All
       ↓
Log to Sheets
```

### Detailed Node Map

| Node | Type | Purpose |
|------|------|---------|
| Gmail Trigger | Trigger | Polls inbox every 5 min |
| Email Classification Agent | LangChain Agent | AI classification with Gmail tool |
| Gemini Model | LLM | Powers classification |
| Read Full Email (Tool) | Gmail Tool | Retrieves complete email |
| Parse AI JSON Output | Code | Safely parses AI output |
| Safety Layer | IF | Forces high-risk → notify |
| Route by Action | Switch | 4-way router by action field |
| Send Reply / Draft / Alert / NoOp | Actions | Execute classified action |
| Merge | Merge | Consolidate all 4 paths |
| Log to Sheets | Google Sheets | Audit trail |

---

## 🔒 Safety & Best Practices

### Safety Features

✅ **Safety Layer** - Forces all high-risk emails to owner notification
✅ **Error Handling** - Graceful fallback if JSON parsing fails
✅ **No Placeholder Text** - AI prevents unsafe placeholder text
✅ **Audit Trail** - Every email logged to Google Sheets
✅ **Human Review** - Draft path forces human approval

### Email Categories to Monitor

⚠️ **High Risk** (auto-forced to `notify`):
- Phishing/scam emails
- Financial transactions
- Legal documents
- HR/employment offers
- Suspicious sender patterns

⚠️ **Medium Risk** (draft path):
- Important client communications
- Work-related requests
- Ambiguous intent emails

✅ **Safe to Auto-Reply**:
- Known supportive senders
- Simple status requests
- Internal team communications

### Common Customizations

**To allow auto-replies for a specific sender:**
- Modify AI prompt to mark certain categories as "auto_send"

**To escalate certain categories:**
- Edit Safety Layer conditions

**To add Discord/Slack notifications:**
- Add nodes after "Send Alert to Owner"

---

## 🧪 Testing Checklist

Send test emails to verify each path:

- [ ] Support request (expect: auto_send or draft)
- [ ] Invoice email (expect: notify)
- [ ] Newsletter (expect: ignore)
- [ ] Suspicious email (expect: notify)
- [ ] Check Google Sheets for all entries
- [ ] Verify alert email received for high-risk emails

---

## 📈 Performance Notes

- **Poll Interval:** 5 minutes (adjustable in Gmail Trigger)
- **API Limits:** Respects Gmail & Gemini rate limits
- **Processing Time:** ~10-30 seconds per email
- **Cost:** Gemini flash-lite model (low cost)

---

## 🔄 Version History

| Version | Date | Changes |
|---------|------|---------|
| 2.0 | 2026-03-22 | Complete rewrite: Full classification, 4 action routes, safety layer |
| 1.0 | Earlier | VIP sender filtering with AI reply generation |

---

## 💡 Advanced Customizations

**Multi-language Support:**
- Modify Gemini prompts to analyze language
- Route to translation services before reply

**CRM Integration:**
- Add HubSpot/Salesforce node after classification
- Update CRM based on email category

**Slack Notifications:**
- Replace/supplement Gmail alert with Slack
- Add real-time notifications for high-risk emails

**Email Sentiment Analysis:**
- Extend AI prompt to include sentiment
- Route angry emails to specific handling

**Response Time Metrics:**
- Add timestamp tracking
- Log response times to Sheets
- Build analytics dashboard

---

## 📞 Troubleshooting

### Issue: "Failed to parse AI output"
**Solution:** Check Gemini API is responding. Look at execution logs for AI output.

### Issue: Emails not being classified
**Solution:**
- Verify Gmail Trigger is activated
- Check unread filter working
- Ensure Gemini credentials connected

### Issue: Alert email not sent
**Solution:**
- Verify recipient email in "Send Alert to Owner"
- Check Gmail has send permission
- Test manual workflow execution

### Issue: Google Sheets not updating
**Solution:**
- Verify sheet ID is correct
- Check columns match expected schema
- Ensure Google Sheets credentials active

---

## 📄 License

MIT License - Part of n8n Portfolio

---

## ⭐ Credits

Built with **Google Gemini 2.5 Flash** and **n8n** automation platform.

Perfect for:
- 📬 Personal AI email assistant
- 💼 Team inbox automation
- 🤖 Customer support automation
- ⚙️ Enterprise email routing

---

**Ready to deploy?** Start with the [Setup Instructions](#-setup-instructions) above! 🚀
