---
name: fcxo-onboarding-plan
description: "Build a 30-60-90 plan (Diagnose - Design - Deliver) for a new engagement's first quarter, grounded in its actual scope and framed in the user's role lens. Use when the user wants an onboarding plan, a 30-60-90, a first 90 days plan, a kickoff plan, or an engagement start plan."
argument-hint: "<client/engagement> [start date] [language]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-onboarding-plan - the first-quarter plan a client expects at kickoff

Produce the near-universal month-one deliverable: a 30-60-90 plan (Diagnose - Design -
Deliver) that structures the engagement's first quarter. Clients judge a fractional
executive in the first weeks, so the plan pairs a clear diagnostic path with one or two
visible early wins - credibility compounds from there.

This skill runs naturally right after `/fcxo-new-client` has created the engagement:
point it at that engagement and it turns the recorded scope into a working plan.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md`. Pull the user's **role**
from `me/*- Profile.md` (frontmatter `role:`); it decides which conversations, KPI
families, and vocabulary the plan carries. If no workspace exists, run standalone: ask
for the role, the engagement scope, and the start date, and return the plan in chat.

## Input

`$ARGUMENTS`: the client or engagement, optionally a start date and a language
(default: match the user).

## Step 1 - Read what the engagement already says

The plan must serve **this** scope, not a generic template. Before asking anything,
read what the workspace holds:

- **Engagement file** `clients/<slug>/engagements/<id>/*- Engagement.md` - scope,
  goals, price, cadence, start date. This is the spine of the plan.
- **Client profile** `<Client> - Profile.md` - company, industry, key contacts.
- **Discovery calls** in `engagements/<id>/meetings/` - what the client said is broken,
  who was in the room, what was floated.
- **The proposal** in `clients/<slug>/documents/`, or the lead file
  `leads/<Client> - Lead.md` if one preceded the client - what was promised during the
  sale. The plan must honor those promises or name the change explicitly.

Then ask **one short batched question** covering only the genuine gaps: the start date,
the key stakeholders to meet, and any known constraints (budget freeze, hiring pause,
a deadline the client already lives with). Do not re-ask what the files answer.

## Step 2 - Build the plan

Structure the plan in six parts, each framed in the role's lens:

1. **Engagement frame** - the goal of the engagement in one sentence, and how success
   will be measured at day 90. If the engagement file states goals, use those words.
2. **Days 1-30: Diagnose** - the listening-and-baseline month:
   - **Stakeholder conversations** - name who to meet (from the profile and call notes;
     `TODO:` placeholders for roles the workspace does not name).
   - **Access and data needed** from the client - an explicit list (systems, dashboards,
     documents, meetings to join). This list doubles as the kickoff ask.
   - **Baseline the KPIs the role owns** - the role sets the families (a CMO baselines
     pipeline, CAC, brand reach; a CFO baselines cash, runway, controls; a CTO baselines
     delivery, reliability, team shape); the actual numbers come from the client during
     this month, never from this skill.
   - **One or two visible early wins in weeks 2-3** - small, real, chosen from the
     discovery notes. Early proof buys patience for the deeper work.
3. **Days 31-60: Design** - turn findings into a plan:
   - The findings-to-plan step: what the diagnosis showed, agreed with the client.
   - The decisions that will need making - name them; each significant one can become
     a `/fcxo-decision-report`.
   - Quick wins from month one shipped and communicated.
4. **Days 61-90: Deliver** - make it stick:
   - The roadmap beyond day 90, sequenced and owned.
   - The operating rhythm established - weekly status (via `/fcxo-status-update`),
     the recurring meetings that survive after this quarter.
   - KPI targets set against the month-one baseline.
5. **Working agreement** - cadence and availability (from the engagement file), how
   decisions get made and by whom, how the user and the client escalate.
6. **What the client provides** - access, introductions, data - each item with an
   owner and a date. This section is what the user reads out at the kickoff call.

## Honest craft

- **Never fabricate KPIs, numbers, or org specifics** the workspace does not hold.
  Use named `TODO:` placeholders the user fills (`TODO: baseline CAC from last
  quarter`, `TODO: name of the VP Sales`), same convention as the sibling skills.
- **Role lens, not faked depth.** The role chooses which KPI families and stakeholders
  appear; it does not license inventing domain detail the user has not supplied.
- Apply `${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md` to the prose: no
  em-dash, no AI-slop, plain professional language. The client will read this.

## Output and save

If a workspace exists, save to
`clients/<slug>/engagements/<id>/deliverables/<YYYY-MM-DD> - <Client> Onboarding Plan.md`
with frontmatter `type: deliverable`, `status: draft`, `updated: <date>`. Per the wiki
convention in `practice-workspace.md`, link the plan to `[[<Client> - Profile]]` and to
its engagement file (`[[<Client> - <Descriptor> Engagement]]`), and add a link to the
plan from the engagement file so the graph connects both ways. Otherwise return the
plan in chat.

When the user wants a branded, client-facing document, render the finished plan with
`/fcxo-report` - this skill owns the content, not the rendering.

## Quality gate (before delivering)

- The plan serves this engagement's recorded scope; promises from the proposal or lead
  history are honored or their change is named.
- Days 1-30 contain at least one visible early win and an explicit access/data ask.
- Every number and org specific is sourced from the workspace or marked `TODO:`.
- The role lens shows in the KPI families and stakeholder list, with no invented
  domain depth.
- A client reading only the frame and the day 1-30 section knows what happens next
  week.
