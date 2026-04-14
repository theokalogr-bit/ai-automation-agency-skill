# Handover Checklist: Hotel Booking Communication Flow
## Santorini Breeze Hotel

Complete every item in order before going live. Check each box when done.

---

## 1. Accounts & Subscriptions

- [ ] **Make.com account created** — go to make.com, sign up with hotel email
  - Recommended plan: Core (~€16/month) — covers the volume needed
  - Free trial available for first 30 days
- [ ] **Make.com team access** — invite Nikos as admin so he has full control

---

## 2. Cloudbeds Setup

- [ ] **API access enabled** — log into Cloudbeds → Settings → API → generate API key
  - If not available, contact Cloudbeds support to enable API access on your plan
- [ ] **Webhook configured** — in Make.com, create the webhook URL and paste it into Cloudbeds webhook settings (Settings → Webhooks → New Reservation)
- [ ] **Test booking created** — make a test reservation in Cloudbeds and confirm the webhook fires in Make.com (check the "Run history" tab)

---

## 3. Gmail Connection

- [ ] **Gmail OAuth connected in Make.com** — in Make.com, go to Connections → Add → Gmail → sign in with hotel Gmail account
  - No password is shared — uses Google's secure OAuth login
- [ ] **Test send verified** — send a test email from Make.com via the Gmail connection and confirm it arrives

---

## 4. Email Templates

- [ ] **Confirmation email template finalised** — review draft, confirm hotel logo displays, cancellation policy is current, contact details are correct
- [ ] **Pre-arrival email template finalised** — directions verified, parking info correct, WiFi password current, breakfast times correct
- [ ] **Both templates reviewed by Nikos** before go-live

---

## 5. Invoice Template

- [ ] **Hotel legal name confirmed** (as registered with tax authority)
- [ ] **VAT number (ΑΦΜ) entered** in invoice template
- [ ] **Registered address confirmed** and entered
- [ ] **Hotel logo uploaded** (PNG or SVG, transparent background)
- [ ] **Invoice number sequence confirmed** — starting number for 2026 agreed
- [ ] **Bilingual layout reviewed** — English and Greek sections display correctly
- [ ] **Test invoice generated** from a test booking — VAT calculation verified (13%)

---

## 6. Contact Form 7 (WordPress)

- [ ] **CF7 Webhook plugin installed** on WordPress (or equivalent — "CF7 to Webhook" or "Contact Form 7 – Webhooks")
- [ ] **Webhook URL from Make.com** pasted into CF7 plugin settings
- [ ] **Test form submission** completed — confirm Make.com receives it, auto-reply arrives, Maria receives summary

---

## 7. Google Sheets Log

- [ ] **Google Sheet created** for inquiry log (columns: Date, Guest Name, Email, Requested Dates, Room Preference, Message, Status)
- [ ] **Google Sheets connection** added in Make.com (Google OAuth)
- [ ] **Test row appended** from a test form submission — confirm data is correct

---

## 8. Error Alerts

- [ ] **Alert email addresses confirmed** — Maria's email and Nikos's email are set as alert recipients in Make.com error handlers
- [ ] **Test error triggered** — temporarily break a field to confirm alert fires correctly, then restore

---

## 9. Final Testing (Before Go-Live)

- [ ] **Happy path test:** Create a real test booking in Cloudbeds → confirm email arrives → wait for (or manually trigger) pre-arrival → trigger invoice → all three received and correct
- [ ] **Late booking test:** Create a booking for 2 days from now → confirm pre-arrival sends immediately rather than scheduling
- [ ] **Corporate client test:** Create booking with company name populated → confirm bilingual PDF generated
- [ ] **Form inquiry test:** Submit contact form → confirm auto-reply and Maria forward both arrive → confirm Google Sheet row added
- [ ] **Error test:** Remove guest email from test booking → confirm alert fires → restore and retest

---

## 10. Go-Live

- [ ] **Nikos signs off** on all test results
- [ ] **Make.com scenario turned ON** (toggle switch to active/green)
- [ ] **First live booking processed** — confirm all emails sent without manual intervention
- [ ] **Team briefed** — Maria and staff shown the README, shown how to pause the automation if needed
- [ ] **30-day check-in scheduled** — calendar invite for review call one month after go-live

---

## Who to Contact If Something Breaks

| Issue | Contact |
|-------|---------|
| Make.com not running | Check Make.com status page, then contact automation consultant |
| Cloudbeds webhook stopped firing | Cloudbeds support + automation consultant |
| Gmail OAuth expired | Re-authenticate in Make.com → Connections |
| Invoice error | Do not re-run automation — fix manually, report to consultant with booking ID |
