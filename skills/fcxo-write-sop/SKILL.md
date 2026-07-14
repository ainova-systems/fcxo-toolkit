---
name: fcxo-write-sop
description: "Use when the user says write down my process, document this procedure, turn this into an SOP, write a procedure for X, capture how I do this, or I do this every week and want it on paper. Reads the past outputs of the procedure to learn the format it already produces, asks only what those leave open, writes sop/<Procedure> - SOP.md, and says how to schedule it or turn it into a skill."
argument-hint: "[the procedure: a description, a note, or a pointer at its past outputs]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-write-sop – write down a procedure you already follow

Every fractional executive runs procedures from memory, or from a note somewhere. Written
down, a procedure can be scheduled, turned into a skill, or handed to someone else.
Unwritten, it stays in their head and only they can run it. This skill writes it down.

The SOP is the unit of work. Once it exists on disk, a scheduled task can follow it with
nothing installed, and `/fcxo-learn-skill` can read it and produce an installable skill.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` so the paths you name are real.

## 1. Take the procedure, loosely

Accept whatever the user brings:

- a description in the conversation ("every Monday I scan for companies worth approaching");
- a rough note they already keep somewhere;
- a pointer at past outputs of it ("I do this weekly, here are the last two reports");
- the task they have just finished in this session, which the chat already demonstrates.

If they name nothing, ask which recurring piece of work they want written down. One question.

## 2. Find the evidence before you ask anything

This is the step that makes the SOP worth having, and it happens before any question.

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

## 3. Ask only what the evidence leaves open

One batched round, short. The normal number is small, and a procedure with a couple of past
runs behind it settles nearly everything. Do not interview. When the user cannot yet describe
the procedure at all, that is a discovery problem: hand off to `/fcxo-brainstorm`, then come
back here with what it captured.

## 4. Write the SOP

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
- **Output format** - the sections the result carries, drawn from the past outputs. Where the
  result saves, and how the file is named.
- **Boundaries** - what the procedure never does. Draft-only holds here as everywhere: a
  procedure produces research, drafts and records for the user to review, and it sends nothing.
- **Cadence** - a fact about the user's week ("Rowan runs this on Monday mornings"), stated in
  the body.

Write wikilinks to the files the procedure reads, so the SOP joins the graph.

## 5. Say what can be done with it now

Two lines, at the end. This is the point of having written it, and most users will not know it.

- **Schedule it.** A scheduled task can follow this SOP directly. Nothing is installed and
  nothing is uploaded: the task reads the file and does the work on the cadence they pick.
- **Turn it into a skill.** `/fcxo-learn-skill` reads this SOP and writes a `SKILL.md` they
  add to their Claude account and call with `/`.

The difference, in one sentence: a schedule runs the procedure without them, and a skill lets
them run it whenever they want, from anywhere.

## Rules

- **Shell-free.** File tools only (`Read`, `Write`, `Edit`, `Glob`).
- **Record the procedure they have.** If a step looks wrong to you, say so in one line and let
  them decide. The SOP describes what they do.
- **No workspace.** Ask the few facts you need, return the SOP in chat, and offer to save it
  once a workspace exists.

## How this differs from its neighbours

- `/fcxo-brainstorm` interviews the user to work out something they have not settled yet.
- `/fcxo-write-sop` records a procedure they already follow.
- `/fcxo-learn-skill` turns a procedure into an installable skill.
