# AI Automation Agency — Claude Code Skill

A production-grade Claude Code skill that turns Claude into a senior AI automation consultant. Drop it into any Claude Code project and you get a structured, professional workflow for diagnosing business problems, designing automation systems, selecting the right tools, building reliably, and delivering to clients.

## What This Does

Most AI tools jump straight to building. This skill doesn't. It follows a strict 6-phase consulting workflow that mirrors how a real automation agency operates:

1. **Diagnosis** — Find the real problem, not the stated one. Quantify cost before touching any tool.
2. **Discovery** — Map triggers, inputs, outputs, stack, volume, and people involved.
3. **Stack Selection** — Pick the right tool (Zapier, Make, n8n, Claude API, custom code) based on the client's context — not preference.
4. **Architecture** — Design the full flow, every decision branch, and every failure case before building.
5. **Building** — Implement to the approved architecture with error handling, retries, logging, and a full test plan.
6. **Delivery** — Package for a real business: plain-language docs, handover checklist, monitoring guide, and next automation suggestions.

## What's Included

```
.claude/
  skills/
    ai-automation-agency/
      SKILL.md          # The full 6-phase consulting workflow
      patterns.md       # 8 automation patterns with flow diagrams and failure modes
      stack-guide.md    # Tool selection guide: Zapier, Make, n8n, Python, Claude API
    skill-builder/
      SKILL.md          # Meta-skill for building new Claude Code skills
      reference.md      # Skill authoring reference

AI AutomationAgencyDocs/    # Sample output from a demo client engagement
  README.md                 # Client-facing plain-language guide
  architecture.md           # Full automation architecture
  proposal.md               # Client proposal
  diagnosis.md              # Diagnosis output
  discovery.md              # Discovery summary
  handover-checklist.md     # Setup and handover steps
  monitoring.md             # Post-launch monitoring guide
  next-automations.md       # Follow-on opportunities
```

## How to Use

1. Clone or copy `.claude/skills/ai-automation-agency/` into your project's `.claude/skills/` folder
2. Open Claude Code in that project
3. Describe a client's automation problem — Claude will start the Diagnosis phase automatically

Claude Code picks up skills from `.claude/skills/` and applies them based on context. No configuration needed.

## What the Sample Output Shows

`AI AutomationAgencyDocs/` contains a complete set of deliverables from a demo engagement: a hotel in Santorini automating their guest communication flow (booking confirmations, pre-arrival emails, PDF invoices, website inquiry handling via Make.com + Cloudbeds + Gmail).

It shows exactly what gets produced at the end of a full engagement — from diagnosis through delivery.

## Patterns Library

`patterns.md` covers 8 base automation patterns with flow diagrams and failure modes:

| Pattern | When to Use |
|---|---|
| Webhook → Transform → Action | Event-based triggers in real time |
| Scheduled Polling | Sources without webhook support |
| Event-Driven Pipeline | Multi-stage processing with independent retries |
| Approval Workflow | Human sign-off before automated action |
| Data Sync | Keeping two systems consistent |
| Enrichment Pipeline | Appending data from external sources |
| Monitor & Alert | Threshold-based notifications |
| Document Processing (AI) | Extracting structured data from unstructured input |

## Stack Selection Guide

`stack-guide.md` covers decision criteria for every major automation tool — Zapier, Make, n8n, custom Python/Node.js, and Claude API — including pricing signals, ideal client profiles, and common combinations.

## Built With

- [Claude Code](https://claude.ai/code) — AI coding assistant with persistent skills
- Designed for use with: Make.com, n8n, Zapier, Claude API, Python, Node.js

---

Built by [Theo](https://github.com/theokalm) — AI automation consultant based in Greece.
