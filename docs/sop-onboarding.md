# SOP: Client Onboarding Process

## Purpose
Standard operating procedure for onboarding new podcast clients from contract signing through first episode production.

---

## Process Overview

```
Lead Won → Auto-Setup (Drive + Trello + Email) → Questionnaire → Onboarding Call → First Episode → Active Client
```

---

## Automated Steps (Handled by Make.com Scenario 05)

When a lead's status changes to "Won" in Airtable, the following happens automatically:

| Step | Action | Tool |
|------|--------|------|
| 1 | Client record created in Airtable | Airtable |
| 2 | Google Drive folder structure created | Google Drive |
| 3 | Template files copied to client folder | Google Drive |
| 4 | Folder shared with client email | Google Drive |
| 5 | Onboarding card created on Trello | Trello |
| 6 | Onboarding checklist added to card | Trello |
| 7 | Welcome email sent with all links | Gmail |
| 8 | Client added to Revenue Tracker | Google Sheets |

**Automated reminder:** If client hasn't completed onboarding steps after 3 days, a reminder email is sent (Scenario 06, Route B).

---

## Manual Steps (Action Required)

### Day 1: After Automation Runs

**Verify automated setup:**
1. Check Airtable → Clients table → New record exists
2. Check Google Drive → Client folder created with all subfolders
3. Check Trello → Card exists in "New Client Intake" list
4. Check client received the welcome email

**Prepare client-specific materials:**
1. Customize the service agreement in the client's Drive folder
2. Set up billing (invoice tool of your choice)
3. Move Trello card to **"2. Onboarding In Progress"**

### Day 1-3: Client Completes Their Steps

Monitor the Trello card checklist. Client needs to:
- [ ] Complete the onboarding questionnaire (Google Form)
- [ ] Upload brand assets to Drive
- [ ] Review and sign the agreement
- [ ] Book their onboarding call

**As items are completed:**
1. Check off items on the Trello card
2. Review questionnaire responses
3. Review brand assets for completeness

### Onboarding Call Day

**Before the call (30 min prior):**
1. Review the client's questionnaire responses
2. Review their brand assets
3. Prepare an episode schedule proposal
4. Open their Drive folder for screen sharing

**During the call (60 min):**
1. Welcome and introductions (5 min)
2. Walk through production process (10 min)
3. Review Drive folder and show how to upload files (10 min)
4. Discuss brand, style, and podcast format preferences (15 min)
5. Plan the first 4 episodes together (15 min)
6. Set communication preferences and expectations (5 min)

**After the call (within 1 hour):**
1. Send a summary email with:
   - Episode schedule for the first month
   - Recording dates
   - Any action items for the client
   - Link to their Drive folder
2. Check off "Complete onboarding call" on Trello
3. If all tasks done, check off "Mark onboarding as complete" on Trello
4. In Airtable, check the **"Onboarding Complete"** checkbox
   - This triggers automatic status update to "Active" (Scenario 06)

### Post-Onboarding: First Episode Setup

1. Create the first episode folder in Drive:
   `03 - Episodes / EP001 - {Episode Title}`
2. Create subfolders: Raw Audio, Edited Audio, Show Notes, Artwork, Social Media Assets
3. Create an episode card on Trello in the **"4. Episode Scheduled"** list
4. Add the episode production checklist to the card
5. Create an Episode record in Airtable
6. Send the client their recording instructions/schedule

---

## Onboarding Checklist (Complete Reference)

| # | Task | Owner | Tool | Automated? |
|---|------|-------|------|-----------|
| 1 | Create client record | System | Airtable | Yes |
| 2 | Create Drive folder structure | System | Google Drive | Yes |
| 3 | Copy templates to client folder | System | Google Drive | Yes |
| 4 | Share folder with client | System | Google Drive | Yes |
| 5 | Create Trello card + checklist | System | Trello | Yes |
| 6 | Send welcome email | System | Gmail | Yes |
| 7 | Add to revenue tracker | System | Google Sheets | Yes |
| 8 | Customize service agreement | Team | Google Drive | No |
| 9 | Set up billing | Team | Billing tool | No |
| 10 | Client completes questionnaire | Client | Google Forms | No |
| 11 | Client uploads brand assets | Client | Google Drive | No |
| 12 | Client signs agreement | Client | Google Drive | No |
| 13 | Client books onboarding call | Client | Google Calendar | No |
| 14 | Conduct onboarding call | Team + Client | Google Meet | No |
| 15 | Send post-call summary | Team | Gmail | No |
| 16 | Set up podcast hosting account | Team | Hosting platform | No |
| 17 | Create first episode folder | Team | Google Drive | No |
| 18 | Mark onboarding complete | Team | Airtable | No |
| 19 | Status updated to Active | System | Airtable | Yes |

---

## Quality Checks

Before marking onboarding as complete, verify:

- [ ] Client has access to their Drive folder
- [ ] Brand assets are uploaded and reviewed
- [ ] Agreement is signed by both parties
- [ ] Billing is set up and first invoice sent
- [ ] Episode schedule is agreed upon
- [ ] Recording process is clear to the client
- [ ] Communication channel is established (email/Slack/etc.)
- [ ] Podcast hosting account is ready (if applicable)

---

## Troubleshooting

| Issue | Resolution |
|-------|-----------|
| Client hasn't responded to welcome email | Check spam; resend manually; try phone call |
| Drive folder sharing failed | Manually share; check if client email is valid |
| Client hasn't booked onboarding call after 5 days | Personal phone call or direct email with specific time proposals |
| Client wants to change package after signing | Update Airtable Client record, adjust billing, notify team |
| Trello card wasn't created | Check Make.com execution log; create card manually from template |
