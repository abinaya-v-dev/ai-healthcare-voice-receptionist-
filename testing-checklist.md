# Testing checklist

Use a test spreadsheet, test calendar, and WhatsApp test setup. Use fictional patient records only.

## Availability

- [ ] Valid future date returns doctors who work that day.
- [ ] A booked slot is excluded from free slots.
- [ ] A past date is rejected.
- [ ] Earlier times on the current IST date are excluded.
- [ ] Unknown doctor name does not silently switch to another doctor.
- [ ] A doctor not scheduled on the requested weekday is reported as unavailable.
- [ ] Malformed date/time input returns a clear error.

## Booking

- [ ] Missing patient name is rejected.
- [ ] Invalid phone number is rejected.
- [ ] Past date is rejected.
- [ ] A time outside the doctor's working hours is rejected.
- [ ] A slot already booked is rejected.
- [ ] Successful booking appends one appointment row.
- [ ] Successful booking creates one calendar event.
- [ ] WhatsApp success is reflected in the response.
- [ ] WhatsApp failure does not falsely claim that a message was sent.
- [ ] Retrying the same request does not create duplicate bookings (this needs an idempotency enhancement).

## Reminders

- [ ] Only booked appointments are considered.
- [ ] Only appointments in the configured reminder window are selected.
- [ ] Invalid phone numbers are skipped.
- [ ] Reminder status is updated only after a successful WhatsApp API response.
- [ ] A second scheduler run does not send the same reminder again.

## Before production

- [ ] Add atomic slot locking / concurrency protection.
- [ ] Add webhook authentication.
- [ ] Add request validation and rate limiting.
- [ ] Review privacy, consent, retention, and access controls.
- [ ] Confirm WhatsApp templates and platform policies.
- [ ] Test failure paths and alerting.
