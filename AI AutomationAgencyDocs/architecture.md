---
project: Hotel Booking Communication Flow
client: Nikos Papadopoulos — Santorini Breeze Hotel
phase: Architecture
date: 2026-04-13
---

# Automation Architecture: Hotel Booking Communication Flow

## Trigger A — Cloudbeds New Reservation Webhook
A webhook fires from Cloudbeds the moment a new reservation is confirmed on any connected channel (Booking.com, Airbnb, or website widget). Cloudbeds sends the full reservation payload to a Make.com webhook URL.

## Trigger B — Contact Form 7 Submission
WordPress fires a webhook to Make.com when a visitor submits the direct booking inquiry form. Requires the "CF7 to Webhook" plugin or equivalent to forward form data.

---

## Flow — Trigger A: Confirmed Reservation

### Step 1: Receive + Validate Payload
- Make.com receives Cloudbeds webhook
- Validate required fields: guest email (present and valid format?), check-in date (future date?), check-out date (after check-in?), room type (known value?)
- **If invalid:** Log error with booking ID → Send alert email to Maria → End
- **If valid:** Continue to Step 2

### Step 2: Send Confirmation Email (Immediate)
- Via Gmail (hotel sending address)
- Template includes: guest first name, hotel name, room type, check-in date, check-out date, total nights, rate per night, breakfast inclusion, cancellation policy, hotel address, direct phone number, booking reference
- **If send fails:** Retry once after 5 min → If still fails: alert Maria with booking details
- Log: `CONFIRMATION_SENT | booking_id | guest_email | timestamp`

### Step 3: Schedule Pre-Arrival Email
- Calculate target send date: check-in date minus 3 days, at 09:00 Europe/Athens
- **If check-in is within 3 days (late bookings):** Send pre-arrival immediately instead
- Use Make.com scheduler/delay module to hold until target datetime
- Store: booking_id, guest_email, guest_name, check-in date, room type

### Step 4: Send Pre-Arrival Email (D-3, 09:00)
- Template includes: guest first name, check-in date, check-in time (15:00), directions from Santorini port and airport, parking information, WiFi network + password, breakfast time and location, local emergency contact number, weather note for arrival day (optional)
- **If send fails:** Retry once → If still fails: alert Maria with guest contact details to send manually
- Log: `PRE_ARRIVAL_SENT | booking_id | guest_email | timestamp`

### Step 5: Schedule Invoice Generation
- Calculate target: checkout date at 11:00 Europe/Athens (post-checkout time)
- Store full reservation data needed for invoice: guest name, address (if provided), room type, check-in, check-out, nights count, nightly rate, breakfast charges, subtotal, VAT (13%), total
- Detect corporate flag: if Cloudbeds "company name" field is populated → set language = EN + GR; otherwise → EN only

### Step 6: Generate + Send PDF Invoice (Checkout Day, 11:00)
- Pull final reservation data from Cloudbeds API (to capture any modifications made during stay)
- Populate HTML invoice template with all fields
- Calculate: (nights × rate) + (breakfast days × breakfast rate) = subtotal; subtotal × 1.13 = total; subtotal × 0.13 = VAT amount
- Generate invoice number: `SB-[YEAR]-[sequential_number]` (e.g. SB-2026-0247)
- Convert HTML to PDF via Make.com PDF module or external HTML-to-PDF service
- Attach PDF to Gmail and send to guest
- **If PDF generation fails:** Retry once → If still fails: send plain-text invoice body and alert Maria to send PDF manually
- **If send fails:** Retry once → Alert Maria with guest email and invoice data
- Log: `INVOICE_SENT | booking_id | invoice_number | guest_email | total_eur | timestamp`

---

## Flow — Trigger B: Contact Form 7 Inquiry

### Step 1: Receive Form Data
- Make.com receives: name, email, requested dates, room preference, message

### Step 2: Send Auto-Reply to Guest (Immediate)
- Template: "Thank you [name], we've received your inquiry and will reply within 4 hours. For urgent enquiries call [phone]."
- Log: `INQUIRY_AUTOREPLY | guest_email | timestamp`

### Step 3: Forward to Maria (Immediate)
- Formatted email to Maria's Gmail with: guest name, email, requested dates, room preference, message, direct reply link
- Subject: `New Booking Inquiry — [Guest Name] — [Dates]`

### Step 4: Log to Google Sheet
- Append row: date received, guest name, email, requested dates, room preference, status = "Pending"

---

## Error Handling Summary

| Scenario | Handling |
|----------|----------|
| Webhook not received | Make.com auto-retries 3×; then alerts Nikos by email |
| Guest email missing/invalid | Immediate alert to Maria with booking ID |
| Confirmation email bounce | Log bounce + alert Maria with guest phone number |
| Pre-arrival email fails all retries | Alert Maria with guest contact details 4 days before arrival |
| PDF generation fails | Retry once → plain-text fallback → alert Maria |
| Invoice email bounce | Alert Maria + Nikos; log for manual follow-up |
| Cloudbeds API unavailable at invoice time | Queue and retry in 15 min × 3; then alert |
| Duplicate webhook (same booking_id) | Check booking_id log; skip if already processed (idempotent) |

---

## Visual Flow Diagram

```
[TRIGGER A: Cloudbeds Webhook — New Reservation]
  └─ Step 1: Validate payload
       ├─ [Missing/invalid fields] → Log error → Alert Maria → END
       └─ [Valid]
            └─ Step 2: Send Confirmation Email (NOW)
                 ├─ [Fail] → Retry 1× → Alert Maria → END
                 └─ [Success] → Log
                      └─ Step 3: Schedule Pre-Arrival
                           ├─ [Check-in ≤ 3 days] → Send immediately
                           └─ [Check-in > 3 days] → Wait until D-3 09:00
                                └─ Step 4: Send Pre-Arrival Email
                                     ├─ [Fail] → Retry 1× → Alert Maria → END
                                     └─ [Success] → Log
                                          └─ Step 5: Schedule Invoice (checkout day 11:00)
                                               └─ Step 6: Generate PDF Invoice
                                                    ├─ [PDF fail] → Retry → Plain-text fallback → Alert Maria
                                                    ├─ [Send fail] → Retry 1× → Alert Maria
                                                    └─ [Success] → Log → END

[TRIGGER B: Contact Form 7 Submission]
  └─ Step 1: Send Auto-Reply to Guest (NOW)
       └─ Step 2: Forward Inquiry to Maria (NOW)
            └─ Step 3: Log to Google Sheet → END
```

---

## Assumptions
- Cloudbeds webhook can be configured and activated (requires Cloudbeds API access — may need support request)
- Hotel uses a single fixed Gmail address for all guest communications
- Corporate clients are identified by the "company name" field being populated in Cloudbeds
- Check-out time is always 11:00, check-in always 15:00
- VAT rate is 13% (Greek accommodation tax) — must be updated in template if rate changes
- Invoice numbering is sequential and resets annually
- WordPress has internet access to fire outbound webhooks

## Open Questions
- Does Nikos want seasonal email template variations (summer tone vs. winter)?
- What is the invoice number format and starting sequence for 2026?
- Does the Greek tax authority (myDATA) integration need to be addressed in this phase?
- Should Airbnb guests receive different communication (Airbnb has messaging restrictions)?
- Is there a preferred HTML-to-PDF service, or should Make.com's native module be used?
