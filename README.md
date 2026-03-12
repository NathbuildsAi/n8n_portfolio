🤖 AI Gmail Email Classifier (n8n + Gemini)

An AI-powered Gmail automation workflow built with n8n that reads incoming emails, analyzes their content using Google Gemini AI, and automatically categorizes them with Gmail labels.

This project demonstrates how to build an AI-powered inbox classification system using workflow automation and large language models.

🚀 Features

✅ Monitor Gmail for new unread emails
📥 Extract email sender, subject, and message body
🤖 Classify emails using Google Gemini AI
🏷 Automatically apply Gmail labels
📊 Generate structured AI output including:

Label classification

Priority level

Email type

AI confidence score

⚙️ How It Works

The workflow processes emails in the following steps:

1️⃣ Gmail Trigger

The workflow watches your inbox for new unread emails.

2️⃣ Read Email Thread

The Gmail node retrieves the full email thread data.

3️⃣ Extract Email Content

A JavaScript node extracts:

Sender

Subject

Email body

The body is decoded from Gmail's Base64 format.

4️⃣ AI Email Classifier

The extracted email content is sent to Google Gemini AI, which analyzes the message and returns structured classification data.

Example AI output:

{
  "labels": ["CATEGORY_UPDATES"],
  "priority": "medium",
  "type": "update",
  "confidence": 0.92
}
5️⃣ Parse AI Output

The workflow safely parses the AI response and ensures valid JSON output.

If parsing fails, a default classification is applied.

6️⃣ Apply Gmail Label

The selected label is automatically added to the email inside Gmail.

🧠 AI Classification Format

The AI agent returns structured JSON:

{
  "labels": [],
  "priority": "low | medium | high",
  "type": "lead | update | social | promotion | personal | spam",
  "confidence": 0.0
}
Fields Explained

labels

Gmail labels applied to the email.

Available labels:

CATEGORY_PERSONAL

CATEGORY_SOCIAL

CATEGORY_PROMOTIONS

CATEGORY_UPDATES

CATEGORY_FORUMS

SPAM

priority

Indicates how important the email is.

Level	Meaning
low	newsletters, promotions, automated messages
medium	normal emails
high	urgent or time-sensitive emails

type

The general email category.

Type	Example
lead	business inquiry
update	receipts or confirmations
social	social network notifications
promotion	marketing emails
personal	messages from real people
spam	suspicious or irrelevant emails

confidence

AI confidence score between 0 and 1.

Range	Meaning
0.9 – 1.0	very confident
0.7 – 0.89	fairly confident
0.5 – 0.69	uncertain
below 0.5	low confidence
🛠 Requirements

To run this workflow you need:

n8n

Gmail OAuth credentials

Google Gemini API key

🔧 Setup
1️⃣ Install n8n

Example using Docker:

docker run -it --rm \
-p 5678:5678 \
-v ~/.n8n:/home/node/.n8n \
n8nio/n8n
2️⃣ Connect Gmail

Create Gmail OAuth credentials and connect them in n8n.

Google Cloud Console:

https://console.cloud.google.com/

3️⃣ Add Gemini API

Create an API key:

https://aistudio.google.com/app/apikey

Add the key to the Google Gemini node in n8n.

4️⃣ Import the Workflow

Open n8n

Go to Workflows

Click Import

Paste the workflow JSON

💡 Example Use Cases

This automation can be used for:

📬 AI inbox organization
📊 Email prioritization systems
📈 Business lead detection
🤖 AI productivity tools
⚙️ Gmail workflow automation

📂 Project Architecture
Gmail Trigger
      ↓
Read Email Thread
      ↓
Extract Email Content
      ↓
AI Email Classifier (Gemini)
      ↓
Parse AI Output
      ↓
Apply Gmail Label
🔒 Notes

Avoid automatically labeling:

system alerts

sensitive emails

financial notifications

Additional filters can be added to improve classification accuracy.

📜 License

MIT License

💡 Inspiration

This project demonstrates how AI and workflow automation can be combined to build:

AI email assistants

automated inbox management

business workflow automation

AI productivity systems

You can expand this project by adding:

auto-reply agents

lead detection

CRM integrations

Slack or Notion notifications.
You can copy and paste this workflow.
