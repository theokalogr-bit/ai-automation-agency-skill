---
project: Hotel Booking Communication Flow
client: Nikos Papadopoulos — Santorini Breeze Hotel
phase: Discovery
date: 2026-04-13
---

# Discovery Summary: Hotel Booking Communication Flow

## Trigger
- **Primary:** New reservation created in Cloudbeds (from Booking.com, Airbnb, or website booking widget) — fires a webhook
- **Secondary:** Contact Form 7 submission on WordPress website (direct booking inquiry from potential guest)

## Inputs
- **Cloudbeds reservation payload:** guest name, email, phone, room type, check-in date, check-out date, nightly rate, extras (breakfast included Y/N), booking source, booking ID
- **Contact Form 7 payload:** guest name, email, requested dates, room preference, message text

## Outputs
1. **Confirmation email** — sent immediately on booking confirmation
2. **Pre-arrival email** — sent 3 days before check-in at 09:00 local time (includes directions, parking, check-in time, WiFi, breakfast info, emergency contact)
3. **Invoice PDF** — generated and emailed on checkout day at 11:00 (bilingual EN/GR for corporate clients, 13% VAT, hotel logo, correct room/rate/extras breakdown)
4. **Inquiry auto-reply** — immediate acknowledgement to guest for Contact Form 7 submissions
5. **Inquiry forward** — formatted summary to Maria's Gmail for human follow-up
6. **Inquiry log** — row added to Google Sheet (date, name, email, dates, status)

## Stack (Current)
| Tool | Purpose |
|------|---------|
| Cloudbeds | PMS + channel manager (connects Booking.com, Airbnb, website widget) |
| Gmail | All outbound guest communication |
| WordPress + Contact Form 7 | Direct booking inquiry form |
| Excel | Current invoicing (manual, error-prone — to be replaced) |

## Volume & Frequency
| Period | Daily Avg | Weekly Avg |
|--------|-----------|------------|
| Peak season (Jun–Sep, 17 weeks) | ~9 bookings/day | ~63/week |
| Off-peak (35 weeks) | ~2.5 bookings/day | ~18/week |
| Direct bookings (Contact Form 7) | ~30% of total across all periods |

## People Involved
- **Maria** (senior receptionist) — owns the entire communication process today; primary point of contact
- **2 additional staff** — assist in peak season with manual sends
- **Nikos** (owner) — escalation point for disputes; approves any system access changes

## Prior Attempts
None. The process is entirely manual. No automation has been attempted previously.

## Success Criteria
- Zero manual sends required for confirmation and pre-arrival emails
- Zero missed pre-arrival emails in peak season
- Invoice errors eliminated — correct VAT (13%), correct dates, correct guest details, every time
- Maria's routine communication workload reduced by ~80% in peak season
- No guests arriving without having received pre-arrival information
