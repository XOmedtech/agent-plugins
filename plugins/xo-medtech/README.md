# XO Medtech

Work your XO Medtech campaigns from your AI assistant. The plugin connects to your XO account and adds skills for the everyday outreach jobs: choosing who to contact, drafting in your own voice, working the daily to-do list, sending approved contacts to your CRM, and checking how a campaign is doing.

You need an XO Medtech account in an organization that has AI app connections turned on.

## Install

In Claude Code:

```
/plugin marketplace add xomedtech/agent-plugins
/plugin install xo-medtech@xo-medtech
```

The first time a skill needs your data, your browser opens the XO sign-in page. Sign in, choose whether the assistant may make changes or only view, and approve. There are no API keys to copy.

## Skills

| Skill | What it does |
|---|---|
| `plan-outreach-sequence` | Picks a segment of companies and contacts with you, then saves a first-email draft for each. |
| `daily-outreach-todo` | Walks through what is due today and records what you sent and who replied. |
| `draft-in-my-voice` | Drafts an email or LinkedIn message using one of your saved voice prompts, or helps you create one. |
| `push-approved-to-crm` | Approves a draft and sends the contact, company and email to your connected CRM, after you confirm. |
| `campaign-health-check` | Reports progress, overdue work, replies and suggested next steps. Read-only. |

## What it can and cannot do

- It never sends email or LinkedIn messages. You send from your own mail client or your CRM's cadence does.
- Drafts stay drafts until you approve them.
- It only records a send or a reply when you say it happened.
- Sending a contact to your CRM writes outside XO and cannot be undone, so the assistant always asks first.
- It sees only what your XO role lets you see. If you connect as view-only, the skills that make changes will tell you to reconnect and allow changes.

## Managing access

In XO, go to Settings and open Connected apps to see every connection and revoke any of them. Revoking takes effect straight away.

## Support

Email hello@xomedtech.com.
