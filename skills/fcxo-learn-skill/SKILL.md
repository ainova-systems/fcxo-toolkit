---
name: fcxo-learn-skill
description: "Build a new reusable skill from anything the user brings - a goal or question, a sample output or SOP, another skill as reference, a URL or post about how others do it, or the task just done this session. Researches how others do it, finalizes scope with a short interview, and writes a standards-compliant SKILL.md. Use when the user says learn a skill, build me a skill, turn this into a skill, make a skill from this doc/example/url, or save what we just did as a skill."
argument-hint: "[anything: a goal, a path, a URL, a reference skill, or 'what we just did']"
allowed-tools: Read, Write, Edit, Glob, WebFetch, WebSearch, Agent
---

# fcxo-learn-skill – turn anything into a reusable skill

The toolkit's skill builder. The user brings whatever they have - a goal, content, a
reference, a link, or a task just demonstrated in the chat - and this skill researches
how it is done well, finalizes the scope with a short interview, and writes a
standards-compliant `SKILL.md`. The unit of automation in this toolkit is the skill;
whether it runs on demand or on a schedule is an attribute of the skill, decided here.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` for the conventions a
generated skill should follow.

## 1. Take any input

Accept whatever the user provides; read files and URLs with the file and web tools (never
shell-walk):

- a **goal or question** ("a skill that turns a call into a follow-up", "how would I automate X");
- **content / examples**: a sample output to reproduce, an SOP, a pasted procedure;
- **a reference skill**: another `SKILL.md` (a path or pasted) to learn the shape from;
- **a URL or post**: an article or thread about how a task is done via skills or AI;
- **the task just done this session**: the conversation itself is the demonstration.

Any mix is fine. If nothing concrete is given, ask one question for the goal, then proceed.

## 2. Research how others do it (parallel subagents)

Before writing, study the pattern. Spawn a few **parallel research subagents** (or, if
subagents are unavailable, do this inline with WebSearch / WebFetch) to gather in
parallel: how this task or skill is done well elsewhere, similar published skills and
their structure, and whatever the user's referenced URL or skill reveals. Keep it bounded
to a handful of focused searches. Synthesize the best practices and the common shape into
notes you build from. This is what lifts the result above the single example the user
happened to bring.

## 3. Draft the skill model

From the input plus the research, write down: purpose, the **trigger** (when to use + the
phrases the user would say), **inputs**, **atomic steps**, **output format**, **where
output is saved**, **rules / constraints**, and whether it should run **on demand or on a
schedule**. Mark which concrete values are **variables** (client, date, amount) so the
skill generalizes beyond the one example it came from.

## 4. Finalize scope with a short interview

A questionnaire locks the scope. Diff the draft model against the load-bearing elements
and ask **only about the gaps** the input and research did not settle - usually the
trigger phrasing, edge cases, where output is saved, which values vary, and the schedule.
One question at a time, short. For a genuinely fuzzy task, hand off to `/fcxo-brainstorm`
for a deeper pass, then return here. (Deciding *what* is worth automating in the first
place is also `/fcxo-brainstorm`; this skill builds the one you have chosen.)

## 5. Decide the cadence

Tag the skill **on-demand** (fire when needed) or **scheduled** (runs on a cadence, e.g.
a Monday review), or partially (some runs manual). Scheduling infrastructure (cron, cloud
schedules) belongs to the user's harness: record the schedule intent in the skill and tell
the user how to wire it in their tool (e.g. a Cowork scheduled task).
Default to on-demand unless it must run even when the user forgets.

## 6. Write the SKILL.md (standards-guided)

Author a standards-compliant skill in the agentskills.io format - the same standard this
toolkit uses, so it is portable across Claude Code, Cowork, Codex and others:

- **Name it plainly. The `fcxo-` prefix is reserved** for the skills that ship with this
  toolkit, so the skill you write here gets a plain descriptive name
  (`weekly-lead-opportunity-scan`, `monday-invoice-run`). A skill named `fcxo-something`
  would collide with a toolkit skill on the next plugin update and the user's own work
  would be shadowed by it.
- **Check it is not a duplicate.** Before settling on the name, compare the skill model
  against the toolkit skills already installed (`/fcxo-help` lists them). If it does the
  same job under a different name, say so and offer to extend the existing skill, so the
  user is left with one skill to maintain and no near-twin to keep in sync.
- frontmatter: `name` and a concise `description` that **front-loads the trigger
  keywords** (models undertrigger and descriptions get truncated, so put the "use
  when..." phrases first);
- a fixed, scannable section order: **When to use**, **Inputs** (instruct the skill to
  ASK for any missing input), **Steps** (atomic, numbered, each with the *why* so it
  generalizes), **Output** + save location, **Rules**, and a short **self-check**;
- **No invented commands or tools.** Reference only actions and tools that actually exist
  in the user's harness; never fabricate a CLI command or a tool the skill cannot run.
  Frame tool use correctly: this toolkit is shell-free (file tools, plus web tools only
  where the skill genuinely researches the web).
- Generalize: abstract the example's specific values into named inputs. Keep `SKILL.md`
  under ~500 lines; push long reference material into a `references/` file beside it.
- If the task reads the practice workspace, wire it to `practice-workspace.md` (read
  context, write output to the right folder, wiki-link related files).
- **A generated skill inherits the toolkit's hard rules. Write them into its Rules
  section, in its own words, whenever they apply:**
  - **Outward communications are draft-only.** If the skill touches an email, a DM, a
    message, or anything else addressed to another person, it writes the draft into the
    workspace for the user to send. It sends nothing itself. A user asking for a skill that
    "confirms the call" or "sends the weekly follow-up" gets a skill that **drafts** the
    confirmation or the follow-up.
  - **The connector write boundary.** If the skill touches a connected tool, it reads
    `Practice Workspace - Integrations.md` to learn which tool the user has. It never names
    a vendor and never asks for a credential, an API key, or an OAuth flow. The one write
    it may make is a guest-free hold on the user's own calendar. Every other connected tool
    is read-only: read from it, draft into the workspace. A generated skill never sends,
    invites, notifies, or shares, and it never creates or modifies a calendar event that
    already has attendees, because the calendar service emails every attendee an update.
  - If the user's request needs a send to work at all, say so plainly, build the drafting
    half, and leave the send to them.

## 7. Stage for review, do not auto-activate

Mirror the toolkit's draft-only discipline as a write-approval gate. Ask once where the
user's harness reads skills from (e.g. the project's `.claude/skills/<name>/SKILL.md`, or
their personal skills folder) and write the draft there; if they want to look first, stage
it in the workspace's own skills folder, `skills/<name>/SKILL.md`, for review before it
goes live. Restate the trigger phrase so they can fire it. Never write a user's custom
skill into a shared or public location automatically.

## Validate

Run a quick baseline check: fire a real prompt with the new skill and confirm it triggers
and the output matches the intent. If it under-triggers, sharpen the description's trigger
phrases.

## Notes

- Goal-first or content-first, both work: bring an example and it extracts, bring a goal
  and it researches; usually it is a mix.
- Discovery of *what* to automate is `/fcxo-brainstorm`; this skill builds the chosen one.
