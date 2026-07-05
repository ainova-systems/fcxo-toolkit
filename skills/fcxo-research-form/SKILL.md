---
name: fcxo-research-form
description: "Build a short, typed questionnaire as a platform-neutral form spec ready to paste into any forms tool, or synthesize the responses once they come back. Use when the user wants a research form, questionnaire, or survey, stakeholder questions, an employee survey, to collect input from the client's team, or to analyze survey responses."
argument-hint: "<topic> [client/engagement] [audience] | <responses file> to analyze"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-research-form - ask once, typed and short, then read the answers honestly

This is the "collect data" front of the delivery pattern: what the user cannot read
from the client's artifacts, they ask for once, structured. One skill, two modes:
**build the questionnaire** (Mode 1) and **synthesize the responses** (Mode 2).

The skill is deliberately platform-neutral and integration-free: it **never calls a
forms API**. Mode 1 produces a clean markdown form specification the user pastes into
their tool of choice - Google Forms, Typeform, or a plain email - so it works with
whatever the client's world already runs on.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md`. Pull the user's role
from `me/*- Profile.md` (frontmatter `role:`): it sets the question families - a CMO
surveys brand and funnel perception, a CTO surveys delivery and quality pain, a CFO
surveys process and control friction - without faking domain depth the user does not
have. No workspace? Run standalone: ask one short batched question (role, the decision
the data serves, the audience, the promise to respondents) and build the spec in chat.

## Input

`$ARGUMENTS`: a topic plus optionally a client/engagement and an audience (Mode 1), or
pasted responses / a path to an exported responses file (Mode 2). If the input is
responses or the user asks to analyze, run Mode 2; otherwise Mode 1. If empty, ask
which mode and for the topic or the responses.

## Mode 1 - build the questionnaire

### Step 1 - lock the purpose

Name the decision or deliverable the data serves: an assessment, an onboarding
diagnosis, a decision report. Every question must earn its place against that purpose;
a question that would not change the deliverable gets cut. Before drafting, read what
the workspace already answers - the engagement file, meeting notes, prior
deliverables - and never ask respondents for facts the artifacts already hold.

### Step 2 - define the audience and the promise

- **Audience** - leadership, a team, or customers. It changes vocabulary, sensitivity,
  and what respondents can actually know.
- **Promise** - anonymous or attributed. Decide with the user and **state it in the
  form's intro text**; respondents answer honestly only when they know the terms.

### Step 3 - draft the question set

- **Short**: respondents finish in ~10 minutes (roughly 10-15 questions).
- **Typed**: every question is one of `scale 1-5`, `single choice`, `multi choice`,
  `short text`. Choice questions carry their options.
- **Neutral**: non-leading phrasing ("How would you rate X" rather than "How much does
  X hurt you").
- **Ordered easy to sensitive**: warm-up and context first, friction and judgment
  later.
- **One open closer**: "What did we not ask that we should have?" (short text).
- The role lens picks the question families; the client's actual context from the
  workspace picks the specifics.

### Step 4 - output the form spec

Clean markdown, ready to paste into any forms tool:

```
# {Form title}
{Intro text: who is asking, what the data serves, the anonymity or attribution
promise, and the ~10-minute length}

## Section 1 - {name}
1. {Question} (scale 1-5: 1 = {low anchor}, 5 = {high anchor})
2. {Question} (single choice: A / B / C)
3. {Question} (multi choice: A / B / C / D)
4. {Question} (short text)
```

Close with a **one-line invite text** the user can send along with the form link,
written per `${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md`.

## Mode 2 - synthesize the responses

The user pastes responses or points to an exported file they saved into the workspace
(CSV or text); read it with the file tools. Find the Mode 1 questionnaire file for the
original questions and the decision they serve.

Aggregate honestly:

- **Counts first** - how many responded, against how many were invited if known.
- **Distributions per scaled question** - the spread, never only an average; a
  1-and-5 split is a finding, not a 3.
- **Free-text themes** - the recurring themes, quoted verbatim where the wording
  matters.
- **Splits and outliers flagged** - if leadership and the team disagree, or one
  respondent contradicts the rest, say so. Never smooth over a split, and never
  fabricate or extrapolate a respondent.
- **The "so what"** - tie the findings back to the decision from Mode 1: what the data
  says the deliverable should conclude, and what remains unknown.

The synthesis becomes engagement context: it feeds `/fcxo-report`,
`/fcxo-decision-report`, or the onboarding plan.

## Output and save

- **Form spec** →
  `clients/<slug>/engagements/<id>/notes/<YYYY-MM-DD> - <Client> <Topic> Questionnaire.md`
- **Synthesis** → same folder,
  `<YYYY-MM-DD> - <Client> <Topic> Survey Findings.md`, linking its questionnaire
  (`[[<YYYY-MM-DD> - <Client> <Topic> Questionnaire]]`).

Both carry frontmatter `type: survey`, `status: draft`, `updated: <date>`, and per the
wiki convention in `practice-workspace.md` link `[[<Client> - Profile]]` and the
engagement file (`[[<Client> - <Descriptor> Engagement]]`). Practice-wide research
with no client (say, surveying the user's own network) → `research/`, same naming.
No workspace → return the spec or synthesis in chat and offer to save it once a
workspace exists.

## Quality gate (before delivering)

- Every question earns its place against the locked decision; nothing is asked that
  the workspace artifacts already answer.
- The intro text states the anonymity or attribution promise; the set finishes in ~10
  minutes; every question carries a type, and choice questions carry their options.
- Phrasing is neutral and ordered easy to sensitive, with the open closer last.
- The synthesis reports real counts and distributions, quotes verbatim, and flags
  splits and outliers; no respondent is fabricated or smoothed over.
- The "so what" answers the decision the form was built for.
