# Hotel Booking Communication Flow
## Santorini Breeze Hotel — Plain-Language Guide

**For:** Nikos, Maria, and the front desk team  
**Last updated:** April 2026

---

## What This Automation Does

This system automatically sends the right email to every guest at the right time — so your team doesn't have to.

When a guest books a room (from any channel), three things happen automatically:

1. **Within minutes of booking:** The guest receives a confirmation email with their room details, dates, and check-in instructions.
2. **Three days before arrival:** The guest receives a pre-arrival email with directions, parking, WiFi, check-in time, and breakfast information.
3. **On checkout day:** The guest receives a correct PDF invoice by email.

When a potential guest fills in the contact form on your website, they receive an automatic reply within seconds, and Maria gets a summary in her inbox.

**Your team does not need to do anything for these to happen.**

---

## What Triggers It

- A new booking confirmed in **Cloudbeds** (from Booking.com, Airbnb, or your website widget)
- A form submission on your **WordPress website** contact form

---

## What It Produces

| Email | Sent To | When |
|-------|---------|------|
| Booking confirmation | Guest | Within minutes of booking |
| Pre-arrival information | Guest | 3 days before check-in, at 09:00 |
| PDF invoice | Guest | Checkout day at 11:00 |
| Inquiry acknowledgement | Potential guest | Within minutes of form submission |
| Inquiry summary | Maria | Within minutes of form submission |

---

## What to Do If Something Goes Wrong

**If a guest says they didn't receive a confirmation email:**
1. Check your Gmail "Sent" folder — search the guest's name
2. If it's not there, go to Make.com → open the scenario → check the last run logs
3. Send the confirmation manually from your Gmail template
4. Contact us if this happens more than once in a week

**If a pre-arrival email wasn't sent:**
1. In Make.com, check the scenario history for that booking date
2. Send the pre-arrival email manually from the template in your Gmail drafts
3. Note the booking ID and contact us

**If an invoice has an error:**
1. Do not resend the automated one — it will send the same error
2. Open the invoice template in Make.com and check the reservation data
3. Correct manually in Excel as a short-term fix and send to the guest
4. Report the error to us with the booking ID

**If you receive an alert email about an error:**
1. Read it — it will tell you exactly which booking failed and what to do
2. Take the manual action described in the alert
3. Contact us if you're unsure

---

## How to Pause or Turn It Off

If you need to stop the automation (for example, during a major system change or hotel closure):

1. Log in to **Make.com** at make.com
2. Go to **Scenarios**
3. Find the scenario called **"Santorini Breeze — Booking Communications"**
4. Click the toggle switch to turn it **OFF** (it will turn grey)
5. Turn it back ON the same way when you're ready

**Important:** While the automation is off, no emails will be sent automatically. Maria must send confirmations, pre-arrivals, and invoices manually.

---

## Contacts

- **Make.com account:** [login credentials stored securely by Nikos]
- **Gmail account used:** [hotel sending address]
- **For automation issues:** [your automation consultant contact]
