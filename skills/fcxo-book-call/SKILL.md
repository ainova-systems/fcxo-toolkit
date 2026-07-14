---
name: fcxo-book-call
description: "Turn 'we should talk' into a held slot and a drafted reply: read the calendar, hold the time on your own calendar with no guests, and draft the message that proposes it. Use when the user wants to book a call, schedule a meeting, find a slot, set up the review call, propose times, or get a call in the diary."
argument-hint: "<lead or client> [what the call is for, how long, any time the other side asked for]"
---

# fcxo-book-call - a held slot and the reply that proposes it

Someone said "let's talk". This skill closes the gap between that and a call in the
diary: it finds real free time, holds it on the user's own calendar, and drafts the
reply that offers the slot. The user sends the reply and sends the invitation once the
time is agreed.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` for the workspace layout,
the wiki convention, and the draft-only rule, and
`${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md` for how the reply should sound.

## The one hard rule: the hold carries no guests

The hold is a private note to self on the user's **own** calendar, so it carries **no
guests**. The moment another person is on a calendar event, the calendar service emails
them an invitation or an update, and that is an outward message the user did not send.
That is the reason, and the rule has two halves:

- **Never add a guest** to an event.
- **Never create or modify any event that already has attendees.** The common case is a
  call that was agreed, invited properly by the user, and has now slipped. Editing that
  event mails every attendee. So if the event you would touch has guests, stop, say in
  one line why you stopped, and let the user move it themselves.

The toolkit is draft-only: the human sends. This skill holds the time and drafts the
reply, and the user sends the real invitation once the other side confirms.

## Input

`$ARGUMENTS`: who the call is with (a lead or a client), and optionally what it is for,
how long it needs, and any timing the other side already asked for. If the counterpart
is ambiguous and a workspace exists, list the plausible companies and ask once.

## Resolve the company and read the context

Resolve the company folder: `leads/<slug>/` before the contract, `clients/<slug>/`
after it. Then read, in this order:

1. **The record** - `leads/**/*- Lead.md` or `clients/<slug>/*- Profile.md`: who the
   contact is, where the relationship stands, what the next step was meant to be.
2. **The latest message** in `<company>/communications/` - this is what was actually
   agreed. "Thursday works", "sometime next week", "after we've read the proposal".
   The message decides the timing, the duration, and the tone of the reply; your own
   preferences do not.
3. **What motivates the call** - the proposal in `<company>/proposals/`, the last
   summary in `<company>/meetings/`, or the engagement. This gives the event its title
   and the reply its one-line purpose.

Set the duration from the purpose: an intro call is 30 minutes, a discovery or proposal
review is 45 to 60, a working session is longer. State the duration you chose.

## Find the calendar, then read it

The toolkit ships no connectors. The calendar belongs to the user's agent surface, so
the skill's first job is to find out which one they have:

1. Read `Practice Workspace - Integrations.md` at the workspace root and take the
   **Calendar** row. It names the connected tool and the access this skill has with it:
   read availability, create a guest-free hold.
2. Use that tool through the agent surface's own connector. The connector is already the
   user's: they installed it, and it carries their permission. Never ask for a credential,
   an API key, or an OAuth flow, and never assume a vendor the registry does not name.

Then read the user's real availability. Respect working hours, the user's timezone, and
existing commitments - a free gap between two calls in different cities is not a free
slot. Prefer slots that match what the other side asked for; if they said "Thursday",
look at Thursday first.

Pick two or three candidate slots and check them against the incoming message before
committing.

**No calendar row, or the named tool is not connected** - degrade gracefully and never
block: say so in one line, ask the user for two or three slots that work for them, draft
the reply from those, and tell them to put the hold in their own calendar. Everything
else in the skill is unchanged.

## Create the hold

One event on the user's **own** calendar, **no guests**. Before writing anything, check
the event you are about to create or move: if a call for this company already sits in
the calendar with attendees on it, the user has sent a real invitation, so stop and hand
it back to them (see the hard rule).

- **Title** - always opens with `[hold]`, then the company, the purpose, and the person,
  so the user reads it cold a week later: `[hold] Talvora Analytics - proposal review
  (Maya Chen)`. The marker is unconditional. The event is created before the reply has
  even been sent, so the other side has agreed to nothing yet, and the event is a
  guest-free note to self. The user renames or clears it once the call is confirmed.
- **Time** - the first-choice slot, in the user's timezone, for the duration the call
  actually needs.
- **Description** - the company folder and the document that motivates the call (the
  proposal, the last summary), plus the alternative slots the reply offers, so the user
  sees at a glance what the other side was given.

A guest-free `[hold]` stays yours to move: a later run of this skill can shift it to
whichever slot the other side picks. Once the user sends the real invitation, the event
has attendees and this skill leaves it alone for good.

## Draft the reply

The reply lives inside a **live thread**, so it continues the conversation:

- Mirror the incoming message: language, greeting register, tone, length, punctuation.
- In a running exchange, open on the content. Do not re-introduce yourself or restate
  what was just said.
- Propose the slot **with a timezone** and offer the alternatives in one line. Give the
  other side an easy yes.
- Say who sends the invitation, so nobody waits on the other: the user sends it once the
  time is agreed.

## Save

- **Reply** - append it to the existing thread in
  `<company>/communications/<YYYY-MM-DD> - To <Name> - <Subject>.md`, verbatim in a
  fenced block, frontmatter `status: draft`. One thread, one file: append, never fork a
  parallel file for the same conversation.
- **Record** - `Edit` the lead or client record with a dated line naming the proposed
  slot and wikilinking the reply (`[[<YYYY-MM-DD> - To <Name> - <Subject>]]`), and
  refresh `updated:`.
- **No workspace** - return the reply in chat, describe the hold, and offer to file both
  once a workspace exists.

## Quality gate

Before delivering, check:

- The hold exists on the user's own calendar, has **no guests**, and its title opens with
  `[hold]`.
- No event with attendees was created or modified. If one stood in the way, the run
  stopped, said why, and left it to the user.
- The calendar tool came from the **Calendar** row of `Practice Workspace - Integrations.md`,
  and no vendor was assumed. With no row and no connected calendar, the slots came from
  the user and the skill still delivered a reply.
- The reply proposes a specific slot with a timezone, and every alternative it offers is
  genuinely free in the calendar.
- Nothing was sent. The reply is a draft; the user is the send mechanism.
- The reply was appended to the existing thread.
- The duration matches what the call is for, and the record links the reply.

## Hands off to

- `/fcxo-call-prep` builds the one-page brief before the call.
- `/fcxo-call-summary` records what was decided after it.
- `/fcxo-log-message` files the other side's answer when it arrives, so the thread stays
  the durable timeline.
