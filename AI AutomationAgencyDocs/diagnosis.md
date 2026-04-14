---
project: Hotel Booking Communication Flow
client: Nikos Papadopoulos — Santorini Breeze Hotel
phase: Diagnosis
date: 2026-04-13
---

# Diagnosis: Hotel Booking Communication Flow

Full transcript of the diagnostic interview conducted with the client, including all four rounds, ROI calculation, and confirmed automation opportunity.

---

## First Contact — Client's Initial Problem Statement

> "Γεια σας! Hello. My name is Nikos. I own a boutique hotel in Santorini. I have 45 rooms, we are very busy in summer — June to September is crazy. My problem is my staff spend too much time on emails. Every booking from Booking.com or direct, they must send a confirmation email, then a welcome email before arrival, then after checkout a thank-you and invoice. Three people doing this all day. I want to automate the emails."

**Consultant note:** The stated problem ("automate the emails") is a solution, not a problem. Before designing anything, we need to understand what's actually happening and where the real cost sits. Moving to Round 1.

---

## Round 1 — The Problem Surface

**Question asked:**
> "Walk me through exactly what happens today when a new booking comes in — step by step, from the moment it appears until the guest gets their first email. Who touches it, what systems are involved?"

**Nikos's answer:**
> "OK so — booking comes in on Booking.com, my receptionist Maria sees it in the morning when she checks. She opens our Excel sheet, copies the guest name, dates, room number. Then she opens Gmail and types the confirmation email — we have a template in Word she copies from. Then she sends. Then 3 days before arrival she must remember to send the pre-arrival email with directions, check-in time, parking info. Then after checkout, she makes the invoice in another Excel file and emails it. Sometimes she forgets the pre-arrival. Sometimes invoice has wrong dates. In peak season we get 8-10 new bookings per day."

**Key findings from Round 1:**
- Process is entirely manual end-to-end — no automation exists at any step
- Maria checks Booking.com manually each morning (no real-time trigger)
- Copy-paste workflow from Excel to Word template to Gmail
- Pre-arrival email relies on Maria's memory — no scheduling mechanism
- Invoice is built manually in a separate Excel file — prone to data entry errors
- Volume: 8–10 bookings/day in peak season across 3 staff members

---

## Round 2 — The Pain

**Question asked:**
> "The pre-arrival email being forgotten stands out to me — what happens when a guest arrives without having received directions or check-in instructions?"

**Nikos's answer:**
> "Ah, this is a big problem. They call the hotel, sometimes 10-15 minutes before check-in, sometimes they get lost. Maria has to stop what she's doing to give directions by phone. In August this happens maybe 3-4 times per week. And the invoice errors — sometimes wrong VAT, wrong dates — we have to redo them, guests complain, one time we had a dispute with a corporate client. Very stressful."

**Key findings from Round 2:**
- 3–4 missed pre-arrival emails per week in peak season → emergency phone calls → staff interruption
- Invoice errors are recurring: wrong VAT, wrong dates — not isolated incidents
- At least one corporate client dispute caused by invoice inaccuracy
- Maria's time during peak check-in hours is being consumed by preventable calls
- Staff stress confirmed — this is a quality-of-work issue, not just an efficiency issue

---

## Round 3 — ROI Quantification

**Question asked:**
> "Let me put a number on this. Roughly, what do you pay Maria and the other two staff members per hour?"

**Nikos's answer:**
> "Maria is senior, she earns about €14/hour. The other two are €11/hour. In summer all three are doing this. In winter only Maria, maybe 2-3 bookings per day."

**ROI Calculation:**

| Component | Calculation | Annual Cost |
|-----------|------------|-------------|
| Peak season comms (Jun–Sep, 17 weeks) | 9 avg bookings/day × 7 days = 63/week · 15 min/booking · 3 staff · avg €12/hr · 17 weeks | ~€3,264 |
| Off-peak comms (35 weeks) | ~18 bookings/week · 15 min/booking · 1 staff (Maria) · €14/hr · 35 weeks | ~€2,205 |
| Error rework (invoice + calls) | 3–4 calls/week peak × 20 min × €14/hr × 17 weeks + invoice redo time | ~€400 |
| **Total** | | **~€5,900/year** |

*Assumptions: 15 min per booking covers confirmation + pre-arrival scheduling + invoice; calls average 20 min; invoice redos averaged at 1/week. Does not count: client relationship cost of corporate dispute, future bookings lost to poor guest experience.*

**Presented to client:**
> "This process is costing roughly €5,900 per year in staff time alone — before counting errors, delays, and client impact."

---

## Round 4 — The Real Problem

**Question asked:**
> "If we fixed the email flow perfectly, is there anything else upstream that would still cause problems? For example — is the data in Booking.com always accurate, or do guests sometimes book with wrong dates, wrong room types?"

**Nikos's answer:**
> "The data from Booking.com is usually fine. The main problem is really the manual steps. Oh — also we have direct bookings from our website with a contact form, not just Booking.com. Maybe 30% of bookings are direct."

**Key findings from Round 4:**
- Data quality in Cloudbeds is reliable — no upstream data problem
- The bottleneck is purely the manual execution of a known, predictable process
- 30% of bookings come through a WordPress contact form — a second trigger source not initially mentioned
- Direct booking inquiries currently land in Maria's inbox with no auto-acknowledgement to the guest

---

## Confirmed Diagnosis Output

```
## Automation Opportunity: Hotel Booking Communication Flow

**Real Problem:**
Zero automation in post-booking workflow — every guest touchpoint 
(confirmation, pre-arrival, invoice) is manual, causing errors, 
forgotten sends, and staff time waste at peak load.

**Annual Cost:** ~€5,900/year in direct staff time + unquantified 
cost of invoice disputes and guest experience failures.
  Assumptions: 9 bookings/day peak (17 weeks), 2.5/day off-peak 
  (35 weeks), 15 min/booking, avg €12/hr blended rate.

**Business Impact:**
3–4 missed pre-arrival emails/week in peak season → emergency 
calls + guest frustration. Invoice errors → corporate client 
disputes. Staff time consumed during highest-load period when 
they are needed for actual hospitality.

**Automation Hypothesis:**
A trigger-based flow that fires on new booking (from Cloudbeds 
channel manager + website contact form), sends a templated 
confirmation immediately, schedules a pre-arrival email 3 days 
before check-in, and generates + sends a correct invoice on 
checkout date — with zero manual steps.
```

**Client confirmation:**
> "Yes, exactly this. This is exactly it."

Diagnosis confirmed. Proceeding to Discovery.
