# 🤖 Automated Intelligent Inbox Manager (Powered by Gemini & n8n)

An AI-powered Gmail automation workflow built with **n8n** that reads incoming emails, detects important senders, and automatically generates professional replies using **Google Gemini AI**.

This project demonstrates how to build an intelligent inbox management system with VIP sender filtering, AI reply generation, and safety checks.

---

## 🚀 Features

✅ Monitor Gmail for new unread emails
⭐ Detect important senders (VIP list)
🤖 Generate replies using Google Gemini AI
🛡 Safety check — only sends when AI confirms a reply is needed
📤 Replies in the same email thread (not a new email)
📬 Automatically archives replied threads
📖 Marks unimportant emails as read
⚙️ Error handling — AI failures don't crash the workflow

---

## ⚙️ How It Works

The workflow processes emails in the following steps:

### 1️⃣ Gmail Trigger

The workflow watches your inbox for **new unread emails** every minute.

### 2️⃣ Important Senders Config

A configurable VIP sender list filters which emails get AI replies.

```javascript
const importantSenders = [
  "vip@example.com",
  "client@example.com"
];
```

### 3️⃣ Check Sender Match

Compares the email sender against your VIP list.

### 4️⃣ Is Important?

Routes the email based on sender match:

- **Yes** → Continue to AI reply pipeline
- **No** → Mark as read and skip

### 5️⃣ Star Email

Important emails are labeled with `CATEGORY_PERSONAL` for visibility.

### 6️⃣ Get & Extract Email Content

The full email is retrieved and the body is decoded from Gmail's Base64 format, extracting:

- Sender (From)
- Subject
- Email body

### 7️⃣ AI Agent (Gemini)

The extracted email is sent to **Google Gemini AI**, which analyzes the message and generates a structured response.

Example AI output:

```
SUBJECT: Re: Project Update

EMAIL BODY:
Thank you for the update. I've reviewed the details and everything looks good.
Please proceed with the next steps as planned.

Best regards

ACTION: Reply
```

### 8️⃣ Parse AI Output

A JavaScript node extracts the structured fields from the AI response:

- `response_subject` — Reply subject line
- `response_body` — Reply message
- `response_action` — Reply / Ask for clarification / No response needed

If the AI agent fails, it defaults to "No response needed" instead of crashing.

### 9️⃣ Should Send?

A safety check that verifies **two conditions** before sending:

- `response_action` is NOT "No response needed"
- `response_body` is NOT empty

### 🔟 Reply via Gmail

The reply is sent **in the same email thread** — not as a new email.

### 1️⃣1️⃣ Remove Labels

After replying, the thread's `UNREAD` and `INBOX` labels are removed to keep the inbox clean.

---

## 🧠 AI Response Format

The AI agent returns structured output:

```
SUBJECT: [Reply subject line]

EMAIL BODY:
[Full reply message]

ACTION:
[Reply / Ask for clarification / No response needed]
```

### Action Types

| Action | Behavior |
|--------|----------|
| `Reply` | Send the generated reply |
| `Ask for clarification` | Send a follow-up question |
| `No response needed` | Skip — do not send |

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

Add it to the Google Gemini Chat Model node in n8n.

### 4️⃣ Configure Important Senders

Edit the **"Important Senders Config"** node and replace the example emails with your real VIP addresses:

```javascript
const importantSenders = [
  "your-vip@example.com",
  "your-client@example.com"
];
```

### 5️⃣ Import the Workflow

1. Open n8n
2. Go to **Workflows**
3. Click **Import**
4. Paste the workflow JSON

---

## 💡 Example Use Cases

This automation can be used for:

📬 Personal AI email assistant
💼 Client communication automation
🤖 Customer auto-reply system
⚙️ Inbox productivity workflow
📊 VIP sender prioritization

---

## 📂 Project Architecture

```
Gmail Trigger (Unread Emails)
        ↓
Important Senders Config
        ↓
  Check Sender Match
        ↓
    Is Important?
       ↙         ↘
    YES             NO
     ↓               ↓
  Star Email     Mark as Read
     ↓
 Get Message
     ↓
Extract Email Content
     ↓
  Edit Fields
     ↓
 AI Agent (Gemini)
     ↓
Parse AI Output
     ↓
 Should Send?
    ↙       ↘
  YES        NO
   ↓          ↓
 Reply     Skip
via Gmail
   ↓
Remove Labels
(Archive Thread)
```

---

## 🔒 Notes

- The AI **never** uses placeholder text like `[Your Name]` or `[Company]`
- Replies are only sent when the AI confirms a reply is needed
- If the AI agent fails, the workflow **does not crash** — it skips the reply
- Unimportant emails are silently marked as read
- Replied threads are archived to keep the inbox clean

Avoid auto-replying to:

- Newsletters and marketing emails
- System alerts
- Unknown or suspicious senders

Add these to your filtering logic for better control.

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

- Sentiment detection
- Multi-language reply support
- CRM integrations
- Slack or Notion notifications
- Email analytics dashboards

---

⭐ **Feel free to fork the workflow and adapt it for your own AI automation projects.**
