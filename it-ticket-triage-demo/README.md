# IT Support Ticket Triage — AI Multi-Agent System

An AI-powered workflow that classifies incoming IT support tickets, 
auto-drafts responses for low-risk requests, and routes anything 
uncertain to a human for approval via Slack — with full logging 
to Google Sheets.

## Architecture
Form/UI → Classifier Agent (Groq LLM) → Severity Router (IF) 
  → Auto path: Resolver Agent → Log 
  → Human path: Slack Approval → Log

## Tech Stack
- n8n (workflow orchestration)
- Groq (LLM inference)
- Slack (human-in-the-loop approval)
- Google Sheets (observability/logging)

## Files
- `/workflow/IT_Ticket_Triage.json` — exported n8n workflow
- <img width="1852" height="827" alt="image" src="https://github.com/user-attachments/assets/8b0ff675-50b4-4268-ac09-f4c0c69e3e78" />
