# Stack Selection Guide

Reference for Phase 3 — Stack Selection. Use this to recommend the right tool based on the client's context, technical level, volume, and existing stack.

---

## Quick Decision Tree

1. **Does the client already use a tool that can handle this?** → Start there. Adoption cost is zero.
2. **Is the team non-technical and needs to maintain this themselves?** → Zapier or Make.
3. **Does the automation need AI processing (understanding text, extracting data, making decisions)?** → Claude API, possibly combined with n8n or Make.
4. **Is volume high (1000+ runs/day) or cost at scale a concern?** → n8n self-hosted or custom code.
5. **Does it require complex custom logic, databases, or code that no-code tools can't express?** → Python/Node.js or n8n.
6. **Is it a simple linear trigger → action with no custom logic?** → Zapier or Make.

---

## Tool Profiles

### Zapier

**Best for:** Simple trigger → action automations. Teams with no technical staff. Maximum integrations (7,000+).

**Strengths:**
- Easiest setup — most people can configure it in an hour
- Largest integration library
- Reliable uptime, managed infrastructure
- Good support and documentation

**Weaknesses:**
- Expensive at scale (tasks-based pricing adds up fast)
- Limited custom logic — complex branching requires workarounds
- No self-hosting option
- Hard to version control or review as code

**Pricing signal:** If volume exceeds ~2,000 tasks/month, run the numbers against Make or n8n.

**Ideal client:** Small business, non-technical team, <500 runs/day, existing Zapier subscription.

---

### Make (formerly Integromat)

**Best for:** Visual workflow design with more power than Zapier. Complex multi-step flows. Teams comfortable with a learning curve.

**Strengths:**
- Visual scenario builder that's easy to explain to clients
- Much cheaper than Zapier at volume (operations-based, not task-based)
- Strong data transformation and iteration tools
- Handles complex branching, filters, and routers natively

**Weaknesses:**
- Steeper learning curve than Zapier
- Fewer native integrations (but HTTP module covers most gaps)
- Not self-hostable

**Pricing signal:** For moderate complexity + moderate volume, Make is typically 5–10× cheaper than Zapier.

**Ideal client:** Operations-focused team, moderate technical comfort, 500–5,000 runs/day, budget-conscious.

---

### n8n

**Best for:** Developer-friendly automations. Complex logic. Self-hosted deployments where data privacy matters.

**Strengths:**
- Open source — self-host for near-zero per-run cost
- Write JavaScript directly inside nodes for custom logic
- Version control friendly (JSON-based workflow definitions)
- Strong AI/LLM integrations built-in
- Data stays on your infrastructure (GDPR, HIPAA friendly)

**Weaknesses:**
- Requires a server to self-host (Railway, Render, or VPS)
- Higher setup overhead than Zapier/Make
- Smaller integration library (though HTTP and JS nodes fill gaps)
- Client must be comfortable managing a hosted instance, or you host it for them

**Pricing signal:** Cloud plan is reasonable; self-hosted is free. Best economics for high-volume or data-sensitive clients.

**Ideal client:** Tech-savvy team or developer-maintained, high volume, data privacy concerns, complex logic needs.

---

### Custom Python or Node.js

**Best for:** Maximum flexibility. When no-code tools genuinely can't express the logic. When the automation is mission-critical and needs full observability.

**Strengths:**
- No constraints on logic, data structures, or integrations
- Full control over error handling, retries, logging
- Can be tested like any other code
- Version controlled, code reviewed, CI/CD deployable

**Weaknesses:**
- Highest maintenance burden — needs a developer to own it
- Slower to build than no-code tools
- Infrastructure required (server, cron, queue, or serverless)
- Not manageable by non-technical clients without tooling

**Use when:**
- The automation requires custom algorithms or data processing
- No-code tools have hit their limits or workarounds become unwieldy
- The client has a dev team who will own maintenance
- The automation is business-critical enough to warrant real engineering

**Ideal client:** Has a developer or dev team, complex requirements, mission-critical workflow.

---

### Claude API (Anthropic)

**Best for:** Any step in an automation that requires understanding language, extracting information, classifying content, drafting text, or making judgment-based decisions.

**Strengths:**
- Understands unstructured text (emails, PDFs, support tickets, forms)
- Can extract structured data from messy inputs
- Can classify, route, or triage content without brittle rule sets
- Can draft responses, summaries, or documents
- Combines with any other tool in the stack

**Weaknesses:**
- Adds per-token cost — design prompts carefully
- Non-deterministic — outputs vary slightly; build validation on top
- Latency (1–5s per call) — not suitable for sub-second real-time needs
- Requires prompt engineering to get reliable structured output

**When to include Claude API:**
- Input data is unstructured (free text, emails, documents)
- A human is currently reading and routing/responding to content
- Classification or extraction rules would be too brittle to hardcode
- Content generation (replies, summaries, reports) is part of the workflow

**Always pair with:** JSON mode or structured output prompting + output validation before downstream steps.

**Ideal use:** One or more steps inside a Make, n8n, or custom Python automation — not usually the entire stack.

---

## Common Combinations

| Scenario | Recommended Stack |
|---|---|
| Simple business workflow, non-technical team | Zapier or Make |
| Complex workflow, non-technical team | Make |
| High volume, cost-sensitive | n8n self-hosted |
| Data privacy required | n8n self-hosted or custom code |
| AI content processing + workflow | Claude API + Make or n8n |
| Mission-critical, developer-owned | Custom Python/Node.js |
| Rapid prototype to validate before investing | Make or n8n cloud |
| Client already on Zapier | Zapier (don't migrate unnecessarily) |

---

## Red Flags to Raise Before Recommending

- **Rate limits:** Zapier and Make have per-step and per-minute rate limits. High-volume automations will hit them.
- **Data residency:** Zapier and Make are US/EU cloud services. If the client has data sovereignty requirements, use n8n self-hosted or custom code.
- **Cost surprises:** Zapier's task-based model can shock clients when volume grows. Always show projected monthly cost at expected volume.
- **Maintenance ownership:** "We'll set it up and hand it over" only works if the client or their team can actually maintain it. Mismatch here kills automations within months.
- **Third-party API dependencies:** If the automation depends on an API with its own costs, rate limits, or approval process — surface this in Stack Selection, not after build.
