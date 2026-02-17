# Google Sheets Templates

## Sheet 1: Lead Tracker & Scoring Dashboard

### Tab: "Lead Data"

This sheet receives raw lead data from Make.com and calculates lead scores.

| Column | Header | Source | Notes |
|--------|--------|--------|-------|
| A | Timestamp | Auto | `=NOW()` on insert |
| B | Full Name | Form | From Squarespace |
| C | Email | Form | From Squarespace |
| D | Phone | Form | From Squarespace |
| E | Company | Form | From Squarespace |
| F | Website | Form | From Squarespace |
| G | Has Existing Podcast | Form | Yes / No |
| H | Podcast Name | Form | If applicable |
| I | Services Interested | Form | Comma-separated |
| J | Episodes/Month | Form | Selection value |
| K | Budget Range | Form | Selection value |
| L | Referral Source | Form | Selection value |
| M | Additional Notes | Form | Free text |
| N | **Lead Score** | Formula | Calculated (see below) |
| O | **Lead Grade** | Formula | A / B / C / D |
| P | Lead Status | Make.com | Updated by automation |
| Q | Airtable Record ID | Make.com | For cross-referencing |
| R | Last Email Sent | Make.com | Email sequence tracking |
| S | Days Since Submission | Formula | `=TODAY()-A2` |

### Lead Scoring Formula (Column N)

```
=SUM(
  IF(K2="$5,000+/month", 30,
    IF(K2="$2,500 - $5,000/month", 25,
      IF(K2="$1,000 - $2,500/month", 20,
        IF(K2="$500 - $1,000/month", 10, 5)))),
  IF(G2="Yes", 15, 5),
  IF(OR(ISNUMBER(SEARCH("Full Podcast Production", I2)),
        ISNUMBER(SEARCH("Launch Package", I2))), 20,
    IF(ISNUMBER(SEARCH("Editing Only", I2)), 10, 5)),
  IF(J2="8 episodes (bi-weekly)", 15,
    IF(J2="4 episodes (weekly)", 12,
      IF(J2="1-2 episodes", 8, 5))),
  IF(F2<>"", 10, 0),
  IF(D2<>"", 5, 0),
  IF(L2="Referral", 5, 0)
)
```

**Scoring Breakdown:**

| Factor | Criteria | Points |
|--------|----------|--------|
| Budget | $5,000+/mo | 30 |
| Budget | $2,500-$5,000/mo | 25 |
| Budget | $1,000-$2,500/mo | 20 |
| Budget | $500-$1,000/mo | 10 |
| Budget | Under $500/mo | 5 |
| Existing Podcast | Yes | 15 |
| Existing Podcast | No | 5 |
| Service Type | Full Production or Launch | 20 |
| Service Type | Editing Only | 10 |
| Service Type | Other | 5 |
| Volume | 8 episodes/mo | 15 |
| Volume | 4 episodes/mo | 12 |
| Volume | 1-2 episodes/mo | 8 |
| Volume | Custom/unsure | 5 |
| Has Website | Yes | 10 |
| Has Phone | Yes | 5 |
| Referral Source | Referral | 5 |

**Max Score: 100**

### Lead Grade Formula (Column O)

```
=IF(N2>=80, "A",
  IF(N2>=60, "B",
    IF(N2>=40, "C", "D")))
```

| Grade | Score Range | Action |
|-------|------------|--------|
| A | 80-100 | Immediate personal outreach + discovery call |
| B | 60-79 | Automated outreach sequence (priority) |
| C | 40-59 | Automated outreach sequence (standard) |
| D | 0-39 | Add to nurture list |

---

### Tab: "Pipeline Dashboard"

Summary dashboard with charts and KPIs.

| Cell | Label | Formula |
|------|-------|---------|
| B2 | Total Leads (This Month) | `=COUNTIFS('Lead Data'!A:A, ">="&DATE(YEAR(TODAY()),MONTH(TODAY()),1))` |
| B3 | Grade A Leads | `=COUNTIF('Lead Data'!O:O, "A")` |
| B4 | Grade B Leads | `=COUNTIF('Lead Data'!O:O, "B")` |
| B5 | Discovery Calls Booked | `=COUNTIF('Lead Data'!P:P, "Discovery Call Scheduled")` |
| B6 | Conversion Rate | `=COUNTIF('Lead Data'!P:P,"Won")/COUNTA('Lead Data'!B:B)-1` |
| B7 | Avg Lead Score | `=AVERAGE('Lead Data'!N:N)` |
| B8 | Leads Needing Follow-Up | `=COUNTIFS('Lead Data'!P:P,"Contacted",'Lead Data'!S:S,">3")` |

### Charts to Create:
1. **Lead Volume by Week** — Bar chart from Timestamp column
2. **Lead Grade Distribution** — Pie chart from Grade column
3. **Pipeline Funnel** — Funnel chart from Status column
4. **Lead Source Breakdown** — Pie chart from Referral Source
5. **Revenue Pipeline** — Sum of Budget Range by Status

---

## Sheet 2: Revenue Tracker

### Tab: "Monthly Revenue"

| Column | Header | Notes |
|--------|--------|-------|
| A | Month | Jan 2026, Feb 2026, etc. |
| B | New Clients | Count of new clients |
| C | Churned Clients | Count of lost clients |
| D | Total Active Clients | Running total |
| E | MRR (Monthly Recurring Revenue) | Sum of active client rates |
| F | New Revenue | Revenue from new clients |
| G | Churned Revenue | Lost revenue |
| H | Net Revenue Change | `=F-G` |
| I | Growth Rate | `=H/E(previous row)` |

### Tab: "Client Revenue"

| Column | Header | Notes |
|--------|--------|-------|
| A | Client Name | From Airtable |
| B | Package | Service type |
| C | Monthly Rate | Dollar amount |
| D | Start Date | Contract start |
| E | Months Active | `=DATEDIF(D2, TODAY(), "M")` |
| F | Lifetime Value | `=C2*E2` |
| G | Status | Active / Paused / Churned |

---

## Sheet 3: Email Sequence Tracker

### Tab: "Outreach Log"

| Column | Header | Notes |
|--------|--------|-------|
| A | Lead Name | From Airtable |
| B | Email | Lead email |
| C | Sequence Step | 1, 2, 3, etc. |
| D | Email Type | Initial / Follow-up 1 / Follow-up 2 |
| E | Sent Date | When email was sent |
| F | Opened | Yes/No (if tracking available) |
| G | Replied | Yes/No |
| H | Next Action Date | When to send next email |
| I | Status | Active / Completed / Unsubscribed |
