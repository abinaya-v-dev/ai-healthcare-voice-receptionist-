# Setup guide

## Prerequisites

- n8n instance
- Sarvam AI Indus account and configured voice agent
- Google account with a spreadsheet and calendar
- WhatsApp Business Platform / Cloud API setup with an approved message template
- Access to configure webhook tools in the voice agent

## 1. Prepare Google Sheets

Create a spreadsheet with two tabs named exactly `Doctors` and `Appointments`.

Suggested `Doctors` columns:

`doctor_id`, `doctor_name`, `department`, `days`, `start`, `end`, `slot_min`

Suggested `Appointments` columns:

`appt_id`, `patient_name`, `phone`, `reason`, `doctor_id`, `doctor_name`, `department`, `date`, `time`, `end_time`, `status`, `reminder_sent`

Use sample/fictitious records while testing. The workflow expects the column names used by its JavaScript code.

## 2. Import the n8n workflow

1. Import `workflow/hospital-receptionist-sanitized.json` into a test n8n workspace.
2. Reconnect Google Sheets credentials on every Google Sheets node.
3. Replace `YOUR_GOOGLE_SHEET_ID` with your test spreadsheet ID in each Sheets node.
4. Reconnect Google Calendar credentials and select your test calendar; replace the calendar placeholder.
5. Configure WhatsApp HTTP Header Auth using an n8n credential. Do not paste the token into a node parameter or commit it.
6. Replace `YOUR_WHATSAPP_PHONE_NUMBER_ID` in the WhatsApp Graph API URLs with your own phone-number ID.
7. Verify the approved WhatsApp template name and language in your WhatsApp Business account.
8. Keep the workflow inactive until each integration has been tested in a non-production environment.

## 3. Configure the voice agent

Create tools that call the n8n webhook paths:

- `POST /webhook/check-availability`
- `POST /webhook/book-appointment`

Map the voice-agent arguments to the fields expected by the workflow. The booking code reads patient name, phone, reason, date, time, and doctor/dept selection. Availability uses date, optional time, and doctor/dept selection.

The exported JavaScript contains compatibility handling for a few tool-call payload shapes, but the exact Sarvam Indus request and response format must be confirmed from your agent's tool configuration. Test a real tool call and inspect the n8n execution before enabling calls for users.

## 4. Time zone and reminders

The workflow's date/time calculations use India Standard Time (UTC+05:30) logic in the code. Confirm your n8n instance timezone and schedule-trigger timezone agree with your deployment requirements.

## 5. Deployment checklist

- Use a dedicated test spreadsheet and calendar first.
- Use fictitious patient details only during development.
- Confirm a booking is written once, and only once.
- Confirm the calendar event has the correct start/end.
- Confirm WhatsApp success and failure are both handled.
- Confirm a reminder is sent only once.
- Add webhook authentication or signature validation before exposing endpoints publicly.
