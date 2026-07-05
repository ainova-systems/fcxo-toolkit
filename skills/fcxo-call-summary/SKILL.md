---
name: fcxo-call-summary
description: "Turn call notes or a transcript into action items, a follow-up email draft, and an internal summary. Use when the user wants to summarize a call, process notes, write a follow-up, or recap a meeting."
argument-hint: "<paste notes/transcript or path> [client/engagement]"
allowed-tools: Read, Write, Glob
---

# fcxo-call-summary – from messy notes to action items and a follow-up

Process raw call notes or a transcript into three clean outputs. The follow-up email is
**draft only**.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` for where things live and
`${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md` for the follow-up's tone. Read the
user's role silently from `me/*- Profile.md` and weight the summary's emphasis to it: a CTO
recap leads with delivery, architecture, and risk; a CMO recap with pipeline and growth;
a CFO recap with cash and commitments.

## Input

`$ARGUMENTS` (or an attached file): the notes/transcript, and optionally which
client/engagement it belongs to. If the client is unclear and a workspace exists, list
clients and ask once. If no notes are provided, ask for them.

## Produce three outputs

1. **Internal summary** – what was discussed, decisions made, open questions, and any
   change to scope, timeline, or risk. Factual and brief. This is for the user's
   records, in their language.
2. **Action items** – a checklist with owner (user / client / other) and, where stated,
   a due date. Pull every commitment made on the call. Do not invent dates that weren't
   said.
3. **Follow-up email (draft)** – a short message to the client recapping what matters
   to them and confirming next steps. Mirror the relationship's tone; apply the copy
   principles (no em-dash, plain language, no filler). Mark it clearly as a draft.

## Save

If a workspace exists, save the internal summary and action items to
`clients/<slug>/engagements/<id>/meetings/<YYYY-MM-DD> - <Client> Call Summary.md` with
frontmatter `type: meeting`, `updated: <date>`. Per the wiki convention in
`practice-workspace.md`, the summary links `[[<Client> - Profile]]` and its engagement
file (`[[<Client> - <Descriptor> Engagement]]`). Put the follow-up draft in the client's
`communications/` folder, the home for outward message drafts, and link the same client
profile from it. Never send it.

If no workspace, return all three in chat.

## Notes

- Distinguish a stated commitment from a vague intention – only firm commitments become
  action items with owners.
- If the notes are thin, summarize what's there and flag the gaps rather than padding.
