# Automation Patterns Library

Reference for Phase 4 — Architecture. Every automation is a variation of one or more of these base patterns. Identify the right pattern first, then adapt it to the client's specific requirements. This prevents designing from scratch every time and ensures proven failure modes are accounted for.

---

## Pattern 1: Webhook → Transform → Action

**The most common pattern.** Something happens in one system, data is transformed, and an action is triggered in another.

**When to use:**
- A form is submitted, an order is placed, a record is created
- One system needs to notify or update another in near real-time
- The trigger is event-based, not scheduled

**Flow:**
```
[External Event]
  └─ Webhook received
       └─ Validate payload
            ├─ [Invalid] → Log + Alert → End
            └─ [Valid] → Transform data
                              └─ Call target system
                                    ├─ [Success] → Log → End
                                    └─ [Failure] → Retry (3×) → Dead-letter queue → Alert
```

**Key failure modes:**
- Webhook received but payload is malformed → validate before processing
- Target system is down → retry with exponential backoff, queue if still failing
- Duplicate webhooks → check for idempotency key before processing
- Webhook volume spike → rate limit or queue to avoid overwhelming target

**Typical stack:** Zapier / Make / n8n for simple cases. Custom code + queue (Redis, SQS) for high volume or reliability requirements.

---

## Pattern 2: Scheduled Polling

**For when the source system doesn't support webhooks.** A cron job checks for new or changed records on a schedule.

**When to use:**
- Source system has no webhook support (older CRMs, spreadsheets, legacy tools)
- Batch processing is acceptable (check every 15 min, hourly, daily)
- You're pulling reports or exports rather than reacting to events

**Flow:**
```
[Cron Trigger — every X minutes/hours]
  └─ Fetch records from source
       └─ Filter: new or changed since last run?
            ├─ [Nothing new] → Log → End
            └─ [New records] → Process each record
                                    └─ [For each] → Transform → Act
                                                         ├─ [Success] → Mark processed → Log
                                                         └─ [Failure] → Log error → Continue next record
  └─ Update "last run" timestamp
```

**Key failure modes:**
- Fetching all records every time (expensive, slow) → store and check a "last processed" cursor
- Missing records if the run fails mid-batch → use a queue or mark records as processed individually
- Clock drift causing missed or duplicate processing → use record timestamps, not just run timestamps
- Rate limits on the source API → add delays between requests

**Typical stack:** Make or n8n for scheduled scenarios. Python with APScheduler or cron for custom needs.

---

## Pattern 3: Event-Driven Pipeline

**A chain of steps where each step produces output that triggers the next.** More complex than webhook → action, with multiple processing stages.

**When to use:**
- Data needs to pass through several transformations or enrichments
- Each stage can fail independently and should be retried independently
- Multiple systems are involved in sequence

**Flow:**
```
[Trigger]
  └─ Stage 1: Ingest + Validate
       └─ Stage 2: Enrich / Transform
            └─ Stage 3: Decision / Routing
                 ├─ [Route A] → Stage 4a: Action A
                 └─ [Route B] → Stage 4b: Action B
                                    └─ Stage 5: Notify + Log
```

**Key failure modes:**
- A middle stage fails and poisons the whole pipeline → make each stage independently retryable
- Stages run at different speeds → use a queue between stages for decoupling
- Hard to debug which stage failed → log a unique ID at each stage so you can trace a record through

**Typical stack:** n8n or custom Python with a message queue (Redis, RabbitMQ) for resilience. Make for simpler linear versions.

---

## Pattern 4: Approval Workflow (Human in the Loop)

**Automation handles the preparation; a human makes a decision; automation handles the execution.** Keeps humans in control of important decisions while removing manual setup work.

**When to use:**
- The automation produces an output that requires human sign-off before acting
- High-stakes actions (sending to clients, publishing content, approving payments)
- The client isn't ready to fully automate a decision yet

**Flow:**
```
[Trigger]
  └─ Prepare package (draft, summary, data)
       └─ Send for approval (email, Slack, form)
            └─ Wait for response
                 ├─ [Approved] → Execute action → Notify → Log
                 ├─ [Rejected] → Log reason → Notify originator → End
                 └─ [No response after X hours] → Escalate → Alert → End
```

**Key failure modes:**
- Approval sits unanswered indefinitely → set a timeout with escalation
- Approver approves twice → idempotency check before executing
- Approver is on leave → route to backup approver
- No audit trail of who approved what → log approver identity and timestamp

**Typical stack:** Slack/email approval steps in Make or n8n. Custom approval UI for complex cases.

---

## Pattern 5: Data Sync

**Keep two or more systems in sync.** Changes in one system propagate to the other, with conflict resolution when both change simultaneously.

**When to use:**
- Two systems need to share the same data (CRM + spreadsheet, two CRMs, CRM + billing)
- Manual copy-paste between systems is causing errors or delays

