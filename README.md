# 🤖 AI Smart Email Auto-Responder (Gemini Agent)

An AI-powered Gmail automation workflow built with **n8n** that reads incoming emails, analyzes them using **Google Gemini AI**, and automatically sends intelligent replies based on sentiment detection and email classification.

This project demonstrates how to build an AI-powered email auto-responder using workflow automation, LangChain agents, and large language models.

---

## 🚀 Features

✅ Monitor Gmail for new unread emails
📥 Extract email sender, subject, and message body
🤖 AI-powered email triage using Google Gemini
💬 Automatic reply generation with tone and sentiment matching
🛡 Spam, newsletter, and automated email filtering
📊 Generate structured AI output including:

- Reply decision (`should_reply`)
- Email type classification
- Tone recommendation
- Sentiment detection
- AI confidence score

🏷 Flag uncertain emails for manual review
✉️ Mark processed emails as read

---

## ⚙️ How It Works

The workflow processes emails in the following steps:

### 1️⃣ Gmail Trigger

The workflow watches your inbox for **new unread emails** every minute.

### 2️⃣ AI Agent - Triage

The email content is sent to a **Google Gemini AI agent** that analyzes the message and returns a structured decision.

Example AI output:

```json
{
  "should_reply": true,
  "type": "customer_inquiry",
  "tone": "professional",
  "sentiment": "neutral",
  "confidence": 0.92,
  "reason": "Customer asking about pricing"
}
```

### 3️⃣ Parse Decision

A JavaScript node extracts and validates the AI response, ensuring valid JSON output.

If parsing fails, the email is flagged for manual review.

### 4️⃣ Should Reply?

The workflow checks **three conditions** (all must pass):

- `should_reply` = `true`
- `confidence` >= `0.7`
- `type` != `spam`

### 5️⃣ AI Agent - Write Reply

If approved, a second **Gemini AI agent** generates a reply with:

- Tone matching (professional, friendly, formal)
- Sentiment-aware adjustments
- Strict 120-word limit
- No placeholder text (e.g., `[Your Name]`)

### 6️⃣ Send Reply via Gmail

The generated reply is sent as a Gmail reply to the original thread.

### 7️⃣ Mark as Read

The processed email is automatically marked as read to prevent reprocessing.

### 8️⃣ Manual Review Fallback

Emails that fail triage or have low confidence are labeled for **manual review**.

---

## 🧠 AI Triage Classification

The AI triage agent returns structured JSON:

```json
{
  "should_reply": true,
  "type": "customer_inquiry",
  "tone": "professional",
  "sentiment": "neutral",
  "confidence": 0.92,
  "reason": "brief reason"
}
```

### Fields Explained

| Field | Description |
|-------|-------------|
| `should_reply` | Whether the email needs an automated reply |
| `type` | Email category |
| `tone` | Recommended reply tone |
| `sentiment` | Detected emotional tone |
| `confidence` | AI confidence score (0.0 – 1.0) |
| `reason` | Brief explanation of the decision |

### Email Types

| Type | Example |
|------|---------|
| `customer_inquiry` | Product or service questions |
| `support_request` | Help or troubleshooting |
| `faq` | Frequently asked questions |
| `client_reply` | Follow-up from a client |
| `spam` | Junk or irrelevant emails |
| `newsletter` | Marketing newsletters |
| `automated` | System-generated emails |
| `other` | Uncategorized |

### Sentiment Detection

| Sentiment | AI Behavior |
|-----------|-------------|
| `positive` | Match their energy, be warm |
| `neutral` | Stay professional and helpful |
| `negative` | Respond calmly, acknowledge frustration |
| `urgent` | Acknowledge urgency, be concise |

### Confidence Score

| Range | Meaning |
|-------|---------|
| 0.9 – 1.0 | Very confident |
| 0.7 – 0.89 | Fairly confident → auto-reply |
| 0.5 – 0.69 | Uncertain → manual review |
| Below 0.5 | Low confidence → skip |

---

## 🛠 Requirements

To run this workflow you need:

- **n8n**
- **Gmail OAuth credentials**
- **Google Gemini API key**

---

## 🔧 Setup

### 1️⃣ Install n8n

Example using Docker:

```bash
docker run -it --rm \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

Then open:

http://localhost:5678

### 2️⃣ Connect Gmail

Create Gmail OAuth credentials and connect them in n8n.

Google Cloud Console:

https://console.cloud.google.com/

### 3️⃣ Add Gemini API

Create an API key:

https://aistudio.google.com/app/apikey

Add the key to both Gemini Chat Model nodes in n8n.

### 4️⃣ Import the Workflow

1. Open n8n
2. Go to **Workflows**
3. Click **Import**
4. Paste the workflow JSON from `ai-email-auto-responder-gemini-agent.json`

### 5️⃣ Configure Labels

Open the **"Label - Needs Manual Review"** node and select your desired Gmail label from the dropdown.

---

## 💡 Example Use Cases

This automation can be used for:

📬 Automated customer support replies
💼 Client email management
❓ FAQ auto-responses
🤖 AI productivity and inbox automation
⚙️ Gmail workflow automation
📊 Email sentiment analysis

---

## 📂 Project Architecture

```
Gmail Trigger (Unread Emails)
        ↓
AI Agent - Triage (Gemini)
        ↓
  Parse Decision
        ↓
  Should Reply? (confidence ≥ 0.7)
       ↙          ↘
    YES              NO
     ↓                ↓
AI Agent -      Check Manual
Write Reply      Review?
     ↓              ↙    ↘
  Reply           Label    Skip
 Succeeded?     (Manual
    ↙    ↘      Review)
  Yes    No
   ↓      ↓
 Send   Label
 Reply  (Manual
   ↓    Review)
 Mark
as Read
```

---

## 🔒 Notes

Avoid automatically replying to:

- System alerts
- Sensitive emails
- Financial notifications
- Legal or contract emails

The AI will **never** reply to:

- Spam or marketing emails
- Newsletters
- Automated/noreply emails

Replies are strictly **under 120 words**.

The AI **never** uses placeholder text like `[Your Name]` or `[Company]`.

Low-confidence emails are routed to **manual review**.

Additional filters can be added to improve classification accuracy.

---

## 📜 License

MIT License

---

## 💡 Inspiration

This project demonstrates how AI and workflow automation can be combined to build:

- AI email assistants
- Automated inbox management
- Business workflow automation
- AI productivity systems

You can expand this project by adding:

- Multi-language reply support
- CRM integrations
- Slack or Notion notifications
- Email analytics dashboards
- Custom reply templates per email type

---

⭐ **Feel free to fork the workflow and adapt it for your own AI automation projects.**
