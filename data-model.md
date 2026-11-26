# Data model

## Doctors tab

| Column | Purpose |
|---|---|
| `doctor_id` | Stable unique doctor identifier |
| `doctor_name` | Display name |
| `department` | Department/specialty label |
| `days` | Comma-separated working days (for example `Mon,Tue,Wed`) |
| `start` | Working-day start time |
| `end` | Working-day end time |
| `slot_min` | Appointment slot length in minutes |

## Appointments tab

| Column | Purpose |
|---|---|
| `appt_id` | Unique appointment identifier |
| `patient_name` | Patient's name |
| `phone` | Mobile number used for contact |
| `reason` | Patient-provided appointment reason |
| `doctor_id` | Selected doctor identifier |
| `doctor_name` | Doctor display name |
| `department` | Department |
| `date` | Appointment date |
| `time` | Appointment start time |
| `end_time` | Appointment end time |
| `status` | Booking state; availability logic counts rows marked `booked` |
| `reminder_sent` | Reminder state; blank means not yet sent |

The precise columns should be checked against the spreadsheet used by the owner before import. Do not put real patient data in this public repository.
