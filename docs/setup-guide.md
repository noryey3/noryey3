# Setup Guide — Podcast Business Automation System

Complete step-by-step instructions to deploy the full automation pipeline.

---

## Prerequisites

- [Make.com](https://www.make.com/) account (Core plan or higher recommended)
- [Airtable](https://airtable.com/) account (Pro plan for automations)
- [Squarespace](https://www.squarespace.com/) website (Business plan or higher for code injection)
- Google Workspace account (Gmail, Calendar, Drive, Sheets)
- [Trello](https://trello.com/) account (free tier works)

---

## Phase 1: Set Up Airtable (CRM Database)

### 1.1 Create the Base
1. Log into Airtable and create a new base called **"Podcast Business CRM"**
2. Reference `airtable/schema.json` for the complete schema

### 1.2 Create the Tables
Create 4 tables with fields as specified in the schema:

**Table 1: Leads**
- Contains all incoming leads
- Key fields: Full Name, Email, Lead Score, Lead Status, Pipeline Stage
- Create all views: All Leads, New Leads, Pipeline Board, Follow-Up Due, High-Value Leads

**Table 2: Clients**
- Converted leads who are active clients
- Linked to Leads table via "Lead Record" field
- Create views: All Clients, Onboarding In Progress, Active Clients

**Table 3: Episodes**
- Individual podcast episodes
- Linked to Clients table
- Create the Production Pipeline kanban view

**Table 4: Email Log**
- Tracks all automated emails
- Linked to both Leads and Clients tables

### 1.3 Set Up Views
- Create the kanban views for Pipeline Board and Production Pipeline
- Set up filters for actionable views (New Leads, Follow-Up Due)

---

## Phase 2: Set Up Squarespace Lead Capture Form

### 2.1 Create the Form
1. In your Squarespace site editor, go to the page for lead capture (e.g., `/get-started`)
2. Add a **Form Block**
3. Configure all fields as specified in `squarespace/form-config.json`
4. Style the form to match your brand

### 2.2 Configure Webhook (after Make.com setup)
1. After creating Scenario 01 in Make.com, you'll get a webhook URL
2. In Squarespace, go to **Settings > Developer Tools > Webhooks** (if available)
3. Or use **Settings > Advanced > Code Injection** to add a custom form handler
4. Alternative: Use Make.com's native Squarespace integration module

---

## Phase 3: Set Up Google Workspace

### 3.1 Google Sheets
Create 3 spreadsheets:

**Spreadsheet 1: "Lead Tracker & Scoring Dashboard"**
- Tab "Lead Data" — columns A through S as specified in `google-sheets/templates.md`
- Add the lead scoring formula in column N
- Add the lead grade formula in column O
- Tab "Pipeline Dashboard" — KPI formulas and charts

**Spreadsheet 2: "Revenue Tracker"**
- Tab "Monthly Revenue" — monthly MRR tracking
- Tab "Client Revenue" — per-client revenue and LTV

**Spreadsheet 3: "Email Sequence Tracker"**
- Tab "Outreach Log" — tracks all email sequences

### 3.2 Google Calendar
1. Create 4 calendars as specified in `google-calendar/setup.md`:
   - Discovery Calls (Blue)
   - Onboarding Calls (Green)
   - Recording Sessions (Red)
   - Team Meetings (Yellow)

2. Set up **Google Calendar Appointment Schedule**:
   - Create a "Discovery Call" appointment type (30 min)
   - Create an "Onboarding Call" appointment type (60 min)
   - Configure availability windows, buffers, and booking rules
   - Copy the booking page URLs — you'll need these for Make.com

### 3.3 Google Drive
1. Create the folder structure as specified in `google-drive/folder-structure.json`
2. Create the root folder: **"Podcast Business"**
3. Create subfolders: 01 - Clients, 02 - Templates, 03 - Business Operations, 04 - Leads & Sales
4. Populate the Templates folder with your template documents
5. Note the folder IDs — you'll need these for Make.com

### 3.4 Google Forms
1. Create an **"Onboarding Questionnaire"** form with questions about:
   - Brand details (colors, fonts, logo upload)
   - Podcast style preferences
   - Target audience description
   - Episode format preferences
   - Communication preferences
   - Any existing episodes or content
2. Copy the form URL for use in email templates

---

## Phase 4: Set Up Trello

### 4.1 Create the Board
1. Create a board called **"Podcast Production Pipeline"**
2. Create all 10 lists as specified in `trello/board-structure.json`
3. Create the labels (color-coded)

### 4.2 Configure Power-Ups
1. Enable the **Calendar** power-up for due date visualization
2. Enable **Custom Fields** and create the fields specified in the schema

### 4.3 Set Up Butler Automations (optional)
1. Configure Trello Butler rules for the automations listed in `trello/board-structure.json`
2. These are supplementary to the Make.com automations

---

## Phase 5: Set Up Make.com Scenarios

**Important: Set up scenarios in order, as later scenarios depend on earlier ones.**

### 5.1 Scenario 01 — Lead Capture
File: `make-scenarios/01-lead-capture.json`

1. Create a new scenario
2. Add **Webhooks > Custom Webhook** as the first module
3. Click "Add" to create the webhook and copy the URL
4. **Go back to Squarespace and configure this webhook URL** (Phase 2.2)
5. Add **Tools > Set Variable** module for lead scoring
6. Add **Airtable > Create a Record** module (connect your Airtable account, select the Leads table)
7. Add **Google Sheets > Add a Row** module
8. Add a **Router** with two routes based on lead grade
9. Add **Gmail > Send Email** for Grade A internal notifications
10. Add **Webhooks > Webhook Response** for success response
11. Configure error handling: Break + Notify
12. **Test:** Submit a test form on Squarespace and verify data appears in Airtable and Sheets

### 5.2 Scenario 02 — Lead Qualification
File: `make-scenarios/02-lead-qualification.json`

1. Create a new scenario with **scheduled** trigger (every 15 min)
2. Add **Airtable > Search Records** with the filter for new, unprocessed leads
3. Add **Iterator** to process leads one by one
4. Add **Router** with 4 routes (Grade A, B, C, D)
5. For each route, add the appropriate Airtable update and Gmail modules
6. For Grade B route, paste the HTML from `gmail-templates/01-initial-outreach.html` into the Gmail module
7. **Replace** `YOUR_GOOGLE_CALENDAR_APPOINTMENT_URL` with your actual Discovery Call booking link
8. Add Email Log record creation for each email sent
9. **Test:** Create a test lead in Airtable with status "New" and score 70

### 5.3 Scenario 03 — Automated Outreach
File: `make-scenarios/03-automated-outreach.json`

1. Create a scheduled scenario running **daily at 9 AM**
2. Add **Airtable > Search Records** with filter for leads due for follow-up
3. Add **Router** with 2 routes: Follow-Up #1 (step 1) and Follow-Up #2 (step 2)
4. For each route, add Gmail, Airtable update, Email Log, and Sheets update modules
5. Paste HTML from `gmail-templates/02-follow-up-1.html` and `03-follow-up-2.html`
6. **Test:** Set a test lead's sequence step to 1 with a past follow-up date

### 5.4 Scenario 04 — Booking & Scheduling
File: `make-scenarios/04-booking-scheduling.json`

1. Create a scenario with **Google Calendar > Watch Events** trigger
2. Select the "Discovery Calls" calendar
3. Add variable extraction for attendee email and call details
4. Add **Airtable > Search Records** to find the lead by email
5. Add Router to handle existing vs. new leads
6. Add Airtable update, Gmail confirmation, and Sheets update modules
7. Paste HTML from `gmail-templates/04-booking-confirmation.html`
8. **Test:** Create a test calendar event with a lead's email as attendee

### 5.5 Scenario 05 — Client Onboarding
File: `make-scenarios/05-client-onboarding.json`

1. Create a scheduled scenario (every 15 min)
2. Add **Airtable > Search Records** for leads with status "Won" and no linked client
3. Add all **Google Drive > Create Folder** modules for the client folder structure
4. Add **Google Drive > Share** module
5. Add **Airtable > Create Record** for the Clients table
6. Add **Trello > Create Card** and **Create Checklist** modules
7. Add **Gmail > Send Email** with welcome template from `gmail-templates/05-welcome-onboarding.html`
8. **Replace** `YOUR_ONBOARDING_CALENDAR_URL` and `YOUR_GOOGLE_FORMS_QUESTIONNAIRE_URL`
9. **Test:** Change a test lead's status to "Won" in Airtable

### 5.6 Scenario 06 — Delivery & Fulfillment
File: `make-scenarios/06-delivery-fulfillment.json`

1. Create a scheduled scenario (every 30 min)
2. Set up the main Router with 3 routes
3. **Route A:** Episode delivery — watches for approved episodes, sends delivery email
4. **Route B:** Onboarding reminder — sends 1 reminder after 3 days
5. **Route C:** Onboarding completion — updates status when onboarding is done
6. **Test each route independently**

---

## Phase 6: End-to-End Testing

### Test the Full Pipeline
1. **Submit a test lead** through the Squarespace form
2. **Verify** it appears in Airtable (Leads table) and Google Sheets
3. **Wait or manually trigger** Scenario 02 — verify lead is qualified and email is sent
4. **Verify** the outreach email arrives with correct content and booking link
5. **Book a discovery call** using the link — verify Scenario 04 updates Airtable
6. **Manually update** lead status to "Won" — verify Scenario 05 creates:
   - Client record in Airtable
   - Google Drive folder structure (shared with client email)
   - Trello card with onboarding checklist
   - Welcome email with all links
7. **Check** the Trello board — verify card is in "New Client Intake" with checklist
8. **Verify** Google Drive folder has all subfolders and template files
9. **Mark onboarding complete** in Airtable — verify status updates

### Common Issues
| Issue | Solution |
|-------|----------|
| Webhook not receiving data | Check Squarespace webhook URL; test with Make.com's "Run once" |
| Airtable field mapping errors | Verify field names match exactly (case-sensitive) |
| Gmail sending limits | Google Workspace: 2,000/day; Free Gmail: 500/day |
| Duplicate records | Add deduplication checks using email as unique identifier |
| Drive sharing failures | Ensure the Google account has permission to share externally |

---

## Phase 7: Go Live

1. Turn on all 6 scenarios in Make.com
2. Set appropriate scheduling intervals
3. Configure error notifications (email alerts for any failures)
4. Monitor the Make.com execution history for the first week
5. Review Airtable data accuracy daily for the first week
6. Adjust lead scoring weights based on actual conversion data
