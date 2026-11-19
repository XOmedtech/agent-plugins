---
name: campaign-health-check
description: Summarize how one XO Medtech campaign, or all of them, is doing — outreach progress, what is overdue, who replied and what to do next. Use when the user asks for a campaign status, pipeline or outreach report, a weekly summary, or "how are my campaigns doing".
---

# Campaign health check

A read-only report. It changes nothing in XO.

## Tools

`get_dashboard_summary`, `list_campaigns`, `get_campaign`, `get_campaign_analytics`, `get_campaign_sequence`, `list_replied_contacts`, `search_leads`, `list_org_users`

## Steps

1. **Scope.** One campaign or all of them? If the user did not say and has more than three campaigns, start with the overview and offer to go deeper on one.
2. **Overview (all campaigns).** Call `get_dashboard_summary` and `list_campaigns`. Report totals, then one row per campaign: companies, contacts, and the share of each contacted.
3. **One campaign.** Call `get_campaign_analytics`, then `get_campaign_sequence` and `list_replied_contacts` for that campaign.
   - Progress: contactable versus contacted, for companies and contacts.
   - To-do list: how many contacts are overdue, due today and due later, read from `by_bucket`. It counts the whole list, while `records` is capped by `limit`, so never count rows to get a total. Pass `include_completed: false` and a small `limit`.
   - Replies: who replied, newest first, with company.
4. **Find the gaps.** Call `search_leads` with `contacted: false`, sorted by score, and name the five highest-scoring companies nobody has contacted.
5. **Recommend.** End with up to three next actions drawn from the data, such as "12 contacts are overdue" or "five high-score companies have no outreach". Offer to help with one. Do not start it unless the user says so.

## Output

Start with a two-sentence summary. Follow with a short table, then the next actions. Give counts and percentages as the tools return them. Do not estimate figures the tools did not provide.

## Rules

- Replies are what people recorded in XO. XO does not read mailboxes, so say "marked as replied", not "replied", and do not present the count as a reply rate.
- "Contacted" means a send was recorded in XO. It is not proof of delivery or opens.
- If `get_campaign_sequence` or `list_replied_contacts` is not available, sequences are not enabled for the organization. Leave those sections out and say why.
- When results name a `user_id`, resolve it with `list_org_users` before showing it.
