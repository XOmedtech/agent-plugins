---
name: plan-outreach-sequence
description: Pick a segment of companies and contacts in an XO Medtech campaign and draft their first outreach emails for review. Use when the user wants to start outreach on a campaign, plan who to contact next, or draft emails for a group such as "the Texas CDMOs" or "my top ten uncontacted companies".
---

# Plan an outreach sequence

Turn "who should I reach out to?" into a short, agreed list of contacts with a saved draft for each. Nothing is sent and nothing is approved by this skill.

## Tools

Read: `list_campaigns`, `get_campaign`, `search_leads`, `get_lead`, `get_contact`, `list_voice_prompts`, `get_campaign_sequence`
Write: `draft_email`, `draft_linkedin`, `update_draft`

## Steps

1. **Find the campaign.** Call `list_campaigns`, filtering by `name` when the user named one. If more than one fits, ask which.
2. **Define the segment.** Ask what the user wants if they haven't said: a state or city, favorites only, companies not yet contacted, a number of companies. Then call `search_leads` with those filters. Default to `contacted: false`, sorted by score.
3. **Shortlist.** For the top companies, call `get_lead` to read the profile and contacts. Choose one to three contacts per company whose titles fit the campaign. Skip contacts with no email when planning email outreach.
4. **Agree the list before drafting.** Show the shortlist as a table: company, contact, title, and one line on why now (recent news, funding, a trial, a patent). Ask the user to confirm or change it. Drafting enrolls a contact in the campaign's sequence, so do not draft for anyone the user has not confirmed.
5. **Pick the voice.** Call `list_voice_prompts`. If the user has more than one, ask which to use. If they have none, draft without one.
6. **Draft.** Call `draft_email` for each confirmed contact, passing `voice_prompt_id` when one was chosen. Use `draft_linkedin` only when the user asks for LinkedIn messages.
7. **Review.** Show each subject and body. Apply edits the user asks for with `update_draft`, which changes only the fields passed and keeps the rest.
8. **Hand off.** Summarize who now has a draft and stop. Approving and sending to a CRM is a separate step the user starts.

## Rules

- A draft stays a draft. Never call `approve_draft` or `push_contact_to_crm` from this skill.
- `draft_email` and `draft_linkedin` replace an existing draft and clear its approval. Before drafting for a contact, check `get_contact`. If a draft exists, ask whether to replace it.
- Keep batches small. Draft for at most ten contacts before checking in.
- The first email is what gets drafted here. When a contact is later sent to a CRM that runs its own cadence, XO writes follow-ups two and three into the CRM record. Use `get_campaign_sequence` to show the user what the campaign's steps are.
- If the write tools are not available, the connection is view-only. Tell the user to reconnect XO and allow changes, and offer the shortlist without drafts.
