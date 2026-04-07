# Google Calendar — Property Showing Setup

## Overview

Set up a Google Calendar appointment scheduling page so prospects can self-book a showing with zero back-and-forth.

---

## Step 1: Create the Showings Calendar

1. Go to [calendar.google.com](https://calendar.google.com)
2. Click **+ Other calendars → Create new calendar**
3. Name it: `Property Showings — [Address]`
4. Set timezone to your local timezone
5. Note the **Calendar ID** (used in Make.com RE-03 scenario)

---

## Step 2: Create an Appointment Schedule (Google Calendar Booking Page)

> Available with Google Workspace (Business Starter and above)

1. In Google Calendar, click **+ Create → Appointment schedule**
2. Configure:
   - **Title:** `Tour the Space at [Address]`
   - **Duration:** 30 minutes
   - **Availability:** Mon–Fri, 9am–6pm (or your preferred hours)
   - **Buffer time:** 15 minutes between appointments
   - **Max bookings per day:** 4 (adjust as needed)
   - **Booking window:** Up to 30 days in advance
3. Under **Booking page settings:**
   - Add a booking page title and description (include space highlights)
   - Upload a photo of the space
   - Add the space address so Google Maps shows for attendees
4. Enable **Email confirmations and reminders**:
   - Confirmation: immediately on booking
   - Reminder: 24 hours before
   - Reminder: 1 hour before
5. Copy the **booking page link** — this is your `{{CALENDAR_BOOKING_LINK}}`

---

## Step 3: Connect to Make.com

1. In Make.com, add a **Google Calendar → Watch Events** module in RE-03
2. Select the `Property Showings` calendar
3. Trigger on `Event created`
4. Map fields:
   - `attendee_email` → attendee[0].email
   - `attendee_name` → attendee[0].displayName
   - `start_datetime` → start.dateTime
   - `end_datetime` → end.dateTime
   - `cancel_url` → hangoutLink (or use a custom cancel URL)

---

## Availability Windows (Recommended)

| Day | Hours | Notes |
|-----|-------|-------|
| Monday | 10am – 6pm | Avoid early morning |
| Tuesday | 10am – 6pm | |
| Wednesday | 10am – 6pm | |
| Thursday | 10am – 6pm | |
| Friday | 10am – 4pm | Leave afternoon free |
| Saturday | 10am – 2pm | Optional — for prospects who can't do weekdays |

---

## Manual Booking Fallback

If a prospect replies to an email rather than using the booking link:

1. Manually create a calendar event in the `Property Showings` calendar
2. Add the prospect as an attendee (this triggers the Make.com webhook)
3. Airtable and confirmation email will fire automatically

---

## Notes

- Block off any times you're not available as "Busy" events on the Showings calendar
- Make.com RE-03 fires on **any new event** in this calendar — only use it for showings
- If you use Google Workspace, the appointment page link never expires
