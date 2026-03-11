🤖 AI Gmail Email Classifier (n8n + Gemini)

An AI-powered Gmail automation workflow built with n8n that reads incoming emails, analyzes them using Google Gemini AI, and automatically classifies them into Gmail categories.

The workflow uses AI to understand the sender, subject, and message content, then applies the correct Gmail label while generating structured metadata such as priority, type, and confidence score.

This project demonstrates how to build an AI-powered inbox organization system using workflow automation.

🚀 Features

✅ Monitor Gmail for new unread emails

🧠 Analyze emails using Google Gemini AI

🏷 Automatically apply Gmail category labels

⚡ Detect email priority level

📬 Classify email type (lead, update, social, promotion, etc.)

📊 Generate confidence score for the AI classification

🛡 Safe JSON parsing to prevent workflow errors

⚙️ How It Works
1️⃣ Gmail Trigger

The workflow monitors Gmail for new unread emails.

2️⃣ Read Full Email

The full email thread is retrieved so the AI can analyze:

Sender

Subject

Email body

3️⃣ AI Email Classification

Google Gemini AI analyzes the email and determines:

Gmail label

Email priority

Email type

AI confidence score

Supported Gmail labels:

CATEGORY_PERSONAL

CATEGORY_SOCIAL

CATEGORY_PROMOTIONS

CATEGORY_UPDATES

CATEGORY_FORUMS

SPAM

4️⃣ JSON Response Parsing

The AI returns structured JSON which is safely parsed by a JavaScript node to prevent automation failures.

5️⃣ Apply Gmail Labels

The workflow automatically applies the returned Gmail label to the email.

Your inbox becomes automatically organized.

🧠 AI Response Format

The AI agent returns structured JSON:

{
 "labels": ["CATEGORY_UPDATES"],
 "priority": "medium",
 "type": "update",
 "confidence": 0.92
}
Field Definitions

labels
Gmail category applied to the email.

priority

low → newsletters, promotions, automated notifications
medium → normal communication
high → urgent or time-sensitive emails

type

lead → business inquiry
update → receipt or confirmation
social → social media notification
promotion → marketing email
personal → message from a real person
spam → suspicious or irrelevant message

confidence

A value between 0 and 1 representing how confident the AI is in the classification.

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

Create an API key:

https://aistudio.google.com/app/apikey

Add the key to the Google Gemini Chat Model node in the workflow.

4️⃣ Create Gmail Labels

Ensure the following labels exist in your Gmail account:

CATEGORY_PERSONAL
CATEGORY_SOCIAL
CATEGORY_PROMOTIONS
CATEGORY_UPDATES
CATEGORY_FORUMS
SPAM

📂 Import the Workflow

Open n8n

Go to Workflows

Click Import

Paste the workflow JSON

Save and activate the workflow

💡 Example Use Cases

📥 Automatic inbox organization

🤖 AI-powered email classification

📬 Smart Gmail labeling system

⚡ Productivity automation for email workflows

🧠 AI inbox intelligence

🔒 Notes

To avoid incorrect labeling, consider adding additional filters for:

sensitive emails

financial notifications

unknown senders

You can also extend the workflow with manual review steps for low-confidence classifications.

📜 License

MIT License

💡 Inspiration

This project demonstrates how AI can be used to build:

AI email assistants

AI inbox organization systems

automation workflows with machine learning

It can also serve as a foundation for building:

AI lead detection systems

AI CRM automations

AI customer support workflows

Feel free to copy the workflow and adapt it for your own automation projects.
