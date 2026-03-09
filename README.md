🤖 AI Gmail Auto-Reply Agent (n8n)

An AI-powered Gmail automation workflow built with n8n that reads incoming emails, detects important senders, and automatically generates professional replies using Google Gemini AI.

This project shows how to build a simple AI email assistant for personal or business automation.

🚀 Features

✅ Monitor Gmail for new emails
⭐ Detect important senders (VIP list)
🤖 Generate replies using AI (Google Gemini)
📤 Automatically send email responses
📬 Organize inbox by marking or archiving threads

⚙️ How It Works

Gmail Trigger watches your inbox for unread emails.

The workflow checks if the sender is in your VIP sender list.

If the sender is important:

The email is starred

The message content is extracted

AI generates a professional reply

The reply is automatically sent

If the sender is not important:

The email is marked as read.

🧠 AI Response Format

The AI agent generates structured output:

SUBJECT: Reply subject

EMAIL BODY:
Full reply message

ACTION:
Reply / Ask for clarification / No response needed

This output is parsed and sent as the final Gmail response.

🛠 Requirements

n8n

Gmail API credentials

Google Gemini API key

🔧 Setup
1️⃣ Install n8n

Example using Docker:

docker run -it --rm \
-p 5678:5678 \
-v ~/.n8n:/home/node/.n8n \
n8nio/n8n
2️⃣ Connect Gmail

Create Gmail OAuth credentials and connect them inside n8n.

3️⃣ Add Gemini API

Create an API key from:

https://aistudio.google.com/app/apikey

Add it to the Google Gemini node in n8n.

4️⃣ Configure Important Senders

Edit the Important Senders Config node:

const importantSenders = [
  "vip@example.com",
  "client@example.com"
];

Replace with your real emails.

📂 Import the Workflow

Open n8n

Go to Workflows

Click Import

Paste the workflow JSON

💡 Example Use Cases

Personal AI email assistant

Customer auto-reply system

Client communication automation

Inbox productivity workflow

🔒 Notes

Avoid auto-replying to:

newsletters

marketing emails

unknown senders

Consider adding filters or labels for better control.

📜 License

MIT License

💡 Inspiration

This project is designed as a foundation for AI automation agencies building:

AI email assistants

business workflow automation

AI productivity systems






You can also copy and paste the workflow to try it. :)
