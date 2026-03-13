🤖 AI Smart Email Auto-Responder (Gemini + n8n)

An AI-powered Gmail automation workflow built with n8n that automatically analyzes incoming emails, decides whether a reply is needed, and generates a professional response using Google Gemini AI.

The system performs AI email triage, sentiment detection, safe reply generation, and manual review handling.

This project demonstrates how to build a production-style AI automation workflow for email management.

🚀 Features

📥 Gmail Monitoring
Automatically watches your inbox for new unread emails.

🧠 AI Email Triage
Gemini AI analyzes each email and decides:

Should the email receive a reply?

What type of email it is

What tone should be used

The sender’s sentiment

AI confidence level

😊 Sentiment Detection

The AI detects the emotional tone of the email:

positive

neutral

negative

urgent

Replies automatically adjust tone based on sentiment.

✉️ AI Reply Generation

If the email qualifies for a reply:

A concise response is generated

Tone is adapted to the detected sentiment

Replies stay under 120 words

⚠️ Safety Rules

The AI reply system is constrained to prevent risky responses.

Rules include:

No promises or commitments

No placeholders like [Your Name]

No subject lines

Professional tone

🛑 Spam & Automation Detection

The triage AI automatically ignores:

newsletters

marketing emails

noreply senders

automated systems

spam

👤 Manual Review System

If something fails (AI error or reply generation failure):

The email is labeled:

"Needs Manual Review"

This ensures no messages are lost.

⚙️ Workflow Architecture
New Email (Gmail Trigger)
        ↓
AI Agent – Email Triage (Gemini)
        ↓
Parse Decision (Code Node)
        ↓
Should Reply? (confidence + spam filter)
        ↓
        ├─ YES → AI Agent – Write Reply
        │           ↓
        │     Reply Succeeded?
        │           ↓
        │     Send Reply via Gmail
        │
        └─ NO → Check Manual Review?
                    ↓
              Label "Needs Manual Review"
                    ↓
                Skip
🧠 AI Decision Output

The triage AI returns structured JSON:

{
 "should_reply": true,
 "type": "customer_inquiry",
 "tone": "professional",
 "sentiment": "neutral",
 "confidence": 0.92,
 "reason": "Customer asking about product availability"
}

This structured output allows the workflow to make safe automation decisions.

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

Then open:

http://localhost:5678
2️⃣ Connect Gmail

Create Gmail OAuth credentials and connect them inside n8n.

Replace the credential placeholders in the workflow with your Gmail connection.

3️⃣ Add Google Gemini API

Create an API key:

https://aistudio.google.com/app/apikey

Add it to the Gemini Chat Model nodes.

4️⃣ Create Manual Review Label

Inside Gmail create a label called:

Needs Manual Review

Then replace the placeholder inside the workflow:

REPLACE_WITH_LABEL_ID
📂 Import the Workflow

1️⃣ Open n8n

2️⃣ Go to Workflows

3️⃣ Click Import

4️⃣ Paste the workflow JSON file

5️⃣ Connect your credentials

💡 Example Use Cases

This workflow can be adapted for:

Personal AI email assistants

Customer support automation

Lead response automation

Inbox productivity tools

Small business AI reception systems

🔒 Safety Considerations

Avoid fully automating replies for:

legal emails

billing issues

sensitive topics

contract discussions

Use the Manual Review label as a safety fallback.

📜 License

MIT License

💡 Inspiration

This project demonstrates how to combine:

AI Agents

Email automation

structured AI outputs

safe workflow design

to build real-world AI automation systems.

It can serve as a starting point for building:

AI customer support agents

AI productivity assistants

AI business automation systems

⭐ Feel free to fork the workflow and adapt it for your own AI automation projects.
