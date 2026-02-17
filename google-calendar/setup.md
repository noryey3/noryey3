# Google Calendar - Appointment Scheduling Setup

## Overview

Google Calendar is used for scheduling discovery calls with qualified leads and onboarding calls with new clients. The system integrates with Make.com to automatically create events and send confirmations.

---

## Calendar Structure

### Calendars to Create

| Calendar | Color | Purpose |
|----------|-------|---------|
| **Discovery Calls** | Blue | Initial calls with qualified leads |
| **Onboarding Calls** | Green | Onboarding sessions with new clients |
| **Recording Sessions** | Red | Episode recording sessions |
| **Team Meetings** | Yellow | Internal team meetings |

---

## Google Calendar Appointment Schedule Setup

### Discovery Call Appointment Slots

**Settings:**
- **Title:** "Podcast Discovery Call - [Your Company Name]"
- **Duration:** 30 minutes
- **Availability Window:** Monday-Friday, 9:00 AM - 5:00 PM (your timezone)
- **Buffer Between Appointments:** 15 minutes
- **Minimum Scheduling Notice:** 24 hours
- **Maximum Days in Advance:** 14 days

**Booking Page Details:**
- **Description:**
  ```
  Thanks for your interest in our podcast services! During this 30-minute call, we'll:

  - Learn about your podcast goals and vision
  - Discuss which services best fit your needs
  - Answer any questions you have
  - Outline next steps if we're a good fit

  Please come prepared to share:
  - Your podcast concept or existing show details
  - Your target audience
  - Your ideal publishing schedule
  - Your budget expectations
  ```
- **Confirmation Message:**
  ```
  Your discovery call is confirmed! You'll receive a calendar invite shortly.

  Before our call, please have ready:
  1. Any examples of podcasts you admire
  2. A brief description of your target audience
  3. Questions you'd like answered

  Looking forward to speaking with you!
  ```

### Onboarding Call Appointment Slots

**Settings:**
- **Title:** "Podcast Onboarding Session - [Client Name]"
- **Duration:** 60 minutes
- **Availability Window:** Monday-Friday, 10:00 AM - 4:00 PM
- **Buffer Between Appointments:** 30 minutes
- **Minimum Scheduling Notice:** 48 hours
- **Maximum Days in Advance:** 21 days

**Booking Page Details:**
- **Description:**
  ```
  Welcome to the team! This onboarding session will cover:

  - Walkthrough of our production process
  - Setting up your shared Google Drive folder
  - Reviewing your brand guide and podcast style
  - Establishing your episode schedule
  - Setting communication preferences
  - Q&A

  Please complete the onboarding questionnaire before our call (sent separately).
  ```

---

## Make.com Integration

### Scenario: Auto-Create Calendar Events

**Trigger:** Lead status changed to "Discovery Call Scheduled" in Airtable

**Actions:**
1. Create Google Calendar event on "Discovery Calls" calendar
2. Add attendee (lead's email)
3. Set event description with lead context from Airtable
4. Add Google Meet link automatically
5. Update Airtable with calendar event link and date
6. Send custom confirmation email via Gmail

### Event Template - Discovery Call

```json
{
  "summary": "Discovery Call: {lead_name} - Podcast Services",
  "description": "Discovery call with {lead_name} from {company}.\n\nInterested in: {services}\nBudget: {budget}\nCurrent Podcast: {podcast_name}\n\nLead Score: {score} ({grade})\nNotes: {notes}\n\n---\nAirtable Record: {record_url}",
  "start": {
    "dateTime": "{selected_datetime}",
    "timeZone": "America/New_York"
  },
  "end": {
    "dateTime": "{selected_datetime + 30min}",
    "timeZone": "America/New_York"
  },
  "attendees": [
    { "email": "{lead_email}" },
    { "email": "{team_member_email}" }
  ],
  "conferenceData": {
    "createRequest": {
      "requestId": "discovery-{lead_id}",
      "conferenceSolutionKey": { "type": "hangoutsMeet" }
    }
  },
  "reminders": {
    "useDefault": false,
    "overrides": [
      { "method": "email", "minutes": 1440 },
      { "method": "popup", "minutes": 30 }
    ]
  }
}
```

### Event Template - Onboarding Call

```json
{
  "summary": "Onboarding: {client_name} - {podcast_name}",
  "description": "Onboarding session for {client_name}.\n\nPackage: {service_package}\nStart Date: {contract_start}\n\nAgenda:\n1. Welcome & introductions\n2. Process walkthrough\n3. Drive folder & tools setup\n4. Episode schedule planning\n5. Communication preferences\n6. Q&A\n\n---\nDrive Folder: {drive_folder_url}\nTrello Card: {trello_card_url}",
  "start": {
    "dateTime": "{selected_datetime}",
    "timeZone": "America/New_York"
  },
  "end": {
    "dateTime": "{selected_datetime + 60min}",
    "timeZone": "America/New_York"
  },
  "attendees": [
    { "email": "{client_email}" },
    { "email": "{team_member_email}" }
  ],
  "conferenceData": {
    "createRequest": {
      "requestId": "onboarding-{client_id}",
      "conferenceSolutionKey": { "type": "hangoutsMeet" }
    }
  },
  "reminders": {
    "useDefault": false,
    "overrides": [
      { "method": "email", "minutes": 1440 },
      { "method": "email", "minutes": 60 },
      { "method": "popup", "minutes": 15 }
    ]
  }
}
```

---

## Appointment Booking Flow

```
Lead qualifies (Grade A/B)
    │
    ▼
Make.com sends email with booking link
    │
    ▼
Lead clicks Google Calendar Appointment Schedule link
    │
    ▼
Lead selects available time slot
    │
    ▼
Google Calendar creates event + sends invite
    │
    ▼
Make.com webhook detects new calendar event
    │
    ▼
Airtable updated: Status → "Discovery Call Scheduled"
    │
    ▼
Confirmation email sent via Gmail with call prep info
    │
    ▼
24hrs before: Reminder email sent automatically
    │
    ▼
After call: Team member updates status in Airtable
    │
    ▼
If won → Client record created → Onboarding begins
```
