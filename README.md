# Podcast Business Automation System

End-to-end automation for a podcast business — from **lead generation** through **client onboarding** — powered by [Make.com](https://www.make.com/) (formerly Integromat).

---

## Service Offerings

| Service | Description |
|---------|-------------|
| **Podcast Editing** | Professional audio editing, mixing, mastering, noise removal, and post-production |
| **Podcast Launching** | Full launch package — branding, trailer, hosting setup, first episodes, and launch strategy |
| **Content Research** | Topic research, guest sourcing, episode outlines, talking points, and show notes |
| **Publishing** | Upload, scheduling, distribution to all platforms, SEO optimization, and show notes publishing |
| **Custom Package** | Mix and match any services above, tailored to specific client needs |

---

## System Overview

```
LEAD GENERATION ──> QUALIFICATION ──> OUTREACH ──> BOOKING ──> ONBOARDING ──> DELIVERY
   (Squarespace)     (Airtable)       (Gmail)    (Google Cal)  (Trello)    (Google Drive)
                   (Google Sheets)                              (Gmail)
```

### Pipeline Phases

| # | Phase | Primary Tools | Trigger |
|---|-------|--------------|---------|
| 1 | **Lead Capture** | Squarespace Form → Airtable | Form submission webhook |
| 2 | **Lead Qualification** | Airtable → Google Sheets | New Airtable record |
| 3 | **Automated Outreach** | Gmail (sequences) | Lead status change |
| 4 | **Booking & Scheduling** | Google Calendar → Airtable | Calendar link clicked |
| 5 | **Client Onboarding** | Trello → Gmail → Google Drive | Booking confirmed |
| 6 | **Delivery & Fulfillment** | Google Drive → Trello → Gmail | Onboarding complete |

---

## Make.com Scenarios

| Scenario File | Description |
|--------------|-------------|
| `make-scenarios/01-lead-capture.json` | Squarespace webhook → Airtable + Sheets |
| `make-scenarios/02-lead-qualification.json` | Score leads and route to outreach |
| `make-scenarios/03-automated-outreach.json` | Multi-step email sequences via Gmail |
| `make-scenarios/04-booking-scheduling.json` | Calendar booking → CRM update |
| `make-scenarios/05-client-onboarding.json` | Trello board + Drive folders + welcome email |
| `make-scenarios/06-delivery-fulfillment.json` | Episode delivery + feedback loop |

**Future Scenarios (Payment Automation):**

| Scenario | Description | Status |
|----------|-------------|--------|
| `07 - Invoice Generation` | Auto-generate and send invoices via Stripe/PayPal | Planned |
| `08 - Recurring Billing` | Monthly automated billing per client billing cycle | Planned |
| `09 - Payment Failure & Dunning` | Handle failed payments, retries, account pausing | Planned |

See `docs/future-payment-automation.md` for full roadmap and blueprints.

---

## Tool Configuration

| Tool | Purpose | Config File |
|------|---------|-------------|
| **Airtable** | CRM / Lead database | `airtable/schema.json` |
| **Squarespace** | Lead capture forms | `squarespace/form-config.json` |
| **Google Sheets** | Reporting & lead scoring | `google-sheets/templates.md` |
| **Gmail** | Email sequences | `gmail-templates/` |
| **Trello** | Project management & onboarding | `trello/board-structure.json` |
| **Google Calendar** | Discovery call scheduling | `google-calendar/setup.md` |
| **Google Drive** | Client folders & deliverables | `google-drive/folder-structure.json` |

---

## Quick Start

1. **Import Make.com Scenarios** — Import each JSON blueprint from `make-scenarios/` into your Make.com workspace
2. **Set Up Airtable** — Create the base using the schema in `airtable/schema.json`
3. **Configure Squarespace** — Add the intake form using `squarespace/form-config.json`
4. **Set Up Google Workspace** — Configure Sheets, Calendar, Drive, and Gmail
5. **Create Trello Board** — Set up the board using `trello/board-structure.json`
6. **Connect & Test** — Wire up all API connections in Make.com and run test data

See `docs/setup-guide.md` for detailed step-by-step instructions.

---

## Directory Structure

```
podcast-automation/
├── README.md
├── make-scenarios/           # Make.com scenario blueprints (JSON)
│   ├── 01-lead-capture.json
│   ├── 02-lead-qualification.json
│   ├── 03-automated-outreach.json
│   ├── 04-booking-scheduling.json
│   ├── 05-client-onboarding.json
│   └── 06-delivery-fulfillment.json
├── airtable/
│   └── schema.json           # Base, tables, fields, views
├── squarespace/
│   └── form-config.json      # Form fields and webhook setup
├── google-sheets/
│   └── templates.md          # Sheet structure and formulas
├── gmail-templates/
│   ├── 01-initial-outreach.html
│   ├── 02-follow-up-1.html
│   ├── 03-follow-up-2.html
│   ├── 04-booking-confirmation.html
│   ├── 05-welcome-onboarding.html
│   ├── 06-onboarding-checklist.html
│   └── 07-episode-delivery.html
├── trello/
│   └── board-structure.json  # Lists, cards, labels, checklists
├── google-calendar/
│   └── setup.md              # Calendar and appointment slots config
├── google-drive/
│   └── folder-structure.json # Client folder templates
└── docs/
    ├── setup-guide.md              # Full setup instructions
    ├── sop-lead-management.md
    ├── sop-onboarding.md
    └── future-payment-automation.md # Payment automation roadmap
```
