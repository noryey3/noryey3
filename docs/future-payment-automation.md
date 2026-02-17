# Future Roadmap: Automated Payment System

## Current State (Manual)

Payment tracking is currently handled manually via:
- **Airtable**: `Payment Status` and `Payment Method` fields on the Clients table
- **Google Sheets**: Revenue Tracker spreadsheet for MRR and LTV reporting
- **Manual invoicing**: Invoices created and sent outside the automation system

---

## Planned: Automated Payment Integration

When ready to automate payments, the system will be extended with a payment gateway (Stripe or PayPal) integrated through Make.com.

### Phase 1: Invoice Automation (Recommended First Step)

**Estimated timeline:** Can be added anytime

**Tools to add:**
- Stripe or PayPal (payment processing)
- Make.com Stripe/PayPal module (already available)

**New Make.com Scenario: 07 - Invoice Generation**

```
Trigger: Client onboarding complete (Client Status = "Active")
    │
    ▼
Create Stripe customer from Airtable client data
    │
    ▼
Generate invoice based on Service Package and Monthly Rate
    │
    ▼
Send invoice link via Gmail
    │
    ▼
Update Airtable: Payment Status = "Invoice Sent"
    │
    ▼
Webhook listens for payment → Update to "Paid"
```

**Scenario blueprint:**
```json
{
  "scenario": {
    "name": "07 - Invoice Generation (Future)",
    "description": "Auto-generates and sends invoices when a client becomes active"
  },
  "modules": [
    {
      "id": 1,
      "name": "Airtable - Watch Records",
      "type": "trigger",
      "config": {
        "table": "Clients",
        "filterFormula": "AND({Client Status} = 'Active', {Payment Status} = 'Pending')"
      }
    },
    {
      "id": 2,
      "name": "Stripe - Create Customer",
      "type": "action",
      "config": {
        "name": "{{1.Client Name}}",
        "email": "{{1.Email}}",
        "metadata": {
          "airtable_client_id": "{{1.id}}",
          "service_package": "{{1.Service Package}}"
        }
      }
    },
    {
      "id": 3,
      "name": "Stripe - Create Invoice",
      "type": "action",
      "config": {
        "customer": "{{2.id}}",
        "currency": "usd",
        "description": "{{1.Service Package}} - {{formatDate(now, 'MMMM YYYY')}}",
        "amount": "{{1.Monthly Rate * 100}}",
        "auto_advance": true
      }
    },
    {
      "id": 4,
      "name": "Stripe - Send Invoice",
      "type": "action",
      "config": {
        "invoiceId": "{{3.id}}"
      }
    },
    {
      "id": 5,
      "name": "Airtable - Update Record",
      "type": "action",
      "config": {
        "table": "Clients",
        "recordId": "{{1.id}}",
        "fields": {
          "Payment Status": "Invoice Sent",
          "Payment Method": "Stripe",
          "Next Invoice Date": "{{addDays(now, 30)}}"
        }
      }
    }
  ]
}
```

### Phase 2: Recurring Payment Automation

**Estimated timeline:** After Phase 1 is stable

**New Make.com Scenario: 08 - Recurring Billing**

```
Trigger: Scheduled — runs on the 1st of each month (or per client billing cycle)
    │
    ▼
Airtable: Find all active clients where Next Invoice Date = today
    │
    ▼
For each client:
    ├── Stripe: Create and send invoice
    ├── Airtable: Update Payment Status = "Invoice Sent"
    ├── Airtable: Set Next Invoice Date + billing cycle
    └── Gmail: Send invoice notification
    │
    ▼
Stripe webhook: Payment received
    ├── Airtable: Update Payment Status = "Paid"
    ├── Google Sheets: Update Revenue Tracker
    └── Gmail: Send payment receipt
```

**Airtable changes needed:**
- Add `Stripe Customer ID` field to Clients table
- Add `Last Payment Date` field
- Add `Payment History` linked table

### Phase 3: Payment Failure Handling

**New Make.com Scenario: 09 - Payment Failure & Dunning**

