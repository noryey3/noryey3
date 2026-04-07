# Google Sheets — Prospect Tracker

Used to bulk-import prospects into the pipeline and track outreach reporting.

---

## Sheet 1: Prospect Import List

This sheet is the **source of truth for bulk uploads**. Make.com polls it every 15 minutes and
syncs any new rows into Airtable (skipping duplicates by email).

### Columns

| Column | Field | Notes |
|--------|-------|-------|
| A | `First Name` | Required |
| B | `Last Name` | Required |
| C | `Email` | Required — used as unique key |
| D | `Phone` | Optional |
| E | `Company / Business Name` | Optional |
| F | `Business Type` | Dropdown: Retail, Office, Food & Beverage, Health & Wellness, Creative / Studio, Nonprofit, Individual, Other |
| G | `Move-In Timeline` | Dropdown: ASAP, 1-3 months, 3-6 months, 6-12 months, Just Browsing |
| H | `Source` | Where you found them: Google Maps, LinkedIn, Referral, etc. |
| I | `Lead Score` | 1–10: how well they fit the space (10 = perfect fit) |
| J | `Notes` | Any research notes on the prospect |
| K | `Synced to Airtable?` | Formula: auto-set by Make.com (Yes/No) |
| L | `Date Added` | Auto-filled |

### Lead Score Rubric

| Score | Meaning |
|-------|---------|
| 9-10 | Perfect fit — business type matches, active growth, right size |
| 7-8 | Strong fit — likely interested, just needs outreach |
| 5-6 | Possible fit — unclear need, worth reaching out |
| 3-4 | Long shot — might be interested with the right pitch |
| 1-2 | Weak fit — only reach out if list is exhausted |

### How to populate this list
- **Google Maps**: Search for businesses in the target neighborhood, note contact info
- **LinkedIn**: Search for companies of your target size/type in the city
- **Local directories**: Yelp, BizBuySell, Chamber of Commerce member lists
- **Referrals**: People you already know who might need space

---

## Sheet 2: Pipeline Dashboard

Auto-populated by Make.com from Airtable. Do not edit manually.

### Columns

| Column | Data |
|--------|------|
| A | Prospect Name |
| B | Business Name |
| C | Email |
| D | Status |
| E | Lead Score |
| F | Email #1 Sent |
| G | Follow-Up #1 Sent |
| H | Follow-Up #2 Sent |
| I | Showing Date |
| J | Lease Sent |
| K | Lease Signed |
| L | Days in Pipeline |

### Key Metrics (use COUNTIF formulas)

```
Total Prospects:        =COUNTA(A2:A)
Emails Sent:            =COUNTIF(D2:D,"Email Sent") + COUNTIF(D2:D,"Follow-Up 1 Sent") + ...
Replies / Engaged:      =COUNTIF(D2:D,"Replied") + COUNTIF(D2:D,"Showing Scheduled") + ...
Showings Completed:     =COUNTIF(D2:D,"Showing Completed") + COUNTIF(D2:D,"Interested") + ...
Leases Sent:            =COUNTIF(D2:D,"Lease Sent")
Lease Signed:           =COUNTIF(D2:D,"Lease Signed")
Conversion Rate:        =Lease Signed / Total Prospects
```

---

## Sheet 3: Space Details (constants used in email templates)

Fill this in once — Make.com reads these values to personalize outreach emails.

| Variable Name | Value | Description |
|---------------|-------|-------------|
| `SPACE_ADDRESS` | | Full street address |
| `SPACE_NEIGHBORHOOD` | | Neighborhood name (used in subject lines) |
| `SPACE_SIZE` | | Square footage |
| `SPACE_TYPE` | | e.g. Retail Storefront, Office Suite |
| `MONTHLY_RENT` | | Monthly rent in USD |
| `SECURITY_DEPOSIT` | | Deposit amount |
| `LEASE_TERM` | | e.g. 12 months, 24 months |
| `AVAILABLE_DATE` | | Move-in available date |
| `KEY_FEATURE_1` | | Top selling point #1 |
| `KEY_FEATURE_2` | | Top selling point #2 |
| `KEY_FEATURE_3` | | Top selling point #3 |
| `PARKING_INFO` | | Street / lot / garage details |
| `UTILITIES_DETAIL` | | What's included vs tenant-paid |
| `VIRTUAL_TOUR_URL` | | Link to video or Matterport tour |
| `GOOGLE_MAPS_LINK` | | Google Maps URL |
| `CALENDAR_BOOKING_LINK` | | Google Calendar appointment page |
| `YOUR_NAME` | | Your name (sender) |
| `YOUR_EMAIL` | | Your email address |
| `YOUR_PHONE` | | Your phone number |
| `UNSUBSCRIBE_BASE_URL` | | Your unsubscribe handler URL |
| `AIRTABLE_BASE_URL` | | Direct link to Airtable base |
