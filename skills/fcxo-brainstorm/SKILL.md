---
name: fcxo-brainstorm
description: "Interview the user relentlessly about a plan, decision, or offer, checkpointing every answer to a capture file so nothing is lost. Use to stress-test a plan, run a brainstorm or discovery session, or when the user says 'grill me'."
argument-hint: "<topic> [client/engagement, if it belongs to one]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-brainstorm – grill the user, checkpoint everything

Relentlessly interview the user about every aspect of the topic until you reach shared
understanding. Walk down each branch of the decision tree, resolving dependencies one
by one. The real goal is to **extract what's in their head into a durable, organized
markdown file** so nothing is lost as context fills up.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` for where things live.
Read the user's role silently from `me/*- Profile.md` and let it shape the questions: a
CTO gets grilled on architecture, delivery, and risk; a CMO on audience and pipeline;
a CFO on cash, pricing, and commitments.

## The capture file is the whole point

Long interviews fill up context. If you hold answers only in your head, you will
eventually misremember, conflate, or drop something. So you **checkpoint to disk after
every single answer**. The file is the source of truth. Never make the user ask you to
save progress.

## Setup (do this BEFORE the first question)

1. **Pick the capture location** by topic:
   - About a specific client or engagement →
     `clients/<slug>/engagements/<id>/notes/<YYYY-MM-DD> - <Topic> Brainstorm.md`.
     For a client-wide topic that fits no engagement, use
     `research/<YYYY-MM-DD> - <Client> <Topic> Brainstorm.md` and link the client
     profile.
   - About the practice itself – positioning, offers, strategy, content, anything else →
     `research/<YYYY-MM-DD> - <Topic> Brainstorm.md`.
   - **No workspace** → run standalone: keep the same discipline in an artifact or a
     file the user names, and offer to move it into the workspace once one exists.
2. **Create the file immediately** with frontmatter and a header: title, date, the goal
   of the session, and an empty "Open flags" section. When the topic is scoped to a
   client or engagement, link it per the wiki convention in `practice-workspace.md`
   (`[[<Client> - Profile]]` and, if it fits one, `[[<Client> - <Descriptor> Engagement]]`)
   so the capture connects to its subject.

   ```yaml
   ---
   type: brainstorm
   status: active
   updated: <YYYY-MM-DD>
   ---
   ```

3. **Tell the user where you're saving**, in one line. Then ask Q1.

## The checkpoint rule (non-negotiable)

After EVERY user answer, BEFORE you ask the next question:

- Append a structured entry to the capture file: the question topic, the key facts and
  decisions from their answer (in their words where the wording matters), and any flags
  (things they couldn't answer plus who should).
- Update or correct earlier entries if a later answer changes them.
- Only then ask the next question.

Never batch multiple answers into one write. Checkpoint one answer at a time. The point
is that if context is lost at any moment, the file already holds everything said so far.

## Interview method

- Ask **one question at a time**. For each, provide your **recommended answer** (your
  best inference from the workspace – profile, positioning, offers, the client's
  engagement files) so the user can simply confirm, correct, or redirect.
- Resolve dependencies in order: settle the upstream decision before the ones that
  depend on it.
- If a question can be answered by **reading a workspace file or a doc the user hands
  you**, read it and only surface what's net-new.
- When the user **can't answer** something, capture it as a flag with the right owner
  and move on. Don't stall.
- Keep going until the user says you're done, or you've covered every branch. Offer a
  completeness backstop near the end ("anything we haven't touched?").

## Capture file structure

```
# {Topic}: Brainstorm / Discovery Notes
Date: {date} · Goal: {one line}

## Summary / key decisions
(running synthesis, updated as you go)

## Q&A log
### Q1 - {topic}
- Asked: {question}
- Captured: {facts, decisions, in their words where it matters}
- Flags: {open item -> owner}
...

## Open flags (pending input)
- {item} -> {who can answer}
```

## At the end

1. Do a final read of the capture file for contradictions or gaps and reconcile them.
2. Give the user a short recap: what's captured, what's still flagged, and the
   suggested next step.
3. **Promote what matured.** If the session produced material that belongs in a
   standing file, offer to fold it in, keeping their own words and never polishing them
   into marketing copy (`${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md`): positioning →
   `me/*- Positioning.md`, offers and rates →
   `me/*- Offers.md`, an engagement decision → that engagement's `notes/` or a
   `/fcxo-decision-report`, a post idea → `content/`. The raw capture file stays where
   it is as the record.

## Used by onboarding

`/fcxo-init` (and any future onboarding flow) hands off here when the user can't yet
articulate their positioning, offers, or service in a batched ask. Run the same
interview scoped to "your practice", capture to `research/`, then seed
`me/*- Positioning.md` and `me/*- Offers.md` from the captured words. Per the wiki
convention in `practice-workspace.md`, the seeded `me/` files cross-link the owner
profile (`[[<Owner> - Profile]]`) so the onboarding output joins the graph.
