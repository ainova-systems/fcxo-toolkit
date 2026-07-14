---
name: fcxo-decision-report
description: "Build a board-ready decision memo (situation, options, recommendation, risks) framed in the user's role lens. Use when the user wants a decision report, board memo, exec brief, or options analysis."
argument-hint: "<decision/topic> [client/engagement] [language]"
allowed-tools: Read, Write, Glob, WebFetch, WebSearch
---

# fcxo-decision-report – a memo a board can act on

Produce a crisp decision document: the kind a fractional executive puts in front of a
founder or board to drive one clear decision. This is a high-stakes deliverable – hold a
high bar.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md`. Pull the user's **role**
from `me/*- Profile.md` (it sets the lens and vocabulary) and the **engagement** context
from `clients/<slug>/engagements/<id>/` if relevant. If no workspace, ask for the role
and the decision context.

## Input

`$ARGUMENTS`: the decision or topic, optionally the client/engagement, optionally a
language (default: match the user). If the topic is vague, ask a **short batched** set
of questions: what decision is being made, who decides, what constraints (time, money,
people), and what "good" looks like.

## Principles

- **One decision per memo.** If several decisions are tangled, name them and focus on
  the primary one.
- **Evidence before assertion.** Every claim is backed by a fact, a number, or a named
  source. Research what you can verify; mark assumptions as assumptions.
- **The reader is busy and senior.** Lead with the recommendation. They read the
  summary and skim the rest.
- **Role lens.** Frame options in the terms that role owns (a CFO memo
  weighs cash and risk; a CMO memo weighs growth and positioning; a CTO memo weighs
  architecture and delivery). Do not assert domain specifics the user hasn't supplied or
  that can't be sourced.

## Structure

1. **Executive summary** – the situation in two sentences and the recommendation in one.
   A reader who stops here knows what to do.
2. **Context** – what prompted this decision; the constraints; what's at stake.
3. **Options** – 2-4 real options. For each: what it is, the cost/effort, the upside,
   the risk. Use a comparison table when the options share dimensions.
4. **Recommendation** – the chosen option and *why it wins* over the others. Be
   decisive; this is what the user is paid for.
5. **Risks & mitigations** – the honest downsides and how to manage them.
6. **Next steps** – concrete, owned, time-bound where possible.

## Output and save

Write the memo as clean markdown. Apply the copy principles for prose (no em-dash, no
AI-slop, plain professional language – read
`${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md`).

If a workspace exists, save to
`clients/<slug>/engagements/<id>/deliverables/<YYYY-MM-DD> - <Topic>.md` with frontmatter
`type: deliverable`, `updated: <date>`. Per the wiki convention in
`practice-workspace.md`, the deliverable links its client profile
(`[[<Client> - Profile]]`) and its engagement file
(`[[<Client> - <Descriptor> Engagement]]`). Otherwise return it in chat.

If the user wants a branded, client-facing document, render the finished memo with
`/fcxo-report` on the workspace document system (one self-contained HTML file the user
opens and prints to PDF) – this skill owns the content, and `/fcxo-report` owns the
rendering.

## Quality gate (before delivering)

- The recommendation is stated up front and is unambiguous.
- Every number/claim is sourced or flagged as an assumption.
- Every option is a real alternative the user would genuinely consider. No straw men.
- A busy reader gets the decision from the summary alone.
