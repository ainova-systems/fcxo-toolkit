---
name: fcxo-outreach
description: "Draft personalized cold, warm, referral, or network-touch outreach in the user's own voice. Use when the user wants to draft outreach, write a cold email, message a prospect, ask for a referral, or check in with a warm contact they haven't spoken to in a while."
argument-hint: "<person/company or lead slug> [cold|warm|referral|touch]"
allowed-tools: Read, Write, Edit, Glob, WebFetch, WebSearch
---

# fcxo-outreach – outreach in your voice, ready to send yourself

Draft a single, specific message a fractional executive could send to a prospect or
contact. **Draft only – never send.**

Read `${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md` (how to write it) and
`${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` (where context lives). Pull the
user's voice from `me/*- Positioning.md` and role from `me/*- Profile.md`. If no workspace,
ask for their role and one line on what they do – do not invent positioning.

## Input

`$ARGUMENTS`: the target (a person/company, or an existing lead, `leads/**/*- Lead.md`)
and optionally the type: `cold`, `warm`, `referral`, or `touch`. If the type is unclear,
infer it from context or ask in one line.

## Gather context

- If the target is an existing lead, read `leads/<slug>/<Lead> - Lead.md` and the thread
  so far in `leads/<slug>/communications/`, so the draft continues the conversation.
- If you have little on them, run quick research (as in `fcxo-prospect-research`) to
  find one real, specific hook. A generic message is worse than none.

## Draft by type

- **Cold** – open on a specific, true observation about them (the hook), connect it to
  the value the user provides for that role, make one small ask (a short call). Keep it
  short and lead with them.
- **Warm** – reference the real prior connection plainly. No theatrical "it's been
  ages". Greet, say why you're writing, make the ask.
- **Referral** – ask **only** for the referral. Never pair it with a pitch. Plain and
  direct: "I'd really appreciate a referral if you can share one." State briefly what
  kind of client fits, so they know who to think of.
- **Touch** – a periodic check-in that keeps a warm relationship alive. Reference
  something real and recent from them (a post, a launch, a role change) or share one
  thing genuinely useful to them. No ask, no pitch – the message is complete as a
  touch. Most fractional work arrives through the warm network, so these small
  messages are pipeline work; suggest who is due one when the user asks "who should I
  check in with" (companies whose folder shows no message, meeting, or update for a
  quarter).

For every type: apply the copy principles (no em-dash, no contrast-as-positioning, no
AI-slop, plain English, position from strength). Mirror the channel – an email can be a
touch fuller than a LinkedIn DM.

## Output

Give the draft in a clean block the user can copy. Add one or two lines of plain
commentary (why this hook, what to tweak). If a subject line applies, offer 2 options.

## Save

An outreach draft is a message, so it goes where every message goes: the company's
`communications/` folder. Leads and clients get the same treatment, which is what one
company, one folder buys.

If a workspace exists, save the draft to
`<company>/communications/<YYYY-MM-DD> - To <Name> - <Subject>.md` with frontmatter
`type: communication`, `status: draft`, `updated: <date>`, where `<company>` is
`leads/<slug>/` before the contract and `clients/<slug>/` after it. If the thread already
has a file for this exchange, append to it. Never fork a second file for the same
exchange. Per the wiki convention in `practice-workspace.md`, the draft links the lead or client it addresses
(`[[<Lead> - Lead]]` or `[[<Client> - Profile]]`), and the record links back to the draft.

For a cold prospect or a network touch to a contact with no folder yet, offer to start
`leads/<slug>/<Name> - Lead.md` so this message and every future touch have a home.

Do not send. Remind the user it's a draft for them to send.
