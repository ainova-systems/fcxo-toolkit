---
name: fcxo-call-prep
description: "Build a one-page prep before a discovery, sales, or client call: the objective, a tight agenda, the questions to ask, and the next step to land. Use when the user wants call prep, to prep for a call, prepare for a meeting, a discovery call agenda, or sales call preparation."
argument-hint: "<lead, client, or engagement> [what the call should achieve]"
allowed-tools: Read, Write, Edit, Glob, WebFetch, WebSearch
---

# fcxo-call-prep - one page to read five minutes before the call

The pre-call counterpart to `fcxo-call-summary`. Build a single page the user reads in
five minutes before a discovery/sales call or an important client call, so the call
moves the deal or the engagement forward.

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

- Matches a lead folder (`leads/**/*- Lead.md`), or is a prospect with no folder at all -> **lead call**.
- Matches a client under `clients/` -> **client call**.
- Neither and no workspace -> **standalone** (below).

The mode changes what you read and how you frame the prep. It no longer changes where the
prep is saved: both land in the company's `meetings/` folder (see Save).

## Lead / prospect call

Read the lead file first: `leads/<slug>/<Lead> - Lead.md` already holds the research brief
and any qualification verdict, and its siblings `communications/` and `meetings/` hold the
thread and any earlier call. Do not redo `/fcxo-prospect-research`. The only web work
(WebSearch/WebFetch) is a quick fresh-signals check: anything new since the file's
`updated:` date - news, role changes, funding. One or two searches at most; fold real
findings in with their source, and skip the section if nothing changed.

Prep, in this order:

1. **Objective** - the single decision or commitment this call should land (a paid
   next step, access to the decision-maker, a scoped pilot). One line.
2. **Agenda** - a tight sequence the user can steer by: opening, discovery, positioning
   moment, close. Give each block its minutes.
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
   the objective. Propose a single step.

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
   on the call.
4. **Outcome** - what this call should produce: a decision made, an approval, a scope
   confirmed. One line.

## Save

Call prep is a document, and a company's documents live in that company's folder. There
is one destination for both modes, which keeps this simple:

`<company>/meetings/<YYYY-MM-DD> - <Company> Call Prep.md`, frontmatter `type: meeting`,
`status: draft`, `updated: <date>`. `<company>` is `leads/<slug>/` before the contract and
`clients/<slug>/` after it; for a client call scoped to one engagement, use that
engagement's `meetings/` folder (`clients/<slug>/engagements/<id>/meetings/`).

Then write a dated line back into the company record - the lead file for a lead, the
client profile for a client - with a wikilink to the prep
(`[[<YYYY-MM-DD> - <Company> Call Prep]]`), using Edit, and refresh `updated:` in its
frontmatter. Never overwrite the record. Per the wiki convention, the prep links back to
the company (`[[<Lead> - Lead]]` or `[[<Client> - Profile]]`) and, for a client call, to
the engagement file (`[[<Client> - <Descriptor> Engagement]]`).

After the call, `/fcxo-call-summary` closes the loop - turn the notes into action items
and a follow-up. Mention it when delivering the prep.

## Standalone

No workspace: ask who the call is with and what the user wants from it, build the prep
from what they tell you (plus the fresh-signals check for a prospect), and return it in
chat. Offer to save once a workspace exists.

## Quality gate

Before delivering, check:

- Fits one page and reads in five minutes; anything longer gets cut. Never compress to fit.
- One objective and one proposed next step, and they match.
- Questions and objection answers sound like the user's role and voice.
- The prep adds sequence and focus; it does not recap what the lead or engagement file
  already says.
- Web tools were used only for the fresh-signals check, nothing else.
