# 🤖 WhatsApp AI Appointment Booking Agent

> A conversational AI agent on WhatsApp that qualifies leads, books appointments to Google Calendar, and logs everything to Google Sheets — with voice message support.

![n8n](https://img.shields.io/badge/n8n-Workflow-EA4B71?style=flat-square&logo=n8n&logoColor=white)
![WhatsApp](https://img.shields.io/badge/WhatsApp-Business%20API-25D366?style=flat-square&logo=whatsapp&logoColor=white)
![Gemini](https://img.shields.io/badge/Google-Gemini-4285F4?style=flat-square&logo=google&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-Whisper-74AA9C?style=flat-square&logo=openai&logoColor=white)
![Google Calendar](https://img.shields.io/badge/Google-Calendar-4285F4?style=flat-square&logo=googlecalendar&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google-Sheets-34A853?style=flat-square&logo=googlesheets&logoColor=white)
![Gmail](https://img.shields.io/badge/Gmail-Confirmation-EA4335?style=flat-square&logo=gmail&logoColor=white)

---

## 📋 Table of Contents

- [Overview](#-overview)
- [How It Works](#-how-it-works)
- [Booking Conversation Logic](#-booking-conversation-logic)
- [Prerequisites](#️-prerequisites)
- [Google Sheet Structure](#google-sheet-structure)
- [Setup Instructions](#️-setup-instructions)
- [Node Reference](#-node-reference)
- [Security Notes](#-security-notes)
- [Possible Enhancements](#-possible-enhancements)
- [License](#-license)

---

## 🧩 Overview

This n8n workflow turns a WhatsApp Business number into an AI-powered booking assistant. Customers message the number — by **text or voice note** — and a LangChain agent (Google Gemini) walks them through a structured intake flow, checks real availability on Google Calendar, books the appointment, records everything in a Google Sheet, and emails a confirmation.

Key capabilities:

- Accepts both **text and voice messages** (voice notes are transcribed via OpenAI Whisper and cleaned up before reaching the agent)
- Collects contact details **in a strict order** (email → name → phone → location/time zone → topic)
- Uses the email address as a **unique key** to create-or-update a single row per contact in Google Sheets
- Handles **time zone conversion** transparently — always enforces Pakistan-time office hours behind the scenes, while only ever showing the customer times in their own time zone
- Checks the calendar for real conflicts and only offers genuinely open 60-minute slots, at least 24 hours out
- Creates the calendar event and sends a confirmation email once the customer picks a time
- Maintains **conversation memory per user** (windowed to the last 20 messages) so the flow feels like a natural back-and-forth

---

## 🔁 How It Works

```
WhatsApp Trigger
      │
      ▼
    Switch (message type?)
    ┌────┴─────┐
  audio        text
    │            │
Download Media   │
    │            │
Whisper           │
Transcription     │
    │            │
Clean Up          │
Transcript        │
(LLM agent)       │
    │            │
    └─────┬──────┘
          ▼
   Edit Fields (normalize
     message payload)
          │
          ▼
       AI Agent  ──uses──▶ Gemini (LLM)
          │        ├──▶ Simple Memory (per-user, 20-msg window)
          │        ├──▶ Google Calendar (read / create / delete)
          │        ├──▶ Google Sheets (read / add row / update row)
          │        └──▶ Gmail (send confirmation)
          ▼
     Send Message (WhatsApp reply)
```

There is also a lightweight secondary path (`Webhook → Message a model → Send message1`) using a separate `gpt-4o-mini` chat model — a simpler generic Q&A responder that can be wired to a different external trigger, independent of the main WhatsApp booking flow.

### Step-by-step (main flow)

1. **WhatsApp Trigger** — Listens for incoming WhatsApp messages.
2. **Switch** — Branches on `messages[0].type`: `audio` → transcription path, anything else → straight to the agent.
3. **Download Media** *(audio only)* — Fetches the voice note file from WhatsApp's media URL.
4. **HTTP Request (Whisper)** *(audio only)* — Sends the audio to OpenAI's `whisper-1` transcription endpoint.
5. **Transcribe a Recording** *(audio only)* — A small LLM pass that corrects grammar/punctuation and strips filler words from the raw transcript.
6. **Edit Fields** *(text only)* — Normalizes the incoming `messages` array so both paths feed the agent in the same shape.
7. **AI Agent** — The core LangChain agent. Runs the full booking conversation (see logic below) using Gemini as the LLM, windowed memory keyed by the sender's WhatsApp ID, and six tools (calendar read/create/delete, sheet read/add/update, Gmail send).
8. **Send Message** — Replies to the customer on WhatsApp with the agent's output.

---

## 💬 Booking Conversation Logic

The agent's system prompt encodes a strict, ordered intake script:

1. Ask: *"Would you like to book an appointment?"*
2. If yes, collect information **one field at a time**, waiting for a reply between each:
   - **Email** (used as the unique row key — checks Google Sheets immediately; updates existing row or creates a new one)
   - **Full name**
   - **Phone number**
   - **Location / time zone**
   - **Appointment topic** (what they'd like to discuss)
   - Each answer is written back to the Google Sheet row immediately.
3. Offer the **next 5 available 60-minute slots**:
   - Office hours are Mon–Fri, 09:00–12:00 and 13:00–17:00, **Pakistan time**, with no midday slots.
   - Slots must not conflict with existing calendar events and must start at least 24 hours out.
   - Slots are converted to and displayed only in the **customer's** time zone — Pakistan time and any offset math are never shown to the user.
4. On confirmation:
   - Creates a 60-minute Google Calendar event (default duration, unless the user specifies otherwise).
   - Updates (never duplicates) the customer's existing Sheet row with the confirmed date/time, stored in **Pakistan time**.
   - Sends a confirmation email via Gmail with the appointment time (in the customer's time zone), their name, and the discussion topic.

---

## 🛠️ Prerequisites

| Requirement | Purpose |
|---|---|
| **WhatsApp Business API access** (via Meta/Cloud API) | Receive and send WhatsApp messages |
| **n8n instance** (self-hosted or cloud) | Runs this workflow |
| **Google account** with Calendar, Sheets, and Gmail access | Scheduling, data storage, and confirmations |
| **OpenAI API key** | Whisper transcription of voice notes |
| **Google Gemini API key** | Powers the conversational agent |

### Google Sheet Structure

Create a sheet with these columns (matched/keyed by **Email**):

| Column | Purpose |
|---|---|
| Email | Unique identifier for each contact |
| Name | Full name |
| Phone | Phone number |
| Location | Location / time zone |
| Topic | What the appointment is about |
| Appointment Date | Confirmed date & time (stored in Pakistan time) |

---

## ⚙️ Setup Instructions

1. **Import the workflow** into n8n (`Workflows → Import from File`).
2. **Connect credentials**:
   - WhatsApp Business API (Trigger + Send nodes)
   - OpenAI API (for Whisper transcription and the secondary chat model)
   - Google Gemini API (main agent's LLM)
   - Google Calendar OAuth2
   - Google Sheets OAuth2
   - Gmail OAuth2
3. **Replace placeholders**:
   - Swap `your-calendar@gmail.com` in the three Calendar nodes (`Calendar read`, `Calendar create`, `Calendar delete`) for your real calendar.
   - Swap `YOUR_GOOGLE_SHEET_ID` in the three Sheets nodes for your actual spreadsheet ID.
4. **Review the AI Agent's system prompt** and adjust:
   - Office hours / working days
   - Time zone (currently hardcoded to Pakistan time as the business's local zone)
   - Appointment duration defaults
   - Tone/wording of the scripted questions
5. **Activate the workflow** and message your WhatsApp number to test both a text and a voice-note booking flow.

> 💡 The secondary `Webhook → Message a model → Send message1` branch is a separate, simpler responder — wire it to its own trigger/webhook if you plan to use it, or remove it if it's not needed.

---

## 📋 Node Reference

| Node | Type | Role |
|---|---|---|
| WhatsApp Trigger | WhatsApp Trigger | Entry point for incoming messages |
| Switch | Switch | Routes audio vs. text messages |
| Download Media | HTTP Request | Downloads voice note file |
| HTTP Request | HTTP Request | Sends audio to OpenAI Whisper for transcription |
| Transcribe a Recording | LangChain Agent | Cleans up raw transcript (grammar, filler words) |
| Edit Fields | Set | Normalizes text-message payload shape |
| AI Agent | LangChain Agent | Core conversational booking agent |
| Google Gemini Chat Model | LLM (Gemini) | Language model powering the AI Agent |
| Simple Memory | Buffer Window Memory | Per-user conversation memory (last 20 messages) |
| Calendar read | Google Calendar Tool | Reads existing events to check availability |
| Calendar create | Google Calendar Tool | Creates the confirmed appointment event |
| Calendar delete | Google Calendar Tool | Deletes/cancels an event when needed |
| Get row(s) in sheet in Google Sheets | Google Sheets Tool | Looks up a contact by email |
| Google Sheets add rows | Google Sheets Tool | Creates a new contact row |
| Google Sheets update rows | Google Sheets Tool | Updates an existing contact row (matched on Email) |
| Send a message in Gmail | Gmail Tool | Sends the booking confirmation email |
| Send Message | WhatsApp | Replies to the customer on WhatsApp |
| Webhook / Message a model / Send message1 | Webhook, LangChain Agent, WhatsApp | Independent, simpler secondary responder path |

---

## 🔒 Security Notes

- Replace all placeholder IDs (`your-calendar@gmail.com`, `YOUR_GOOGLE_SHEET_ID`) before going live.
- Store all API keys and OAuth credentials in n8n's credential manager — never hardcode them in node parameters.
- WhatsApp webhook verification tokens should be kept secret and rotated periodically.
- Consider rate-limiting or validating inbound webhook traffic if exposing this publicly.

---

## 🚀 Possible Enhancements

- Add appointment cancellation/rescheduling flows callable by the customer directly in chat.
- Add SMS or WhatsApp reminders 24 hours before the appointment.
- Log every conversation turn to a separate analytics sheet or database.
- Add multi-language support for the scripted intake questions.
- Add retry/error handling around the Whisper transcription and Google API calls.

---

## 📄 License

Internal/private use — adapt as needed for your business.
