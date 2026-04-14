# Monitoring Guide: Hotel Booking Communication Flow
## Santorini Breeze Hotel

---

## Key Metrics to Watch

| Metric | What It Means | How to Check |
|--------|---------------|--------------|
| **Emails sent per day** | Automation is running | Make.com → Run History → count successful operations |
| **Error rate** | How often something fails | Make.com → Run History → filter by "Error" status |
| **Pre-arrival send accuracy** | Are scheduled emails firing on time? | Cross-check Make.com logs vs. upcoming check-ins |
| **Invoice generation success rate** | PDFs created without failure | Make.com → Run History → Step 6 success count |
| **Inquiry response time** | Auto-reply latency | Test form submission and time the auto-reply |

---

## What Normal Looks Like

**Peak season (Jun–Sep):**
- 8–10 successful scenario runs per day (one per new booking)
- 0–1 errors per week is acceptable (transient network issues, one-off bad data)
- All pre-arrival emails in Make.com "Run History" should show "Success"

**Off-peak:**
- 2–4 successful runs per day
- Near-zero errors expected

---

## Failure Signals — Act Immediately

| Signal | What It Likely Means |
|--------|----------------------|
| **Zero runs in 24 hours** (during booking season) | Cloudbeds webhook is broken, or Make.com scenario is turned off |
| **Error rate > 3 per week** | Systematic issue — API change, credential expired, template broken |
| **Guest complains about no confirmation** | Check if their booking ID is in Make.com run history |
| **Guest arrives without pre-arrival email** | Scheduled send failed — check Make.com history for that booking date |
| **Invoice complaints about wrong amount** | VAT calculation error or rate data mismatch — review template immediately |
| **Gmail OAuth error in Make.com** | Google token expired — re-authenticate the Gmail connection |
| **Cloudbeds connection error** | API key may have been regenerated — update in Make.com immediately |

---

## Alerting Setup

The automation already sends email alerts to Maria and Nikos when errors occur. Additionally, configure the following in Make.com:

### Recommended Make.com Alerts
1. **Scenario error notification:** Make.com → Scenario → Settings → enable "Send notification on error" to Maria's email
2. **Incomplete execution alert:** Enable "Send notification on incomplete execution" — fires when a run stops mid-flow
3. **Monthly usage report:** Make.com sends a usage email — review to ensure you're within your plan's operation limits

### Additional Monitoring (Optional but Recommended)
- Set a **weekly calendar reminder** (Monday morning) to open Make.com and check the past week's run history — 5 minutes is enough
- In peak season, check **daily** for the first two weeks after launch

---

## 30-Day Check-In Agenda

Schedule a call 30 days after go-live to review:

- [ ] **Volume vs. estimate:** How many bookings processed? Match expected rate?
- [ ] **Error review:** Any recurring errors? What caused them?
- [ ] **Pre-arrival accuracy:** Any missed sends caught in the logs?
- [ ] **Invoice accuracy:** Any complaints or corrections needed?
- [ ] **Team feedback:** What are Maria and staff finding smooth or frustrating?
- [ ] **Template updates needed:** Any seasonal changes to pre-arrival content?
- [ ] **Edge cases not anticipated:** Anything the automation didn't handle well?
- [ ] **What to tune:** Rate limits, scheduling times, template wording

---

## When to Call for Help (Re-Engagement Thresholds)

Reach out to your automation consultant if:

- Error rate exceeds **5 errors in a single week** and you can't identify the cause
- Any guest receives a **duplicate email** (idempotency failure)
- An invoice is sent with **incorrect VAT or totals** and you don't know why
- Cloudbeds or Make.com announces **API changes** — these can silently break automations
- You want to **add Airbnb in-app messaging, cancellation flows, or myDATA integration** — these require new builds
- Your booking **volume grows significantly** and you're approaching Make.com plan limits

---

## Make.com Plan Limits Reference

| Plan | Monthly Operations | Cost (approx.) |
|------|-------------------|----------------|
| Free | 1,000 | €0 |
| Core | 10,000 | €16/month |
| Pro | 20,000 | €29/month |

At peak volume (~63 bookings/week × 6 operations/booking = ~378 ops/week × 17 weeks ≈ 6,426 ops/season), the **Core plan is sufficient** for the year. Upgrade to Pro if volume grows or new automations are added.
