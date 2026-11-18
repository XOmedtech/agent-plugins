---
name: push-approved-to-crm
description: Approve a contact's outreach email in XO Medtech and send the contact, their company and the email to the organization's connected CRM, with the user's confirmation at each step. Use when the user asks to push, sync or send a contact or a set of contacts to HubSpot, Salesforce, Attio, Notion, Pipedrive or "the CRM".
---

# Send approved contacts to the CRM

A push writes to a system outside XO and cannot be undone from here. This skill makes sure the user has seen exactly what will go, and has said yes, before anything leaves.

## Tools

Read: `list_crm_connections`, `get_contact`, `get_contact_crm_sync`, `search_contacts`, `list_voice_prompts`
Write: `approve_draft`, `push_contact_to_crm`, `mark_contacted`

## Steps

1. **Check the CRM.** Call `list_crm_connections`. With none connected, tell the user to connect one in XO under Settings, then stop. With several, ask which.
2. **Read each contact.** Call `get_contact` and `get_contact_crm_sync`. Stop for that contact and tell the user if:
   - there is no email draft (offer to draft one first);
   - the contact has no email address;
   - the contact was already pushed to this CRM (show the record link and ask whether to push again).
3. **Show what will go.** For each contact: name, title, company, the CRM, and the full email subject and body.
4. **Get a clear yes.** Ask the user to confirm the push for the named contacts. A yes covers only the contacts and drafts shown. If a draft is edited afterwards, show it and ask again.
5. **Approve, then push.** For each confirmed contact, call `approve_draft` with channel `email`, then `push_contact_to_crm`. Pass `contact_name`, `company_name` and `crm_type` exactly as `get_contact` and `list_crm_connections` report them. Pass `voice_prompt_id` if the user chose a voice for the follow-ups.
6. **Report by mode.** Read `sequenceMode` in each result:
   - `automated`: the CRM's own cadence sends. XO drafts follow-ups two and three into the record in the background. Tell the user nothing more is needed from them for the email track.
   - `manual`: nothing was sent. Tell the user they still need to send this email from their own mail client. When they say it is sent, call `mark_contacted` with channel `email`.
7. **Summarize.** List what was pushed, with record links from the results, and anything skipped with the reason.

## Rules

- Never call `approve_draft` or `push_contact_to_crm` without the user's confirmation for that contact in this conversation.
- For more than one contact, show the whole list and get one confirmation for that list. Push at most ten contacts per confirmation.
- If a push fails, stop the batch, report the error as given, and ask how to proceed. Do not retry on your own.
- Never call `mark_contacted` because a push succeeded. Only call it when the user says they sent the message.
- If the write tools are not available, the connection is view-only. Tell the user to reconnect XO and allow changes.
