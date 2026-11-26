# Architecture

## Runtime path

The supplied n8n export contains three connected workflow paths:

1. **Availability lookup** — `Webhook Check` → `Get Doctors` → `Get Appointments` → `Compute Free Slots` → `Respond Slots`.
2. **Appointment booking** — `Webhook Book` → read doctors and appointments → `Validate Booking` → `Slot OK?`. A valid booking is appended to the appointments sheet, added to Google Calendar, sent through WhatsApp, and returned to the caller. Invalid requests are returned through `Respond Failed`.
3. **Reminders** — a schedule trigger runs every ten minutes between 08:00 and 19:00 according to the cron expression in the export. It reads appointment rows, selects booked appointments approximately 50–65 minutes away whose reminder field is blank, sends a WhatsApp template message, and updates the row after a successful API response.

## Voice interface

The project uses Sarvam AI Indus as the intended voice-agent interface. The n8n code includes request parsing for plain JSON and tool-call-shaped payloads. Verify the exact tool argument schema and response envelope in the Indus agent before deployment; do not assume every voice platform sends identical payloads.

## Design note

The current booking flow checks the slot against the sheet before writing the booking. For production use, protect against two callers booking the same slot concurrently. A spreadsheet read-then-write sequence is not an atomic reservation lock.
