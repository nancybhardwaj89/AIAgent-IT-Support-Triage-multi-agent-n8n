# AI IT Ticket Triage Agent

An AI-powered multi-agent workflow that classifies incoming IT support tickets, auto-drafts responses for low-risk requests, and routes anything uncertain or high-impact to a human for approval — with every decision logged for tracking and accountability.

Built with **n8n**, **Groq (LLM)**, **Slack**, and **Google Sheets**.

---

## Objective

An AI system that classifies IT tickets, drafts responses for low-risk requests, and routes anything uncertain to a human for approval — with full logging.

This project was built to demonstrate practical, enterprise-relevant patterns in AI workflow automation: intelligent decisioning, human-in-the-loop controls, API-first orchestration, and observability — rather than a single "AI does everything" black box.

---

## How It Works

```
Ticket Intake Form
       ↓
Classifier Agent (Groq LLM)
       ↓
Parse Classification (category, severity, confidence, reasoning)
       ↓
Severity Router (IF node)
       ↓
   ┌───────────────┴───────────────┐
   ↓                               ↓
Low severity +               Medium/High severity
high confidence                OR low confidence
   ↓                               ↓
Resolver Agent                Human Review (Slack)
(drafts response)             (Approve / Reject)
   ↓                               ↓
Set Draft Status               Log decision
   ↓                               ↓
Log – Auto Path                Log – Human Path
   (Google Sheets)                (Google Sheets)
```

1. **Ticket Intake Form** — captures the requester's name, subject, description, and priority guess.
2. **Classifier Agent** — an LLM reads the ticket and outputs structured JSON: category, severity, confidence, and reasoning.
3. **Severity Router** — an IF node applies a simple governance rule: only tickets classified as **low severity with high confidence** are eligible for automatic handling. Everything else — medium, high, or uncertain — is routed to a human.
4. **Resolver Agent** (auto path) — drafts a suggested response for low-risk tickets. This is intentionally labeled as a **draft awaiting verification**, not a completed resolution, since the AI doesn't have write access to any real backend system (e.g. identity management) in this demo.
5. **Human Review via Slack** (escalation path) — sends ticket details to a Slack channel with Approve/Reject buttons, pausing the workflow until a person responds.
6. **Logging** — every ticket, regardless of path, is appended to a Google Sheet with timestamp, requester, subject, category, severity, confidence, path taken, status, and resolution/response text — giving a full audit trail of every AI decision.

---

## Why This Design

- **Conservative auto-resolution threshold**: only the clearest, lowest-risk tickets bypass human review. The cost of a wrong automated action scales with severity, so anything ambiguous defaults to a human decision.
- **Draft, not resolution**: the AI-generated response is explicitly framed as a draft, not a completed fix — avoiding overstating what a text-generation model has actually accomplished.
- **Full observability**: every classification, decision, and outcome is logged, supporting auditability and process improvement over time.

---

## Tech Stack

| Component | Tool |
|---|---|
| Workflow orchestration | [n8n](https://n8n.io) |
| LLM inference | Groq |
| Human-in-the-loop approval | Slack ("Send and Wait for Response") |
| Logging / observability | Google Sheets |
| Trigger | n8n Form Trigger |

---

## Example Scenarios

**Low severity → Auto path**
> Subject: "WiFi password for guest network"
> Description: "Could someone share the guest WiFi password? Not urgent."
>
> Result: classified as `low` severity, `0.9` confidence → Resolver Agent drafts a response → logged as `Auto` / `Response Drafted (Auto)`

**High severity → Human path**
> Subject: "Production database is down"
> Description: "Multiple team members can't access customer records. Urgent."
>
> Result: classified as `high` severity → routed to Slack for human approval → logged as `Human` / `Approved`

---

## What I'd Add for Production Use

- Real API integration (e.g. Azure AD/Okta) for the resolver to take verified actions instead of drafting text
- Retry and exception handling on all external API calls
- Environment separation (dev/prod credentials and webhooks)
- Workflow version control (exported JSON tracked in Git, as in this repo)
- Cost and token tracking per LLM call
- Centralized logging/alerting (e.g. CloudWatch, Datadog) in place of Google Sheets at scale

---

## Repository Contents

```
├── workflow/
│   └── IT_Ticket_Triage.json    # exported n8n workflow
└── README.md
```
## n8n Workflow Diagram
<img width="1852" height="827" alt="image" src="https://github.com/user-attachments/assets/b1d73b61-b9a2-4cc1-b5b3-9d1bc1617239" />

---

## About

Built by [Nancy Bhardwaj](https://linkedin.com/in/nancy-bhardwaj) — Lead QA Engineer & Test Automation Architect, focused on AI-augmented QA and automation engineering.