```
Trigger: Stripe webhook — payment_intent.payment_failed
    │
    ▼
Airtable: Update Payment Status = "Overdue"
    │
    ▼
Gmail: Send payment failure notification to client
    │
    ▼
Wait 3 days → Retry payment via Stripe
    │
    ▼
If still failed after 3 retries:
    ├── Airtable: Update Client Status = "Paused"
    ├── Gmail: Send account pause notification
    └── Trello: Add "Payment Issue" label to client card
```

### Phase 4: Full Financial Dashboard

**Google Sheets additions:**
- Auto-populated payment history from Stripe webhooks
- Real-time MRR calculations
- Churn tracking linked to payment failures
- Revenue by service package breakdown
- Projected revenue forecasting

---

## New Airtable Table: Payments (Future)

```json
{
  "name": "Payments",
  "description": "Tracks all payment transactions — added when payment automation is enabled",
  "fields": [
    { "name": "Payment ID", "type": "autonumber" },
    { "name": "Client", "type": "linkedRecord", "linkedTable": "Clients" },
    { "name": "Amount", "type": "currency", "currencySymbol": "$" },
    { "name": "Currency", "type": "singleSelect", "options": ["USD", "EUR", "GBP", "CAD"] },
    { "name": "Payment Date", "type": "dateTime" },
    { "name": "Payment Method", "type": "singleSelect", "options": ["Stripe", "PayPal", "Bank Transfer", "Other"] },
    { "name": "Stripe Invoice ID", "type": "singleLineText" },
    { "name": "Stripe Payment Intent ID", "type": "singleLineText" },
    { "name": "Status", "type": "singleSelect", "options": ["Pending", "Paid", "Failed", "Refunded", "Disputed"] },
    { "name": "Invoice PDF", "type": "url" },
    { "name": "Billing Period", "type": "singleLineText", "description": "e.g., Feb 2026" },
    { "name": "Service Package", "type": "singleSelect", "options": [
      "Podcast Editing",
      "Podcast Launching",
      "Content Research",
      "Publishing",
      "Custom Package"
    ]},
    { "name": "Notes", "type": "multilineText" }
  ],
  "views": [
    {
      "name": "All Payments",
      "type": "grid",
      "sort": [{ "field": "Payment Date", "direction": "desc" }]
    },
    {
      "name": "Overdue",
      "type": "grid",
      "filter": { "field": "Status", "operator": "is", "value": "Failed" }
    },
    {
      "name": "By Client",
      "type": "grid",
      "groupBy": "Client"
    },
    {
      "name": "Monthly Revenue",
      "type": "grid",
      "groupBy": "Billing Period"
    }
  ]
}
```

---

## Implementation Checklist

When you're ready to add payment automation:

- [ ] Choose payment provider (Stripe recommended for subscription billing)
- [ ] Create Stripe account and get API keys
- [ ] Add Stripe connection in Make.com
- [ ] Create the Payments table in Airtable
- [ ] Build Scenario 07 (Invoice Generation)
- [ ] Test with a single client
- [ ] Build Scenario 08 (Recurring Billing)
- [ ] Build Scenario 09 (Payment Failure Handling)
- [ ] Set up Stripe webhooks in Make.com
- [ ] Update Google Sheets Revenue Tracker to pull from Payments table
- [ ] Go live with automated billing

---

## Cost Estimates

| Tool | Cost | Notes |
|------|------|-------|
| Stripe | 2.9% + $0.30 per transaction | No monthly fee |
| PayPal | 2.99% + $0.49 per transaction | Alternative option |
| Make.com | Included in existing plan | May need extra operations depending on volume |

---

## Alternative: PayPal Integration

If PayPal is preferred over Stripe, the same scenarios apply with minor changes:
- Replace Stripe modules with PayPal modules in Make.com
- PayPal Invoicing API handles invoice creation and sending
- PayPal IPN (Instant Payment Notification) replaces Stripe webhooks
- Same Airtable and Google Sheets updates apply