**Flow:**
```
[Change detected in System A]
  └─ Fetch current state from System B
       └─ Compare records
            ├─ [No conflict] → Apply change to System B → Log
            └─ [Conflict — both changed] → Apply conflict rule
                                                ├─ [A wins] → Overwrite B → Log
                                                ├─ [B wins] → Revert A → Log
                                                └─ [Manual review] → Flag for human → Pause sync
```

**Key failure modes:**
- Sync loop: A updates B, B change triggers A update, infinitely → detect and break loops with a "sync in progress" flag
- Conflict resolution is wrong for the business → confirm the rule during Discovery
- Large initial sync overwhelms APIs → throttle and batch
- Partial sync leaves systems in inconsistent state → use transactions or rollback

**Typical stack:** n8n or custom code for reliable bidirectional sync. Make for simpler one-directional sync.

---

## Pattern 6: Enrichment Pipeline

**Take raw input data and add information from external sources before storing or acting on it.**

**When to use:**
- New leads need company data appended (from Clearbit, Apollo, LinkedIn)
- Addresses need validation or geocoding
- Records need classification or scoring before routing

**Flow:**
```
[New record created]
  └─ Queue for enrichment
       └─ Call Enrichment Source 1 (e.g. company data)
            └─ Call Enrichment Source 2 (e.g. email validation)
                 └─ Merge results onto record
                      └─ Apply scoring / classification logic
                           └─ Route or store enriched record
```

**Key failure modes:**
- Enrichment API returns nothing → handle gracefully, store partial data, don't block the pipeline
- Enrichment API is slow → run enrichment steps in parallel where possible
- API cost per call adds up → check if enrichment is needed (e.g. don't re-enrich existing contacts)
- Rate limits → queue and throttle

**Typical stack:** n8n or custom Python. Make for simpler cases with fewer sources.

---

## Pattern 7: Monitor & Alert

**Watch a system or metric and notify the right person when something goes outside normal bounds.**

**When to use:**
- The client needs to know when something goes wrong before a customer tells them
- A metric (inventory, revenue, error rate) needs a threshold-based alert
- Periodic health checks on a system or integration

**Flow:**
```
[Scheduled Trigger]
  └─ Fetch current metric / state
       └─ Compare to threshold / expected state
            ├─ [Normal] → Log → End
            └─ [Anomaly] → Determine severity
                               ├─ [Warning] → Notify via Slack/email
                               └─ [Critical] → Page on-call → Create incident → Escalate
```

**Key failure modes:**
- Alert storms: every run triggers an alert → add a "cooldown" period, only alert once per incident
- Flapping: metric bounces around the threshold → require N consecutive anomalies before alerting
- Alert fatigue: too many low-value alerts → tune thresholds or add severity tiers
- The monitor itself fails silently → add a "dead man's switch" — alert if no successful run in X hours

**Typical stack:** n8n or Make for scheduled checks. Custom code for complex anomaly detection.

---

## Pattern 8: Document Processing (AI Extraction)

**Use AI to extract structured data from unstructured documents — emails, PDFs, invoices, contracts, support tickets.**

**When to use:**
- Humans are reading documents and manually entering data
- Data comes in via email or PDF with no API
- Content needs classification, routing, or summarisation

**Flow:**
```
[Document arrives — email, upload, or webhook]
  └─ Extract raw text (PDF parser, email body)
       └─ Send to Claude API with extraction prompt
            └─ Parse structured output (JSON)
                 └─ Validate extracted fields
                      ├─ [Confidence high] → Write to system → Log
                      └─ [Confidence low / missing fields] → Flag for human review
                                                                  └─ Human corrects → Write to system
```

**Key failure modes:**
- AI extracts wrong data confidently → always validate required fields, check data types and ranges
- Document format varies → test on a wide sample before going live, handle format exceptions
- AI output is not valid JSON → wrap in try/catch, log raw output for debugging
- Cost per document is too high → batch documents, cache results for duplicates, use cheaper model for simple extractions

**Prompt engineering tips for Claude API:**
- Ask for JSON output explicitly with a defined schema
- Include 1–2 examples of good extraction in the prompt (few-shot)
- Ask Claude to indicate confidence or flag uncertain fields
- Validate the output programmatically before trusting it downstream

**Typical stack:** Claude API + Python or n8n. PDF parsing with PyMuPDF, pdfplumber, or Adobe API.

---

## Combining Patterns

Most real automations combine 2–3 patterns. Common combinations:

| Combination | Example |
|---|---|
| Webhook + Enrichment + Action | New lead arrives → enrich with company data → create CRM record |
| Polling + Document Processing + Sync | Check email hourly → extract invoice data → write to accounting system |
| Webhook + Approval + Action | Support ticket arrives → AI drafts reply → human approves → reply sent |
| Scheduled + Monitor + Alert | Check inventory nightly → flag low stock → alert purchasing team |
| Event-Driven + Enrichment + Approval | Order placed → verify address → flag high-value orders for review → fulfil |
