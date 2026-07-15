---
name: fcxo-write-sop
description: "Use when the user says write down my process, document this procedure, capture how I do this, turn this into an SOP, or I do this every week and want it on paper, AND when the user hands a bare goal with no procedure yet: write me an SOP for X, set up a procedure for onboarding a client, automate lead generation. It grades what it is given: rich evidence gets captured into sop/<Procedure> - SOP.md with few questions and no web research; a bare goal gets researched and built into the same file as a proposed procedure the user refines by running it."
argument-hint: "[the procedure: a description, a note, a pointer at its past outputs, or a bare goal]"
allowed-tools: Read, Write, Edit, Glob, WebFetch, WebSearch
---

# fcxo-write-sop – write down a procedure, or build one you want

Every fractional executive runs procedures from memory, or from a note somewhere. Written
down, a procedure can be scheduled, turned into a skill, or handed to someone else.
Unwritten, it stays in their head and only they can run it. This skill writes it down when the
procedure already exists, and builds it when the user has a goal but no procedure yet. Either
way the unit of work is the SOP, and it lands in the same place.

Once the SOP exists on disk, a scheduled task can follow it with nothing installed, and
`/fcxo-learn-skill` can read it and produce an installable skill.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` so the paths you name are real.

## 1. Take the input, loosely

Accept whatever the user brings:

- a description in the conversation ("every Monday I scan for companies worth approaching");
- a rough note they already keep somewhere;
- a pointer at past outputs of it ("I do this weekly, here are the last two reports");
- the task they have just finished in this session, which the chat already demonstrates;
- a bare goal with no procedure behind it yet ("write me an SOP for onboarding a client",
  "automate lead generation").

If they name nothing, ask which piece of work they want written down. One question.

## 2. Grade what you were given

Do the least work that yields a real SOP. Place the input on one of three rungs, in order, and
stop climbing the moment a rung fits.

- **The procedure already exists** - past outputs, a note, or a clear description. Capture it
  from that evidence (§3), ask only what the evidence leaves open (§4), and stop. No web
  research. This is the fast lane and the common case for an established practice.
- **A described procedure with gaps** - the user describes most of it, but a step or a source
  is missing. Capture the described part from the evidence, then research or ask to fill the
  named gaps, and nothing more.
- **A bare goal, no procedure yet** - there is nothing to capture, so build it (§5): research
  how the work is done, pull the user's own specifics from the workspace, ask what is
  genuinely open, and write the SOP as a proposed procedure the user owns.

The guardrail, stated plainly: rich evidence means capture and stop. If the evidence answers
the procedure, a written practice never triggers a web-research pass. Escalation to research
happens only when the input is genuinely thin.

## 3. Capture: find the evidence before you ask anything

This is the step that makes a captured SOP worth having, and it happens before any question.

- **Past outputs are the specification of the output format.** If the procedure has produced
  dated files before, they are usually in `research/`, sometimes in the company's own folder.
  Read them. Their sections, their headings, their fields and their ordering are what the SOP
  must specify, because a future run has to produce something that matches what the user
  already has. Where they have a format, use theirs.
- **The workspace is the specification of the inputs.** Work out what the procedure reads
  when it runs: the owner's positioning and offers in `me/`, the ICP, the records under
  `leads/` and `clients/`, the previous output. Name those files by path.
- **The configuration, when there is one.** Many procedures carry settings beside them: the
  sources they may use, the exclusions, the thresholds. Find that note and name it.

All of this is discoverable by name and by folder, so none of it is a question.

## 4. Ask only what the evidence leaves open

One batched round, short. The normal number is small, and a procedure with a couple of past
runs behind it settles nearly everything. Do not interview. When the user cannot describe the
work at all, that is a discovery problem: hand off to `/fcxo-brainstorm`, then come back here
with what it captured.

## 5. Build: research and the user's specifics, for a thin goal

Reach this step only on the bare-goal rung. There is no procedure to capture, so assemble one.

- **Research how the work is done.** Use `WebSearch` and `WebFetch` to learn how this kind of
  procedure is run well, so the steps reflect real practice and not a guess.
- **Ground it in the user's own workspace.** A researched SOP fits the user's own practice.
  Read the owner's positioning and offers in `me/`, the ICP, and the records under `leads/`
  and `clients/`, and build the steps around those. Never fabricate the user's specifics,
  their ICP, their prices, or their sources; pull them from the workspace, or ask.
- **Ask what neither research nor the workspace answers.** One short batched round for what is
  genuinely open. Hand off to `/fcxo-brainstorm` only when the user cannot describe what they
  want at all.

## 6. Write the SOP

Save it to `sop/<Procedure> - SOP.md`. `sop/` holds the procedures the user follows; `me/`
holds identity (profile, positioning, offers, voice). A procedure is not identity.

**The title says what the procedure does, and carries no cadence word.** "Lead Opportunity
Scan" names the work. Capabilities and cadence are separate layers: the schedule owns the
cadence, so the title holds whether they run it weekly, every second week, or twice in one
week.

```yaml
---
type: sop
updated: <YYYY-MM-DD>
---
```

The body carries:

- **What it produces** - one line, so the reader knows what a run gives them.
- **Steps** - numbered, short, in the user's own words. A procedure a person actually follows
  is five or six steps. Twenty steps means it is two procedures, so say so and split it.
- **What it reads** - the real files and folders, by path.
- **Output format** - the sections the result carries, drawn from the past outputs where the
  procedure has them. Where the result saves, and how the file is named.
- **Boundaries** - what the procedure never does. Draft-only holds here as everywhere: a
  procedure produces research, drafts and records for the user to review, and it sends nothing.

**No cadence in the SOP.** The SOP is what the procedure does. When it runs, and how often, is
set by the schedule or by whoever runs it, so the same file works run by hand or on any
cadence. Do not write a day or a frequency into the SOP, the same way the title carries no
cadence word. If the user mentions one ("I do this on Mondays"), it is context for the schedule
they set later, not a line in the procedure.

For a built SOP, add one line near the top: this is a proposed procedure drawn from how the
work is done plus the user's practice, and it sharpens as they run it. A captured SOP does not
need that note, because it already describes what the user does.

Write wikilinks to the files the procedure reads, so the SOP joins the graph.

## 7. Say what can be done with it now

Two lines, at the end. This is the point of having written it, and most users will not know it.

- **Schedule it.** A scheduled task can follow this SOP directly. Nothing is installed and
  nothing is uploaded: the task reads the file and does the work on the cadence they pick.
- **Turn it into a skill.** `/fcxo-learn-skill` reads this SOP and writes a `SKILL.md` they
  add to their Claude account and call with `/`.

The difference, in one sentence: a schedule runs the procedure without them, and a skill lets
them run it whenever they want, from anywhere.

## Rules

- **Shell-free.** File tools plus inline web only (`Read`, `Write`, `Edit`, `Glob`,
  `WebFetch`, `WebSearch`). No shell, no sub-agents.
- **Record the procedure they have.** If a step looks wrong to you, say so in one line and let
  them decide. A captured SOP describes what they do.
- **No workspace.** Ask the few facts you need, return the SOP in chat, and offer to save it
  once a workspace exists.

## How this differs from its neighbours

- `/fcxo-brainstorm` is the deep discovery interview for something genuinely unformed. This
  skill can do light brainstorm-style questioning inline when a bare goal needs the user's
  specifics, and hands off to `/fcxo-brainstorm` only when the user cannot describe what they
  want at all.
- `/fcxo-write-sop` produces the SOP from any starting point: it captures a procedure the user
  already follows, or builds one from a bare goal.
- `/fcxo-learn-skill` turns a procedure into an installable skill. Its research half now
  overlaps this skill's build rung; that overlap is recorded in the Value Plan.
