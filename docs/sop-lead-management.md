# SOP: Lead Management Process

## Purpose
Standard operating procedure for managing podcast business leads from initial capture through conversion or nurture.

---

## Process Overview

```
Form Submission → Auto-Capture → Auto-Score → Auto-Outreach → Booking → Discovery Call → Decision
```

---

## Automated Steps (No Action Required)

These steps are handled automatically by Make.com:

| Step | What Happens | Scenario |
|------|-------------|----------|
| 1 | Lead submits Squarespace form | — |
| 2 | Data captured in Airtable + Google Sheets | Scenario 01 |
| 3 | Lead scored (0-100) and graded (A-D) | Scenario 01 |
| 4 | Grade A: Internal notification sent | Scenario 01 |
| 5 | Grade B/C: Initial outreach email sent | Scenario 02 |
| 6 | Grade D: Added to nurture list | Scenario 02 |
| 7 | Follow-Up #1 sent (Day 3) | Scenario 03 |
| 8 | Follow-Up #2 sent (Day 8) | Scenario 03 |
| 9 | No response after 3 emails → Status: Nurture | Scenario 03 |
| 10 | Lead books call → Status updated, confirmation sent | Scenario 04 |

---

## Manual Steps (Action Required)

### When You Receive a Grade A Lead Notification

**Timeframe:** Within 1 hour of notification

1. Open the Airtable record from the notification email
2. Review the lead's details, score, and interests
3. Send a **personal** email (not the automated template)
4. Reference something specific from their form submission
5. Include your booking link
6. Update the Lead Status to "Contacted" in Airtable
7. Set the Follow Up Date to 2 days from now

### After a Discovery Call

**Timeframe:** Within 30 minutes of the call ending

1. Open the lead record in Airtable
2. Update the Lead Status:
   - **"Won"** → Client said yes → Triggers onboarding automation
   - **"Proposal Sent"** → Needs a proposal → Set Follow Up Date to 3 days
   - **"Lost"** → Not a fit → Add reason in Notes field
   - **"Nurture"** → Not now, maybe later
3. If "Won": Add monthly rate in Notes (for the automation to reference)
4. If "Proposal Sent": Create and send the proposal, then follow up on the set date

### Weekly Lead Review (Every Monday)

1. Open the **"Follow-Up Due"** view in Airtable
2. Review all leads with overdue follow-up dates
3. For each lead, decide:
   - Send a personal follow-up email
   - Move to "Nurture" if no progress
   - Close as "Lost" if clearly not interested
4. Open the **"Pipeline Dashboard"** in Google Sheets
5. Review KPIs: total leads, conversion rate, average lead score
6. Note any trends or issues

---

## Lead Score Adjustment

Review and adjust lead scoring weights monthly based on actual conversions:

1. Export all "Won" leads from the last 3 months
2. Analyze which factors best predicted conversion
3. Adjust scoring weights in:
   - Google Sheets formulas (Column N)
   - Make.com Scenario 01 (variable calculation)
   - Make.com Scenario 02 (router thresholds)

---

## Escalation Procedures

| Situation | Action |
|-----------|--------|
| Lead replies to automated email | Manually take over the conversation; update status to "Qualified" |
| Lead complains about email frequency | Immediately update Outreach Sequence Step to 99 to stop emails |
| High-profile lead identified | Notify founder/owner for personal outreach |
| Automated email fails to send | Check Make.com execution log; resend manually if needed |
| Duplicate lead detected | Merge records in Airtable; keep the one with more data |
