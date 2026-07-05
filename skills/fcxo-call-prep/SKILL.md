---
name: fcxo-call-prep
description: "Build a one-page prep before a discovery, sales, or client call: the objective, a tight agenda, the questions to ask, and the next step to land. Use when the user wants call prep, to prep for a call, prepare for a meeting, a discovery call agenda, or sales call preparation."
argument-hint: "<lead, client, or engagement> [what the call should achieve]"
allowed-tools: Read, Write, Edit, Glob, WebFetch, WebSearch
---

# fcxo-call-prep - one page to read five minutes before the call

The pre-call counterpart to `fcxo-call-summary`. Build a single page the user reads in
five minutes before a discovery/sales call or an important client call, so the call
moves the deal or the engagement instead of meandering.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` for workspace conventions
and `${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md` for how anything the user will
say out loud (objection answers, the next-step ask) should sound. Read the user's role
silently from `me/*- Profile.md` and frame the whole prep through it: a CTO probes
delivery, architecture, and risk; a CMO pipeline and growth; a CFO cash and control.

## Input

`$ARGUMENTS`: who the call is with (a lead, a client, or an engagement) and, optionally,
when it is and what the user wants from it. If the counterpart is ambiguous and a
workspace exists, list the plausible leads/clients and ask once.

## Pick the mode

Who the call is with decides the prep:

- Matches a `leads/*- Lead.md` file, or is a prospect with no client folder -> **lead call**.
- Matches a client under `clients/` -> **client call**.
- Neither and no workspace -> **standalone** (below).

## Lead / prospect call

Read the lead file first: `leads/<Lead> - Lead.md` already holds the research brief and
any qualification verdict. Do not redo `/fcxo-prospect-research`. The only web work
(WebSearch/WebFetch) is a quick fresh-signals check: anything new since the file's
`updated:` date - news, role changes, funding. One or two searches at most; fold real
findings in with their source, and skip the section if nothing changed.

Prep, in this order:

1. **Objective** - the single decision or commitment this call should land (a paid
   next step, access to the decision-maker, a scoped pilot). One line.
2. **Agenda** - a tight sequence the user can steer by: opening, discovery, positioning
   moment, close. Minutes, not ceremony.
3. **Discovery questions** - walk the pain funnel in the user's role lens, sequenced
   from open ("how is X going") to specific ("what did that cost you last quarter").
   Questions the lead file already answers do not get asked again.
4. **Qualification gaps** - whichever of fit / urgency / budget / access the lead file
   leaves unscored (the four `fcxo-qualify` axes), each with the one question that
   would close it.
5. **Likely objections** - two to four the lead is likely to raise, each with a short
   answer grounded in `me/*- Positioning.md`. If positioning is empty, ask; never
   fabricate an answer the user would not say.
6. **Next step to propose** - the one concrete ask at the end of the call, matched to
   the objective. One step, not a menu.

## Existing client call

Read the engagement file (`clients/<slug>/engagements/<id>/*- Engagement.md`), the most
recent call summary in `meetings/`, recent status updates, and open threads in
`communications/`.

Prep, in this order:

1. **Open items and promises due** - commitments from prior calls and threads, with
   owner and due date. Flag anything the user owes that is overdue.
2. **Decisions pending** - what is waiting on this call, linking the relevant
   deliverables (`[[<YYYY-MM-DD> - <Topic>]]`) so the user can open them in one click.
3. **Risks to raise** - anything on scope, timeline, or budget the user should surface
   on the call rather than sit on.
4. **Outcome** - what this call should produce: a decision made, an approval, a scope
   confirmed. One line.

## Save

- **Lead call**: append the prep to `leads/<Lead> - Lead.md` under a dated heading
  (`## Call prep - <YYYY-MM-DD>`), using Edit - never overwrite the file. Refresh
  `updated:` in its frontmatter.
- **Client call**: save to
  `clients/<slug>/engagements/<id>/meetings/<YYYY-MM-DD> - <Client> Call Prep.md` with
  frontmatter `type: meeting`, `status: draft`, `updated: <date>`. Per the wiki
  convention, link `[[<Client> - Profile]]` and the engagement file
  (`[[<Client> - <Descriptor> Engagement]]`).

After the call, `/fcxo-call-summary` closes the loop - turn the notes into action items
and a follow-up. Mention it when delivering the prep.

## Standalone

No workspace: ask who the call is with and what the user wants from it, build the prep
from what they tell you (plus the fresh-signals check for a prospect), and return it in
chat. Offer to save once a workspace exists.

## Quality gate

Before delivering, check:

- Fits one page and reads in five minutes; anything longer gets cut, not compressed.
- One objective and one proposed next step, and they match.
- Questions and objection answers sound like the user's role and voice, not a script.
- The prep adds sequence and focus; it does not recap what the lead or engagement file
  already says.
- Web tools were used only for the fresh-signals check, nothing else.
