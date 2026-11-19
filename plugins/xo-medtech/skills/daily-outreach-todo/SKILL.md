---
name: daily-outreach-todo
description: Work through today's outreach to-do list for an XO Medtech campaign, one contact at a time. Use when the user asks what outreach is due, wants to work their sequence or follow-ups, or says they sent a message or got a reply and wants XO updated.
---

# Work the daily outreach to-do list

Walk the user through what is due, show them the message for each contact, and record what they tell you happened. The user sends every message themselves. This skill only keeps XO in step with what they did.

## Tools

Read: `list_campaigns`, `get_campaign_sequence`, `get_contact_activity`, `get_contact`
Write: `mark_contacted`, `complete_sequence_step`, `mark_replied`, `remove_from_sequence`, `revert_step_completion`, `update_draft`

## Steps

1. **Find the campaign.** Call `list_campaigns` and ask which campaign if the user has several and did not name one.
2. **Load the list.** Call `get_campaign_sequence` with `include_completed: false`. Pass `only_mine: true` when the user asks for their own territory.
3. **Show what is due.** Lead with the counts per due bucket from `by_bucket`, which covers the whole list. Then list the returned contacts grouped by bucket, overdue first, with contact, company, the next action and its channel. If `total` is larger than the rows returned, say how many more there are.
4. **Go one contact at a time.** Call `get_contact_activity` to get the step's subject and message and show it in full so the user can copy it. Offer edits with `update_draft` if they want changes first.
5. **Record the outcome the user reports:**
   - They sent it: `mark_contacted` with the channel they used.
   - The touch happened outside XO, such as a phone call: `complete_sequence_step` with the `step_index` from `get_campaign_sequence`.
   - The contact replied: `mark_replied`.
   - They want the contact dropped: confirm, then `remove_from_sequence`.
   - They want to skip for now: do nothing and move on.
6. **Close.** When the list is done or the user stops, summarize what was recorded and what is still due.

## Rules

- Never call `mark_contacted`, `complete_sequence_step` or `mark_replied` unless the user has said, for that contact, that it happened. "Looks good" is not "I sent it".
- These tools send nothing. Say so if the user seems to expect XO to send the message.
- Ask before `remove_from_sequence`. It takes the contact out of the outreach list.
- If something was recorded by mistake, `revert_step_completion` undoes the last completion for that contact.
- If `get_campaign_sequence` is not available, sequences are not enabled for this organization. Say that and stop.
- If the write tools are not available, the connection is view-only. Show the list, and tell the user to reconnect XO and allow changes to record progress.
