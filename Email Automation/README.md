# 🤖 Email Automation — Team of Email AI Agents

> A fully automated, AI-powered email management system that classifies incoming emails and routes them to specialized agents for client inquiries, support, meetings, and follow-ups.

![n8n](https://img.shields.io/badge/n8n-Workflow-EA4B71?style=flat-square&logo=n8n&logoColor=white)
![Gmail](https://img.shields.io/badge/Gmail-Trigger%20%26%20Drafts-EA4335?style=flat-square&logo=gmail&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT-74AA9C?style=flat-square&logo=openai&logoColor=white)
![Google Calendar](https://img.shields.io/badge/Google-Calendar-4285F4?style=flat-square&logo=googlecalendar&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-Agents-1C3C3C?style=flat-square)

---

## 📋 Table of Contents

- [Overview](#-overview)
- [What This Email AI Team Can Do](#-what-this-email-ai-team-can-do)
- [How It Works](#-how-it-works)
- [Meet the Agents](#-meet-the-agents)
- [Draft & Approval Process](#-draft--approval-process)
- [Running the Classifier Locally (Optional)](#-running-the-classifier-locally-optional)
- [Prerequisites](#️-prerequisites)
- [Getting Started](#-getting-started)
- [Node Reference](#-node-reference)
- [Security Notes](#-security-notes)
- [Possible Enhancements](#-possible-enhancements)
- [License](#-license)

---

## 🧩 Overview

This n8n workflow turns Gmail into a fully automated inbox triage and response system. A **Supervisor Classifier Agent** reads every incoming email, decides what kind of email it is, and hands it off to a specialized AI agent that drafts the right kind of reply — a client inquiry response, a support ticket reply, a meeting scheduling email, or a follow-up nudge. Every generated reply is saved as a **Gmail draft** for review (or can be configured to send automatically), so nothing goes out without the structure and tone your business wants.

---

## ✅ What This Email AI Team Can Do

- **Classify and Route Emails** — Automatically detects and categorizes emails into Client Inquiry, Support, Follow-Up, and Meeting Request buckets.
- **AI-Powered Email Responses** — Generates professional, personalized replies based on the context of each message.
- **Meeting Scheduling** — Suggests time slots and checks Google Calendar availability before responding.
- **Follow-Up Emails** — Makes sure no conversation is left hanging by nudging unanswered threads.
- **Support Ticket Management** — Handles complaints, troubleshooting requests, and general issue reports.
- **Works 24/7** — Never miss a lead, support request, or business opportunity, even outside office hours.

---

## 🔁 How It Works

```
Gmail Trigger (polls hourly)
        │
        ▼
Classifier Agent ──▶ decides category
        │
        ├── "Support"          ──▶ Gmail (label) ──▶ Support Agent      ──▶ Draft2
        ├── "Meeting Request"  ──▶ Gmail3 (label) ──▶ Meeting Agent     ──▶ Draft
        ├── "Client Inquiry"   ──▶ Gmail (label)  ──▶ Inquiry Agent     ──▶ Draft1
        └── "Follow Up Email"  ──▶ Gmail4 (label) ──▶ Follow Up Agent   ──▶ Draft4
```

Each specialized agent uses its own OpenAI chat model and a **Structured Output Parser** that forces the reply into a consistent `{ "subject": ..., "email_body": ... }` JSON shape, which is then used to create a Gmail draft addressed back to the original sender.

### Step-by-step

1. **Gmail Trigger** — Polls the inbox every hour and downloads any attachments on new messages.
2. **Classifier Agent** — Reads the email body and outputs exactly one category: `Client Inquiry`, `Support`, `Meeting Request`, `Follow-Up Email`, or `Introduction Email`.
3. **Category Filters** (`Support`, `Meeting Request`, `Client Inquiry`, `Follow Up Email`) — Each filter checks the classifier's output and only passes matching emails through to its branch. All four filters run off the same classification, so only the matching branch continues.
4. **Label Nodes** (`Gmail`, `Gmail1`, `Gmail3`, `Gmail4`) — Apply a Gmail label to the message so it's easy to see how it was categorized at a glance in your inbox.
5. **Specialized Agent** — Generates a structured JSON reply tailored to that category (see [Meet the Agents](#-meet-the-agents) below).
6. **Draft Node** (`Draft`, `Draft1`, `Draft2`, `Draft4`) — Creates a Gmail draft reply to the original sender using the agent's generated subject and body.

---

## 🧑‍💼 Meet the Agents

### 1. Supervisor Classifier Agent
Analyzes the incoming email and assigns it to exactly one category, outputting only the category name — no extra commentary — so downstream filters can route reliably.

### 2. Support Agent
Handles customer complaints, technical issues, and refund requests. Replies on behalf of a named support agent/company, follows guidelines for troubleshooting steps, case reference numbers, and empathetic tone, and always returns a structured JSON draft.

### 3. Meeting Agent
Schedules and manages meeting requests. Has access to a **Get Events** Google Calendar tool to check real availability, defaults to 60-minute meetings when no duration is specified, and can suggest new slots for rescheduling requests.

### 4. Client Inquiry Agent
Engages prospective or existing clients — answering pricing questions, service availability questions, and demo requests — with structured, on-brand replies.

### 5. Follow-Up Agent
Sends polite re-engagement emails for proposals, meeting requests, support tickets, or cold outreach that haven't received a response, without being pushy.

---

## 📝 Draft & Approval Process

Every agent's output is parsed through a **Structured Output Parser** to guarantee a clean `subject` / `email_body` JSON object. That object is then used to create a **Gmail draft** addressed to the original sender — giving you (or your team) a chance to review and edit before sending, or to configure auto-send later once you trust the output quality.

---

## 🧠 Running the Classifier Locally (Optional)

The Supervisor Classifier Agent is the most frequently-called node in this workflow, so it's the best candidate for cost optimization. You can swap its OpenAI chat model for a **locally-hosted DeepSeek R1 or Llama model** to:

- Reduce OpenAI API costs over time
- Keep classification logic and email content within your own environment for privacy
- Run the workflow fully offline or in a hybrid setup

This requires wiring a local LLM node (e.g., via Ollama or a self-hosted LangChain-compatible endpoint) in place of the classifier's `OpenAI Chat Model` node.

---

## 🛠️ Prerequisites

| Requirement | Purpose |
|---|---|
| **n8n instance** (self-hosted or cloud) | Runs this workflow |
| **Gmail account** with API access | Trigger on new mail, apply labels, create drafts |
| **Google Calendar access** | Availability checks for the Meeting Agent |
| **OpenAI API key** | Powers the classifier and all four response agents |
| *(Optional)* Local DeepSeek R1 / Llama setup | Cost-saving, privacy-preserving classifier alternative |

---

## 🚀 Getting Started

1. **Log in or sign up** for n8n.
2. **Download** the Team of Email AI Agents template.
3. **Import** the workflow into your n8n instance.
4. **Connect your credentials**: Gmail, Google Calendar, and OpenAI API.
5. **Customize agent responses** — adjust each agent's system prompt (tone, company name, signature, policies) to match your business.
6. *(Optional)* **Integrate DeepSeek R1 / Llama** in the classifier node to run locally and minimize OpenAI API calls.
7. **Test, refine, and activate** the workflow.

---

## 📋 Node Reference

| Node | Type | Role |
|---|---|---|
| Gmail Trigger | Gmail Trigger | Polls inbox hourly, downloads attachments |
| Classifier Agent | LangChain Agent | Categorizes each incoming email |
| Support / Meeting Request / Client Inquiry / Follow Up Email | Filter | Routes email to the matching branch based on classification |
| Gmail / Gmail1 / Gmail3 / Gmail4 | Gmail | Applies a category label to the message |
| Support Agent | LangChain Agent | Drafts support/complaint replies |
| Meeting Agent | LangChain Agent | Drafts meeting scheduling replies |
| Inquiry Agent | LangChain Agent | Drafts client inquiry replies |
| Follow Up Agent | LangChain Agent | Drafts follow-up/re-engagement replies |
| Get Events | Google Calendar Tool | Checks calendar availability for the Meeting Agent |
| Structured Output Parser (x4) | Output Parser | Forces each agent's reply into `{subject, email}` JSON |
| Draft / Draft1 / Draft2 / Draft4 | Gmail | Creates the Gmail draft reply to the sender |
| OpenAI Chat Model (x6) | LLM | Language model backing each agent |

---

## 🔒 Security Notes

- Replace the placeholder calendar (`roberto@lunivate.com`) and Gmail label IDs with your own before activating.
- Store OpenAI and Google credentials in n8n's credential manager — never hardcode API keys.
- Review drafts before enabling auto-send, especially for support/refund responses that reference policy.
- If running a local LLM for classification, ensure the hosting environment still has appropriate access controls over email content.

---

## 🚀 Possible Enhancements

- Auto-send low-risk replies (e.g., simple inquiries) while keeping support/refund responses in draft-only mode.
- Add a fifth branch for "Introduction Email" (already a valid classifier output but currently unrouted).
- Log every classified email and its category to a spreadsheet or database for reporting.
- Add sentiment analysis to prioritize urgent or negative support emails.
- Add Slack/Teams notifications when a new draft is created for team visibility.

---

## 📄 License

Internal/private use — adapt as needed for your business.
