# 🤖 AI Receptionist Agent for Dental Clinics

> A real-time voice conversational AI agent that automates appointment booking and recovers missed leads.

![n8n](https://img.shields.io/badge/n8n-Workflow-EA4B71?style=flat-square&logo=n8n&logoColor=white)
![Vapi](https://img.shields.io/badge/Vapi-Voice%20AI-4C6EF5?style=flat-square)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4-74AA9C?style=flat-square&logo=openai&logoColor=white)
![Python](https://img.shields.io/badge/Python-FastAPI-3776AB?style=flat-square&logo=python&logoColor=white)
![Airtable](https://img.shields.io/badge/Airtable-Database-FFBF00?style=flat-square&logo=airtable&logoColor=black)

---

## 📋 Table of Contents

- [Overview](#-overview)
- [How It Works](#-how-it-works)
- [Prerequisites](#️-prerequisites)
- [Airtable Base Structure](#airtable-base-structure)
- [Setup Instructions](#️-setup-instructions)
- [Node Reference](#-node-reference)
- [Security Notes](#-security-notes)
- [Possible Enhancements](#-possible-enhancements)
- [License](#-license)

---

## 🧩 Overview

This n8n workflow acts as the backend brain for a Vapi-powered AI voice agent. When a caller talks to the AI receptionist, Vapi sends structured data (transcripts, function calls, caller info) to this workflow via webhook. The workflow then:

- Checks real-time appointment availability in Airtable
- Books appointments and marks slots as taken
- Suggests alternative time slots if the requested one is unavailable
- Falls back to OpenAI function calling for more open-ended conversation handling
- Logs every call for analytics and record-keeping
- Sends a response back to Vapi so the AI can speak the result to the caller

---

## 🔁 How It Works

```
Vapi Call → Webhook → Process Data → Log Call
                            │
                            ▼
                 Is it a "checkAvailability"
                     or "bookAppointment"
                         request?
                    ┌───────┴────────┐
                   Yes               No
                    │                 │
        Check Airtable Slots   OpenAI Function Calling
                    │            (handles general
              Slot available?     conversation/logic)
              ┌─────┴─────┐              │
             Yes          No              │
              │            │              │
        Book Appointment  Get Alternative  │
        + Mark Slot Booked  Slots          │
              │            │              │
        Send Confirmation  Format & Send   │
           to Vapi         Alternatives    │
              │            │              │
              └─────┬──────┴──────────────┘
                     ▼
              Respond to Webhook
```

### Step-by-step

1. **Vapi Webhook** — Receives the incoming payload from Vapi (call transcript, function call name/parameters, caller phone number, call SID).
2. **Process Vapi Data** — Normalizes the payload into a consistent structure used by downstream nodes.
3. **Log Call to Airtable** — Records every incoming call (independent of outcome) for analytics.
4. **Is Check Availability?** — Routes the request based on whether Vapi's function call is `checkAvailability`/booking-related or something else.
5. **Check Airtable Availability** — Queries the `Available Slots` table for the requested date/time.
6. **Is Slot Available?** — Branches based on whether a matching open slot was found.
   - **If available:**
     - **Book Appointment in Airtable** — Adds a new row to the `Appointments` table.
     - **Mark Slot as Booked** — Updates the `Available Slots` table so the slot can't be double-booked.
     - **Send Confirmation to Vapi** — Sends a spoken confirmation message + structured result back to the call.
   - **If unavailable:**
     - **Get Alternative Slots** — Pulls other open slots from Airtable.
     - **Format Alternative Slots** — Builds a natural-language list of alternatives.
     - **Send Alternatives to Vapi** — Returns the alternatives so the AI can offer them to the caller.
7. **OpenAI Function Calling** — Handles calls that aren't a direct availability/booking check (general Q&A, service questions, etc.), using GPT-4 with defined `checkAvailability` and `bookAppointment` functions.
8. **Respond to Webhook** — Sends the final acknowledgment back to the triggering webhook call.

---

## 🛠️ Prerequisites

| Requirement | Purpose |
|---|---|
| **Vapi account + phone number** | Handles the actual voice call and speech-to-text/text-to-speech |
| **n8n instance** (self-hosted or cloud) | Runs this workflow |
| **Airtable base** | Stores appointments, available slots, and call logs |
| **OpenAI API key** | Powers conversational/function-calling logic |
| **FastAPI backend service** | Relays workflow responses back to Vapi (`/send-to-vapi` endpoint) |

### Airtable base structure

Create one base with these three tables:

**1. Available Slots**
| Field | Type |
|---|---|
| Appointment Date | Date |
| Appointment Time | Text/Time |
| Booked | Checkbox |
| Booked By | Text (phone number) |
| Booked At | Date/Time |

**2. Appointments**
| Field | Type |
|---|---|
| Patient Name | Text |
| Appointment Date | Date |
| Appointment Time | Text/Time |
| Duration | Number |
| Reason | Text |
| Phone Number | Text |
| Call SID | Text |

**3. Call Logs**
| Field | Type |
|---|---|
| Call SID | Text |
| Phone Number | Text |
| Transcript | Long text |
| Timestamp | Date/Time |

---

## ⚙️ Setup Instructions

1. **Import the workflow** into your n8n instance (`Workflows → Import from File`).
2. **Create your Airtable base** using the table structure above, then replace the placeholder `appXXXXXXXXXXXXXX` base ID in every Airtable node with your real base ID.
3. **Add credentials**:
   - Airtable API token (Personal Access Token recommended)
   - OpenAI API credential
   - HTTP Header Auth credential for your FastAPI service, if secured
4. **Update the FastAPI endpoint URL** (`http://your-fastapi-service:8000/send-to-vapi`) in the following nodes to point to your actual deployed service:
   - `Send Confirmation to Vapi`
   - `Send Alternatives to Vapi`
5. **Configure Vapi**:
   - Point your Vapi assistant's server/webhook URL to this workflow's webhook endpoint (`/webhook/vapi-webhook`).
   - Define the `checkAvailability` and `bookAppointment` functions in your Vapi assistant config to match the parameters expected by the `OpenAI Function Calling` node.
6. **Activate the workflow.**

> 💡 **Using Google Sheets instead of Airtable?** A disabled node — `Book Appointment in Google Sheets (Alternative)` — is included as a starting point. Enable it and disable the Airtable booking node if you'd rather use Sheets.

---

## 📋 Node Reference

| Node | Type | Role |
|---|---|---|
| Vapi Webhook | Webhook (trigger) | Entry point for all Vapi call data |
| Process Vapi Data | Code | Parses/normalizes incoming payload |
| Log Call to Airtable | Airtable | Logs every call for record-keeping |
| Is Check Availability? | IF | Routes booking vs. general conversation |
| Check Airtable Availability | Airtable | Looks up requested slot |
| Is Slot Available? | IF | Branches on availability result |
| Book Appointment in Airtable | Airtable | Creates the appointment record |
| Mark Slot as Booked | Airtable | Prevents double-booking |
| Send Confirmation to Vapi | HTTP Request | Sends success message back to the call |
| Get Alternative Slots | Airtable | Fetches open slots when requested one is taken |
| Format Alternative Slots | Code | Builds a spoken-friendly list of alternatives |
| Send Alternatives to Vapi | HTTP Request | Sends alternatives back to the call |
| OpenAI Function Calling | HTTP Request | GPT-4 handles non-booking conversation |
| Respond to Webhook | Respond to Webhook | Final response to the incoming webhook |

---

## 🔒 Security Notes

- Replace all placeholder IDs (`appXXXXXXXXXXXXXX`, Google Sheet ID, FastAPI URL) with real values before going live.
- Secure the webhook endpoint (e.g., a shared secret validated in the `Process Vapi Data` node) since it will be publicly reachable.
- Use environment-based credentials in n8n rather than hardcoding API keys in node parameters.

---

## 🚀 Possible Enhancements

- Add SMS confirmation (e.g., via Twilio) after successful booking.
- Add cancellation/rescheduling function calls.
- Add a daily digest report of bookings pulled from Call Logs.
- Add retry/error-handling branches around the Airtable and OpenAI HTTP nodes.

---

## 📄 License

Internal/private use — adapt as needed for your clinic or business.
