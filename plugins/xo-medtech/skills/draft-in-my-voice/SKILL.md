---
name: draft-in-my-voice
description: Draft or redraft an outreach email or LinkedIn message for an XO Medtech contact using one of the user's saved voice prompts, or save a new voice prompt. Use when the user asks to write to a contact "in my voice", wants a draft to sound more like them, or wants to create or change a writing voice.
---

# Draft in the user's voice

XO stores voice prompts: short instructions that describe how a person writes. This skill drafts with one, and helps the user create or adjust one.

## Tools

Read: `list_voice_prompts`, `search_contacts`, `get_contact`
Write: `draft_email`, `draft_linkedin`, `update_draft`, `create_voice_prompt`, `update_voice_prompt`

## Steps

1. **Find the contact.** Use `search_contacts` with the name the user gave, then `get_contact`. If several people match, ask which.
2. **Find the voice.** Call `list_voice_prompts`.
   - One prompt: use it.
   - Several: ask which, by name.
   - None: offer to create one (see below), or draft without a voice.
3. **Check for an existing draft.** `get_contact` shows the last generated email and LinkedIn message. Generating again replaces that draft and clears its approval, so if one exists, ask before replacing it.
4. **Draft.** Call `draft_email` or `draft_linkedin` with `voice_prompt_id`.
5. **Review.** Show the result. For small changes, use `update_draft` with only the fields that change. Regenerate only when the user wants a different take.

## Creating or changing a voice prompt

1. Ask the user for two or three messages they have written and like, or for a description of how they write.
2. Write the prompt as plain instructions about their writing: sentence length, how they open and close, formality, words they use and avoid, how direct the ask is. Keep it under 5,000 characters. Describe the style only. Do not copy names, companies or other details from the samples into the prompt.
3. Show the prompt and a suggested short name. Save with `create_voice_prompt` only after the user agrees. Names must be unique among their prompts.
4. To adjust an existing voice, show the current text, propose the change, and call `update_voice_prompt` once the user agrees.

## Rules

- Drafting sends nothing, but it does enroll the contact in their campaign's sequence. Say so the first time in a conversation.
- Never approve a draft or send a contact to a CRM from this skill.
- Do not delete a voice prompt unless the user asks for that prompt by name.
- If the write tools are not available, the connection is view-only. Tell the user to reconnect XO and allow changes.
