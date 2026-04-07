# Real Estate Cold Outreach Automation System

Automated cold outreach pipeline to lease a commercial/residential space — from **prospect identification** through **lease signing** — powered by [Make.com](https://www.make.com/), Gmail, Airtable, and Google Workspace.

---

## Goal

Acquire **one qualified tenant** to lease the available space through a structured, automated outreach and follow-up sequence.

---

## Outreach Pipeline

```
PROSPECT LIST ──> OUTREACH SEQUENCE ──> SHOWING SCHEDULED ──> POST-SHOWING ──> LEASE SIGNED
  (Airtable)        (Gmail sequence)      (Google Calendar)      (Gmail)         (Google Drive)
(Google Sheets)                            (Airtable CRM)        (Trello)
```

### Pipeline Stages

| # | Stage | Primary Tools | Trigger |
|---|-------|--------------|---------|
| 1 | **Prospect Intake** | Google Sheets → Airtable | Manual CSV import or form fill |
| 2 | **Cold Outreach** | Gmail (Day 0) | Prospect status = "New" |
| 3 | **Follow-Up #1** | Gmail (Day 3) | No reply after 3 days |
| 4 | **Follow-Up #2** | Gmail (Day 7) | No reply after 7 days |
| 5 | **Showing Scheduled** | Google Calendar → Gmail | Prospect replies or books |
| 6 | **Post-Showing Follow-Up** | Gmail (24h after showing) | Showing completed |
| 7 | **Lease Offer** | Gmail + Google Drive | Prospect status = "Interested" |
| 8 | **Closed / Lease Signed** | Airtable + Google Drive | Lease returned signed |

---

## Make.com Scenarios

| Scenario File | Description |
|--------------|-------------|
| `make-scenarios/re-01-prospect-intake.json` | Sheet/CSV import → Airtable CRM + deduplication |
| `make-scenarios/re-02-outreach-sequence.json` | Multi-step email sequences via Gmail |
| `make-scenarios/re-03-showing-scheduler.json` | Calendar booking → CRM status update + confirmation email |
| `make-scenarios/re-04-lease-closing.json` | Post-showing follow-up + lease doc delivery via Drive |

---

## Email Sequence Overview

| Template | Timing | Purpose |
|----------|--------|---------|
| `re-01-initial-outreach.html` | Day 0 | Cold intro — property pitch, key highlights |
| `re-02-follow-up-1.html` | Day 3 | Soft follow-up — add social proof / urgency |
| `re-03-follow-up-2.html` | Day 7 | Final follow-up — direct CTA to schedule a showing |
| `re-04-showing-confirmation.html` | On booking | Confirm date/time, share address + parking info |
| `re-05-post-showing-followup.html` | 24h after showing | Recap highlights, answer objections, next steps |
| `re-06-lease-offer.html` | On "Interested" status | Attach lease draft, outline terms, call to action |

---

## Tool Configuration

| Tool | Purpose | Config File |
|------|---------|-------------|
| **Airtable** | Prospect CRM | `airtable/real-estate-schema.json` |
| **Squarespace** | Inbound inquiry form (optional) | `squarespace/property-inquiry-form.json` |
| **Google Sheets** | Prospect list import & reporting | `google-sheets/prospect-tracker.md` |
| **Gmail** | Automated email sequences | `gmail-templates/` |
| **Google Calendar** | Showing scheduling | `google-calendar/setup.md` |
| **Google Drive** | Lease documents & property photos | `google-drive/folder-structure.json` |
| **Trello** | Deal pipeline tracking | `trello/real-estate-pipeline.json` |

---

## Quick Start

1. **Import Make.com Scenarios** — Upload each JSON from `make-scenarios/` into your Make.com workspace
2. **Set Up Airtable** — Create the Prospects base using `airtable/real-estate-schema.json`
3. **Load Your Prospect List** — Import your target list into Google Sheets using the template in `google-sheets/prospect-tracker.md`
4. **Configure Gmail** — Copy email templates from `gmail-templates/` and personalize with your property details
5. **Set Up Google Calendar** — Create an appointment scheduling page for property showings
6. **Upload Property Docs** — Add lease template and property photos to Google Drive
7. **Connect & Activate** — Wire API connections in Make.com and flip the switch

See `docs/real-estate-setup-guide.md` for detailed step-by-step instructions.

---

## Directory Structure

```
noryey3/
├── README.md
├── make-scenarios/
│   ├── re-01-prospect-intake.json
│   ├── re-02-outreach-sequence.json
│   ├── re-03-showing-scheduler.json
│   └── re-04-lease-closing.json
├── airtable/
│   └── real-estate-schema.json
├── squarespace/
│   └── property-inquiry-form.json
├── google-sheets/
│   └── prospect-tracker.md
├── gmail-templates/
│   ├── re-01-initial-outreach.html
│   ├── re-02-follow-up-1.html
│   ├── re-03-follow-up-2.html
│   ├── re-04-showing-confirmation.html
│   ├── re-05-post-showing-followup.html
│   └── re-06-lease-offer.html
├── trello/
│   └── real-estate-pipeline.json
├── google-calendar/
│   └── setup.md
├── google-drive/
│   └── folder-structure.json
└── docs/
    └── real-estate-setup-guide.md
```
