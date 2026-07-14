# Workflows – the sequences a practice runs

Each skill does one job. This file says which skill follows which, and which file one
leaves behind for the next one to pick up. `practice-workspace.md` is the source of truth
for the layout; this file is the source of truth for the order.

**Nothing here is required.** Every skill runs on its own, in any order, and a skipped step
breaks nothing: when a skill cannot find the file it would have read, it asks you for what
it needs and carries on. The sequences matter because the files accumulate. A call summary
feeds the status update, the updates feed the renewal review, the review feeds the case
study, and the case study feeds the next proposal.

`<company>` below is `leads/<slug>/` before a contract and `clients/<slug>/` after one. The
folder keeps its shape; only its parent changes.

## 1. Set up, once

`/fcxo-init` → `/fcxo-brand` → `/fcxo-setup-template`

| Skill | Reads | Writes |
|---|---|---|
| `/fcxo-init` | you: name, role, what you sell, offers and rates, billing details | the workspace: `Practice Workspace - Guide.md`, `Practice Workspace - Integrations.md`, `me/<Owner> - Profile.md` / `- Positioning.md` / `- Offers.md`, `templates/`, `design/DESIGN.md` |
| `/fcxo-brainstorm` | the workspace | `research/<date> - <Topic> Brainstorm.md`, and seeds positioning and offers from your captured words |
| `/fcxo-brand` | `design/DESIGN.md` | the same file: accent color, logo, fonts, billing block |
| `/fcxo-setup-template` | a layout you designed | a `templates/<Doc>.md` + `design/<Doc>.html` pair |

**Hand-off.** `me/<Owner> - Positioning.md` and `- Offers.md` are what every later skill
reads to sound like you and to price like you. `design/DESIGN.md` is what every document
reads to look like you. Init writes both with a clean default, so the rest works before you
touch them. When the positioning is hard to write cold, `/fcxo-brainstorm` interviews you
and fills it.

## 2. A new company appears

`/fcxo-prospect-research` → `/fcxo-qualify` → `/fcxo-outreach`

| Skill | Reads | Writes |
|---|---|---|
| `/fcxo-prospect-research` | a name or URL, the web, your role and positioning | `leads/<slug>/<Lead> - Lead.md` (the brief) |
| `/fcxo-qualify` | the lead file, `me/<Owner> - Offers.md` | the same lead file: fit, urgency, budget signal, engagement shape, go/no-go |
| `/fcxo-outreach` | the lead file, your positioning | a draft in `<company>/communications/` |

**Hand-off.** The lead file. Research writes it, qualify adds the verdict to it, outreach
writes from it. When the company comes to you inbound, start at `/fcxo-log-message`: it
files the first email and opens the same folder.

## 3. A conversation runs

`/fcxo-log-message` → `/fcxo-reply` → `/fcxo-book-call`

| Skill | Reads | Writes |
|---|---|---|
| `/fcxo-log-message` | an email or DM you paste, or your connected mailbox | `<company>/communications/<date> - From <Name> - <Subject>.md`, appended to its thread |
| `/fcxo-reply` | the logged thread, and only it | one reply drafted into the same thread |
| `/fcxo-book-call` | your calendar, the thread | a guest-free hold on your own calendar, plus the reply that proposes the slot |

**Hand-off.** The thread in `<company>/communications/`. One thread is one file, appended
to, so the reply reads what was actually said and mirrors its tone and language. Every
outward message is a draft: you read it and you send it.

## 4. The deal converts

`/fcxo-call-prep` → `/fcxo-call-summary` → `/fcxo-proposal` → `/fcxo-report` → `/fcxo-new-client`

| Skill | Reads | Writes |
|---|---|---|
| `/fcxo-call-prep` | the lead file, the thread, the web | `<company>/meetings/<date> - <Company> Call Prep.md` |
| `/fcxo-call-summary` | your notes or a transcript | `<company>/meetings/<date> - <Company> Call Summary.md`, action items, and a follow-up draft in `communications/` |
| `/fcxo-proposal` | the lead file, the call summary, the thread, `me/<Owner> - Offers.md` | `<company>/proposals/<date> - <Company> Proposal.md` |
| `/fcxo-report` | the proposal markdown, `design/Proposal.html` | the branded `.html` sibling beside it, which you open and print to PDF |
| `/fcxo-new-client` | the accepted proposal | `clients/<slug>/` and `engagements/<id>/<Client> - <Descriptor> Engagement.md` carrying the scope, price, and cadence |

