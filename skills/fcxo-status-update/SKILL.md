---
name: fcxo-status-update
description: "Draft a client's periodic status update from the workspace record - progress tied to engagement goals, explicit asks, risks - plus a private scope-creep check. Use when the user wants to send a status update, weekly update, client update, or progress report, or asks what should I send my client this week."
argument-hint: "[client] [period, e.g. this week] [anything not in the files]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-status-update - the update that keeps your value visible

Draft the short, periodic update the client actually reads: where the engagement
stands, what moved, what you need from them. Engagements renew when the exec's value
stays visible between calls, and this update is the retention artifact. **Draft only** -
the user sends it.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` for the workspace layout
and the wiki convention, and `${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md` for
the draft's tone. Read the user's role silently from `me/*- Profile.md` (the workspace
root is the folder holding `me/`) and weight the update to it: a CTO update leads with
delivery, architecture calls, and risk; a CMO update with pipeline and campaign results;
a CFO update with cash, runway, and commitments.

## Resolve client and period

`$ARGUMENTS` names the client and optionally the period. If the client is unclear and a
workspace exists, list active clients and ask once. The period defaults to "since the
previous status update" (or since the engagement started, when there is none).

## Gather the period from the workspace

The workspace already holds the week - read it before asking the user anything:

1. **Engagement file** `clients/<slug>/engagements/**/*- Engagement.md`: scope, goals,
   status. This is the frame - every reported item ties back to it.
2. **Previous status update** in `clients/<slug>/communications/` (a
   `* Status Update.md` file): what was promised last time. Continuity is the trust
   signal - anything promised there is either done, moved, or explained in this update,
   never silently dropped.
3. **Meetings** `clients/<slug>/engagements/**/meetings/` since the last update:
   decisions, commitments, action items (written by `fcxo-call-summary`).
4. **Communications** threads since the last update: what the client asked, what was
   agreed in writing (logged by `fcxo-log-message`).
5. **Deliverables** `clients/<slug>/engagements/**/deliverables/` shipped since the
   last update.

Then ask the user **only for what the files cannot know**: progress on items with no
written trace, hours spent if the engagement tracks them, anything that happened off
the record. One compact question list, not an interview.

## Scope-creep check (to the user, never in the client draft)

While gathering, compare the period's actual work against the engagement scope. When
something drifted outside it - a new workstream, recurring requests the scope never
named - flag it to the **user** in chat: name the item, say why it sits outside scope,
and suggest addressing it early (a scope conversation on the next call, or a proposal
via `/fcxo-proposal`). Keep the flag out of the client draft; how and when to raise it
is the user's call.

## Draft the update

Short and skimmable, in the reader's language - a busy founder or exec reads this
between meetings. Match the client's language and formality from the thread history in
`communications/`; apply the copy principles (no em-dash, plain words, no filler).

1. **Headline** - one sentence on where the engagement stands.
2. **Done since last update** - each item tied to the engagement goal it serves
   ("release cycle cut to weekly, toward the Q3 delivery goal"), never activity for
   its own sake. Three to six items; fold the rest.
3. **In progress / next** - what is moving and what lands next period.
4. **Decisions or input needed** - explicit asks with an owner and, where one was
   stated, a date. This is the section that makes the client reply.
5. **Risks or blockers** - stated plainly, with what you are doing about each.
6. **Hours / budget note** (only if the engagement tracks them) - one line of period
   usage against the arrangement.

Skip a section that is genuinely empty rather than padding it.

## Save

If a workspace exists, save to
`clients/<slug>/communications/<YYYY-MM-DD> - <Client> Status Update.md` with light
frontmatter (`type: communication`, `status: draft`, `updated: <date>`), linking
`[[<Client> - Profile]]` and the engagement file
(`[[<Client> - <Descriptor> Engagement]]`). The draft body is exactly what the user
copies into email or Slack. Never send it.

If no workspace, ask for the facts (scope, what happened, what's next, the asks) and
return the draft in chat.

## Cadence

This is the toolkit's natural scheduled task: one run per active client per cadence,
for example every Friday, producing a draft the user reviews Monday morning.
Scheduling belongs to the user's harness - wire it in their tool (e.g. a Cowork
scheduled task) with a prompt like "run /fcxo-status-update for <client>".

## Notes

- `fcxo-log-message` and `fcxo-call-summary` build the record this skill reads; the
  more of the week is logged, the fewer questions this skill has to ask.
- `fcxo-renewal-review` aggregates these updates at renewal time - consistent updates
  make that case write itself.

## Quality gate

Before delivering, check:

- Every "done" item ties to an engagement goal, not to raw activity.
- Everything promised in the previous update is accounted for: done, moved, or explained.
- Asks are explicit, each with an owner.
- Tone, language, and formality match the thread history.
- Any scope-creep flag went to the user in chat, never into the client draft.
- The draft was saved (or returned in chat), never sent.
