# 🎯 Lead Generation using Apollo (Apify)

> Effortlessly generate targeted leads by combining Apollo.io, Apify, Airtable, and n8n into one streamlined scraping-and-storage workflow.

![n8n](https://img.shields.io/badge/n8n-Workflow-EA4B71?style=flat-square&logo=n8n&logoColor=white)
![Apify](https://img.shields.io/badge/Apify-Apollo%20Scraper-00B899?style=flat-square)
![Apollo.io](https://img.shields.io/badge/Apollo.io-Lead%20Source-7C3AED?style=flat-square)
![Airtable](https://img.shields.io/badge/Airtable-Database-FFBF00?style=flat-square&logo=airtable&logoColor=black)

---

## 📋 Table of Contents

- [Overview](#-overview)
- [What This Automation Does](#-what-this-automation-does)
- [How It Works](#-how-it-works)
- [Airtable Base Structure](#airtable-base-structure)
- [Prerequisites](#️-prerequisites)
- [Setup Instructions](#️-setup-instructions)
- [Daily Scraping & Pagination](#-daily-scraping--pagination)
- [Node Reference](#-node-reference)
- [⚠️ Security Notes](#️-security-notes)
- [Possible Enhancements](#-possible-enhancements)
- [License](#-license)

---

## 🧩 Overview

This n8n workflow triggers an **Apollo.io people-search scrape** through an **Apify actor**, pulls the resulting dataset, extracts only the fields you need, filters out leads you've already captured, and appends every new, unique lead to an **Airtable** base — ready for outreach or hand-off to a sales sequencing tool.

---

## ✅ What This Automation Does

- **Initiates Lead Scraping** — Triggers an Apollo.io scraper via the Apify API to gather leads matching a saved Apollo search (job title, industry, company size, location, and more).
- **Extracts & Filters Contact Details** — Captures name, title, email, LinkedIn URL, company name, company website, industry, Crunchbase URL, and location.
- **Eliminates Duplicate Leads** — Deduplicates by email address so the same contact is never added twice, even across separate runs.
- **Stores Data in Airtable** — Sends structured, labeled lead records straight into an Airtable base for easy follow-up and enrichment tracking.
- **Automates Scheduling & Pagination** — Designed to process up to 1,000 leads/day (25 leads per page × 40 pages), with Airtable used to track the last page scraped so each day picks up where the previous one left off.

---

## 🔁 How It Works

```
When clicking 'Test workflow'  (manual trigger)
              │
              ▼
     Request Apollo Scraper  ──▶  Apify actor run (Apollo.io people search)
              │
              ▼
         Get Request  ──▶  Pulls the resulting dataset from Apify
              │
              ▼
    Filter Needed Fields  ──▶  Keeps only the contact/company fields you need
              │
              ▼
     Remove Duplicates  ──▶  Dedupes leads by email across runs
              │
              ▼
          Airtable  ──▶  Creates one record per new, unique lead
```

### Step-by-step

1. **When clicking 'Test workflow'** — Manual trigger for testing; replace with a Schedule Trigger for daily automated runs.
2. **Request Apollo Scraper** — Calls the Apify actor endpoint (`POST /v2/acts/{actorId}/runs`) with a saved Apollo.io search URL and session cookies, kicking off the scrape (configured here for up to 1,000 results per run).
3. **Get Request** — Fetches the completed dataset from Apify (`GET /v2/datasets/{datasetId}/items`) once the actor run finishes.
4. **Filter Needed Fields** — A Set node that maps only the relevant fields out of the raw Apollo record: first name, full name, title, email, LinkedIn URL, organization name/website/industry/LinkedIn/Crunchbase URL, employment start date, and a combined `Location` (city, state, country).
5. **Remove Duplicates** — Uses n8n's built-in dedupe operation, keyed on email address, to skip any lead already seen in a previous execution (keeps a history of up to 10,000 emails).
6. **Airtable** — Creates a new record in the `Lead` table with all filtered fields, plus a default `Lead Status` of *"Lead Yet to be Enriched"* and `Start Lead Enrichment` set to `false`, so leads are clearly marked as fresh and ready for your next enrichment step.

---

## 🛠️ Prerequisites

| Requirement | Purpose |
|---|---|
| **Apify account + API token** | Runs the Apollo.io scraping actor and retrieves the resulting dataset |
| **Apollo.io account** | Source of the people-search results (session cookies are used to authenticate the scrape) |
| **Airtable base** | Stores the generated leads |
| **n8n instance** (self-hosted or cloud) | Runs this workflow |

### Airtable Base Structure

The workflow writes to a `Lead` table. At minimum, it populates these columns:

| Field | Type | Source |
|---|---|---|
| First Name | Text | Apollo |
| Full Name | Text | Apollo |
| Title | Text | Apollo |
| Email Address | Text | Apollo |
| LinkedIn URL | Text | Apollo |
| Company Name | Text | Apollo (organization) |
| Company Website | Text | Apollo (organization) |
| LinkedIn Organization URL | Text | Apollo (organization) |
| Crunchbase URL | Text | Apollo (organization) |
| Industry | Text | Apollo (organization) |
| Location | Text | Apollo (organization city/state/country) |
| Time At Company | Text | Apollo (employment history start date) |
| Lead Status | Single select | Set to `"Lead Yet to be Enriched"` by default |
| Start Lead Enrichment | Checkbox | Set to `false` by default |

The existing base also has additional columns (Meeting Date, Summary, Personalization, Instantly Campaign ID, etc.) designed to support a later enrichment/outreach stage beyond this workflow's scope.

---

## ⚙️ Setup Instructions

1. **Import the workflow** into your n8n instance.
2. **Create or connect an Apify account** and set up (or copy) an Apollo.io scraping actor.
3. **Add your Apify API token**:
   - Replace the placeholder `< Insert your API key>` in both **Request Apollo Scraper** and **Get Request** with your real Apify token, ideally using an n8n credential (Header Auth) instead of a hardcoded value.
4. **Set your Apollo.io search parameters**:
   - The `searchUrl` and `cookie` values in **Request Apollo Scraper** define which Apollo.io people search gets scraped (titles, industries, company size, location, exclusions, etc.). Regenerate these from your own logged-in Apollo.io session — see the security note below.
5. **Connect Airtable**:
   - Point the **Airtable** node at your own base and `Lead` table, and confirm your field names match (or update the column mapping).
6. **Swap the manual trigger** for a **Schedule Trigger** (e.g., daily) once you're ready to automate.
7. **Activate the workflow.**

---

## 📅 Daily Scraping & Pagination

Apollo scrapes are capped at **25 leads per page**. To reach **1,000 leads/day**, this workflow is designed to run through **40 pages per day**. To avoid re-scraping the same leads:

- Use an **Airtable field (or separate tracking table)** to store the last page number scraped.
- Optionally add a small **Code/JSON node** before **Request Apollo Scraper** to automatically read that last page from Airtable and inject it into the `startPage` parameter of the actor request — enabling fully hands-off daily pagination.

---

## 📋 Node Reference

| Node | Type | Role |
|---|---|---|
| When clicking 'Test workflow' | Manual Trigger | Starts the workflow (swap for a Schedule Trigger in production) |
| Request Apollo Scraper | HTTP Request | Starts the Apify actor run against Apollo.io |
| Get Request | HTTP Request | Retrieves the scraped dataset from Apify once ready |
| Filter Needed Fields | Set | Maps only the required contact/company fields |
| Remove Duplicates | Remove Duplicates | Dedupes leads by email across all executions |
| Airtable | Airtable | Creates one record per new lead in the `Lead` table |

---

## ⚠️ Security Notes

**This workflow file, as exported, contains sensitive live credentials that must be removed or rotated before sharing or reusing it:**

- The **Apify API token** is hardcoded in a node note in plain text.
- The **Request Apollo Scraper** node body embeds a full set of **live Apollo.io session cookies** (session tokens, CSRF token, remember-me token, etc.) tied to a real Apollo.io account.

**Before doing anything else with this file:**
1. Immediately **rotate/revoke the exposed Apify API token** in your Apify account settings.
2. **Log out and re-authenticate** the Apollo.io account whose cookies were embedded, invalidating the old session.
3. Remove hardcoded tokens/cookies from the workflow JSON and instead store them in n8n's credential manager (Header Auth credential) or environment variables.
4. Treat Apollo.io session cookies as short-lived secrets — they typically expire and need periodic refreshing, and should never be committed to shared files, repos, or templates.

More generally:
- Never commit this JSON file to a public repository as-is.
- Rotate your Airtable Personal Access Token periodically and scope it to only the base(s) it needs.

---

## 🚀 Possible Enhancements

- Replace the manual trigger with a daily Schedule Trigger and automatic page-tracking (see [Daily Scraping & Pagination](#-daily-scraping--pagination)).
- Add error handling around the Apify actor run in case the scrape fails or returns zero results.
- Add a lead-enrichment follow-up workflow that picks up records where `Start Lead Enrichment` is `true`.
- Add Slack/email notifications summarizing how many new leads were added each run.
- Store the Apify actor run ID in Airtable for auditability.

---

## 📄 License

Internal/private use — adapt as needed for your business. Remember to scrub credentials before sharing this template with anyone else.
