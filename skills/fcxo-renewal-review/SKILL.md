---
name: fcxo-renewal-review
description: "Prepare the renewal checkpoint for an active engagement: an evidence-backed results-vs-plan review for the client plus an honest internal read on whether and on what terms to renew. Use when the user wants a renewal review, a QBR or quarterly business review, to prepare a renewal conversation, or asks should I renew this client."
argument-hint: "<client/engagement> [period] [language]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-renewal-review - protect and expand recurring revenue

A fractional practice lives on renewals. This skill prepares the renewal or quarterly
checkpoint for one engagement and produces two things in a single run: the **internal
read** (is this engagement worth renewing, and on what terms) and the **client-facing
review** the user presents in the renewal / QBR conversation. It is the payoff of the
record-keeping the other skills do week by week.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md`. Pull the user's **role**
from `me/*- Profile.md` - it sets the lens (a CFO reads margin and cash discipline, a
CMO reads pipeline and brand movement, a CTO reads delivery and system health). If no
workspace exists, run standalone: ask for the engagement scope, what was promised, what
was delivered, and the rate, then return both parts in chat and offer to save them once
a workspace exists.

## Input

`$ARGUMENTS`: the client or engagement, optionally the review period, optionally a
language for the client-facing part (default: match the user). If the client has
several engagements, ask which one is up for renewal.

## Timing

A renewal conversation happens **before** the term expires, while the user still holds
the initiative. If the engagement file carries a term or end date, check it: the right
moment to run this review is **4-6 weeks ahead** of that date. If the date is close or
already past, say so plainly and adjust the talking points (the ask becomes a
continuation of work already running). `/fcxo-pipeline-review` surfaces engagements
approaching that window week by week.

## Gather the record

The workspace holds the whole engagement history. Read, in this order:

- **The engagement file** `clients/<slug>/engagements/<id>/*- Engagement.md` - scope,
  price, term, start date, status.
- **The onboarding plan**, if one exists (look in the engagement's deliverables and
  notes) - what the engagement promised at the start. This is the baseline the review
  measures against; without it, reconstruct the baseline from the engagement file's
  scope.
- **Status updates** in `clients/<slug>/communications/` - the week-by-week delivery
  record.
- **Deliverables** in `engagements/<id>/deliverables/` and **meeting notes** in
  `engagements/<id>/meetings/` - what actually shipped and what was discussed.
- **Invoices** for this client in `finance/` - what was billed against the scoped
  price.
- **Rates and offers** in `me/*- Offers.md` - the anchor for next-period pricing.

## Build the review

1. **Results vs plan** (client-facing) - what the engagement set out to do vs what
   happened, evidence-backed from the record: cite the deliverable, status update, or
   meeting note behind each result. Where the plan tracked KPIs, show the movement.
   Never invent numbers - if a result was not measured, mark it as unmeasured. Never
   estimate it.
2. **What changed in the client's context** (client-facing) - new goals, new
   constraints, team changes, budget shifts, drawn from meeting notes and
   communications. This sets up why the next period should look the way it does.
3. **The honest internal read** (internal) - value delivered vs effort spent, the
   effective rate vs the market and vs `me/*- Offers.md`, relationship health (pays on
   time, responsive, asks growing or shrinking), and the client's strategic value to
   the practice (referrals, case-study weight, portfolio fit).
4. **Options and recommendation** (internal) - renew as-is / expand / reshape
   (different scope or cadence) / step back or wind down. One recommendation, stated
   plainly, with the reasoning behind it.
5. **Proposed next-period scope and price** (client-facing) - anchored to
   `me/*- Offers.md` and consistent with the recommendation. A reshaped engagement may
   hand off to `/fcxo-proposal` for a full proposal document.
6. **Talking points** (internal) - how to run the conversation: lead with delivered
   value, name the ask (the renewal, the new price, the reshape), and anticipate the
   likely objections with an answer to each.

## Output and save

Write one markdown file with the two parts unmistakably separated: the **client-facing
review** first (results vs plan, context changes, proposed next period), then a marked
divider and the internal part under a heading such as `## Internal read - do not
share` (the honest read, options and recommendation, talking points). The user must be
able to copy the client-facing part without ever touching the internal read.

Save to
`clients/<slug>/engagements/<id>/deliverables/<YYYY-MM-DD> - <Client> Renewal Review.md`
with frontmatter `type: deliverable`, `status: draft`, `updated: <date>`. Per the wiki
convention in `practice-workspace.md`, link the client profile
(`[[<Client> - Profile]]`) and the engagement file
(`[[<Client> - <Descriptor> Engagement]]`). If no workspace exists, return both parts
in chat.

If the user wants a branded document for the QBR, render **only the client-facing
part** with `/fcxo-report` - the internal read never leaves the workspace. Apply the
copy principles to client-facing prose (read
`${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md`).

## Quality gate (before delivering)

- Every result in the client-facing review cites a file from the record or is marked
  as unmeasured; nothing is invented.
- The internal read is honest, including where the engagement underperformed.
- One recommendation, stated plainly.
- The proposed price is anchored to the offers file.
- The internal sections are unmistakably marked, and nothing internal leaks into the
  client-facing part.