**Hand-off.** The call summary carries what the buyer said in their own words into the
proposal, and the proposal's scope and price carry into the engagement file. That engagement
file is what the delivery skills and the invoice read for the rest of the relationship.

## 5. The engagement runs

`/fcxo-onboarding-plan` → `/fcxo-status-update` → `/fcxo-renewal-review`, with
`/fcxo-decision-report`, `/fcxo-research-form`, `/fcxo-report` and `/fcxo-invoice` alongside.

| Skill | Reads | Writes |
|---|---|---|
| `/fcxo-onboarding-plan` | the engagement file | a 30-60-90 plan in `engagements/<id>/deliverables/` |
| `/fcxo-status-update` | the engagement file, the plan, meetings, deliverables | a client update draft in `clients/<slug>/communications/`, plus a private scope-creep check against the proposal's scope |
| `/fcxo-research-form` | the engagement | a questionnaire spec in `engagements/<id>/notes/`, and the findings once responses come back |
| `/fcxo-decision-report` | the engagement record, survey findings, your role lens | a decision memo in `engagements/<id>/deliverables/` |
| `/fcxo-report` | deliverables, call summaries, notes, findings | the composed report and its branded `.html` sibling |
| `/fcxo-renewal-review` | the engagement file, the status updates, the deliverables | the client-facing results-vs-plan review, plus an internal read on whether to renew |
| `/fcxo-invoice` | the engagement's rate and terms, `design/DESIGN.md` | an invoice in `finance/`, as HTML you print to PDF |

**Hand-off.** The onboarding plan sets the goals the status updates report against, and the
status updates are the evidence the renewal review runs on. Skip the updates and the
renewal conversation has nothing behind it but memory.

## 6. The practice runs

`/fcxo-pipeline-review` → `/fcxo-case-study` → `/fcxo-post`

| Skill | Reads | Writes |
|---|---|---|
| `/fcxo-pipeline-review` | every lead, client, and engagement in the workspace | `research/<date> - Pipeline Review.md`: what to chase, what to protect, what to drop |
| `/fcxo-case-study` | a finished or milestone-passing engagement: its deliverables, status updates, renewal review, call summaries | `content/<date> - <Descriptor> Case Study.md`, anonymized |
| `/fcxo-post` | the case study or any topic, `me/<Owner> - Voice.md` | a post draft in `content/`, and durable feedback appended to your voice guide |

**Hand-off.** The case study is where delivery becomes acquisition: it is the social proof
`/fcxo-proposal` reaches for on the next deal, and the material `/fcxo-post` works from.
The pipeline review is what tells you which engagement is ripe for one.

## 7. The work that repeats

`/fcxo-write-sop` → schedule it, or `/fcxo-learn-skill`

A procedure you run by hand is the unit here. Write it down first, then decide how it should
run.

| Skill | Reads | Writes |
|---|---|---|
| `/fcxo-write-sop` | the procedure as you describe it, and the outputs you produced by hand the last few times you ran it | `sop/<Procedure> - SOP.md`, plus whatever configuration the procedure needs, beside it |
| `/fcxo-learn-skill` | an SOP (or a goal, a sample output, a URL, a reference skill, the task you just did) | `skills/<name>/SKILL.md` in your workspace, for you to upload |

**Write it.** `/fcxo-write-sop` reads your past outputs of the procedure, because they are
the specification of the output format: whatever the next run produces has to look like
them. The result is one file in `sop/`, in your words, that a person or an agent can follow.

**Schedule it.** A scheduled task whose prompt says "follow `sop/<Procedure> - SOP.md`" runs
the procedure on a cadence. Nothing is installed, nothing is built, and it works today.

**Build a skill from it.** `/fcxo-learn-skill` reads the same SOP and writes
`skills/<name>/SKILL.md`. To use it, compress the skill folder into a ZIP whose root is that
folder and upload it at **Customize -> Skills -> "+" -> Create skill -> Upload a skill**.
The folder in your workspace is the source; your Claude account is where the skill runs, and
where `/` finds it.

**The difference.** A schedule runs the procedure without you, on a cadence. A skill runs on
demand, wherever you are, whenever you call it. Same procedure, two ways to operationalize
it, and one does not replace the other.

**Hand-off.** The SOP is what both read. Name the skill for what it does
(`lead-opportunity-scan`). The schedule owns the cadence, so that one name holds whether the
procedure runs weekly, every second week, or on demand. The `fcxo-` prefix is reserved for
the toolkit, so a skill you build never shadows one that ships.
