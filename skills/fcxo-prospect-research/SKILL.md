---
name: fcxo-prospect-research
description: "Research a target company and decision-maker before outreach or a sales call, and produce an actionable brief tuned to the user's role. Use when the user wants to research a company or prospect, or prep before reaching out."
argument-hint: "<company, person, or URL>"
allowed-tools: Read, Write, Edit, Glob, WebFetch, WebSearch
---

# fcxo-prospect-research – actionable intel before you reach out

Turn a name or URL into a short brief the user can act on, framed through their role's
value to this prospect.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` for workspace
conventions. Read the user's role from `me/*- Profile.md` if a workspace exists; otherwise
ask their role in one line (it changes what matters in the brief).

## Input

`$ARGUMENTS` is the target: a company name, a person, a LinkedIn/company URL, or a
short description. If empty, ask for it.

## Research

Search the web for what a fractional executive needs to open a relevant conversation:

- **Company** – what they do, size/stage, recent funding or news, hiring signals,
  tech/market footprint, anything suggesting change or pressure.
- **Person** (if named) – role, tenure, background, what they post or care about, a
  genuine point of connection.
- **Trigger signals** – events that create a need for *this role* (e.g. for a CTO: a
  scaling push, a rebuild, a new CTO-less round; for a CMO: a rebrand, a growth target,
  a new market). Tie signals to the user's role.

Verify across more than one source where it matters. Mark anything uncertain as such –
never present a guess as fact.

## Output – the brief

Keep it tight and skimmable. Structure:

1. **Snapshot** – one paragraph: who they are, stage, what's going on now.
2. **Why now, for your role** – 2-4 signals that connect to what the user does, each
   with the source.
3. **Likely pains** – framed as the buyer would feel them, in the buyer's own words.
4. **The opening** – one concrete, specific hook the user could lead with (a fact, a
   change, a shared connection). This is the payoff; make it usable verbatim.
5. **Open questions** – what to confirm on a call.

## Save

If a workspace exists, save the brief to the lead record inside the lead's own folder,
`leads/<slug>/<Lead> - Lead.md` (update the existing record if one exists - add the brief
under a dated heading and keep what is already there), with frontmatter `type: lead`,
`status: new`, `updated: <date>`. When the lead is new, writing the record creates the
folder, and its `communications/`, `meetings/`, and `proposals/` fill up as the deal
moves. Per the wiki convention in
`practice-workspace.md`, if the lead came through a referral or relates to an existing
client, link that source (`[[<Client> - Profile]]` or the referring person). Otherwise
return it in chat and offer to save once a workspace is set up. Suggest `/fcxo-outreach`
or `/fcxo-qualify` as the next step.

## Notes

- Public sources only. Do not fabricate contact details or private data.
- The brief serves a decision (reach out or not, and how) – cut anything that doesn't.
