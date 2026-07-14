---
name: fcxo-learn-skill
description: "Turn a procedure into a reusable skill you can call with a slash command. Its best input is a written SOP, and the path to one is a complete argument. Use when the user says learn a skill, build me a skill, turn this SOP into a skill, make a skill from this doc or example or url, or save what we just did as a skill. Researches how the task is done well, writes skills/<name>/SKILL.md, and tells the user how to upload it to their Claude account."
argument-hint: "[an SOP path, a goal, a URL, a reference skill, or 'what we just did']"
allowed-tools: Read, Write, Edit, Glob, WebFetch, WebSearch, Agent
---

# fcxo-learn-skill – turn a procedure into a skill

The toolkit's skill builder. It reads a procedure the user already follows, researches how
that kind of work is done well, and writes a standards-compliant `SKILL.md` into
`skills/<name>/` in the workspace. The user uploads that folder to their Claude account and
calls the skill with `/`, from anywhere, whenever they want.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` for the conventions a
generated skill follows.

## 1. Take the input. An SOP is the best one, and its path is a complete argument

`/fcxo-learn-skill sop/Lead Opportunity Scan - SOP.md` is everything this skill needs. A
written SOP already states the steps, the inputs, the output format and the boundaries, so
it settles almost every question the interview would otherwise ask. With one in hand, the
normal number of questions is **zero**.

From the SOP, take the procedure. Then go and find the rest yourself, because the SOP names
it and the workspace holds it:

- **the configuration** the SOP points at (the sources it may use, the exclusions, the
  thresholds), wherever the SOP says it lives;
- **the past outputs** of the procedure, the dated files the SOP names in `research/`. They
  are the specification of the output format: read the most recent ones and reproduce their
  shape;
- **the context the procedure reads** when it runs: the owner's `me/` files, the existing
  records under `leads/` and `clients/`.

Each of these is discoverable by name and by folder, so none of them is a question. If the
user has no SOP yet, `/fcxo-write-sop` writes one from the procedure they describe, and this
skill then works from that.

Other inputs work too, and any mix is fine. Read files and URLs with the file and web tools,
never a shell:

- **a goal or question** ("a skill that turns a call into a follow-up");
- **content or examples**: a sample output to reproduce, a pasted procedure;
- **a reference skill**: another `SKILL.md` to learn the shape from;
- **a URL or post** about how the task is done;
- **the task just done this session**: the conversation is the demonstration.

If nothing concrete is given, ask one question for the goal, then proceed.

## 2. Research how others do it (parallel subagents)

Before writing, study the pattern. Spawn a few **parallel research subagents** (or do this
inline with WebSearch / WebFetch if subagents are unavailable) to gather how this task is
done well elsewhere, what similar published skills look like, and whatever the user's
referenced URL or skill reveals. Keep it to a handful of focused searches. Synthesize the
common shape into notes you build from. This is what lifts the result above the single
example the user happened to bring.

## 3. Draft the skill model

From the input plus the research, write down: purpose, the **trigger** (when to use, and the
phrases the user would say), **inputs**, **atomic steps**, **output format**, **where output
is saved**, and **rules / constraints**. Mark which concrete values are **variables**
(client, date, amount) so the skill generalizes beyond the one example it came from.

## 4. Close the gaps with a short interview

Diff the model against the load-bearing elements and ask **only about what the input and the
research did not settle**. A written SOP with its configuration and a couple of past runs
settles all of them, so ask nothing and write the skill. When gaps do exist, they are usually
the trigger phrasing, the edge cases, where output is saved, and which values vary. One
question at a time, short. For a genuinely fuzzy task, hand off to `/fcxo-brainstorm` for a
deeper pass and come back. (Deciding *what* is worth automating is also `/fcxo-brainstorm`;
this skill builds the one already chosen.)

## 5. Name it for what it does

**The name says what the skill does.** "Lead Opportunity Scan" becomes
`lead-opportunity-scan`. Derive the name from the procedure's title in kebab-case, drop the
document words (`SOP`, `procedure`, `process`), and **strip any cadence word** (`weekly`,
`daily`, `monthly`, `nightly`) even when the source procedure's title carries one. The
schedule owns the cadence, so the same skill keeps that name whether it runs weekly, every
second week, or on demand. Capability and cadence are separate layers, and a cadence in the
name is a restriction that buys nothing.

**The `fcxo-` prefix is reserved** for the skills that ship with this toolkit. A skill named
`fcxo-something` would be shadowed by a toolkit skill on the next plugin update, so the skill
you write here gets a plain descriptive name.

**Check it is not a duplicate.** Compare the model against the toolkit skills already
installed (`/fcxo-help` lists them). If it does the same job under a different name, say so
and offer to extend the existing skill, so the user keeps one skill to maintain instead of
two near-twins to hold in sync.

## 6. Write the SKILL.md (standards-guided)

Author the skill in the agentskills.io format, the standard this toolkit uses, so it is
portable across Claude, Cowork, Codex and others:

- frontmatter: `name` and a concise `description` that **front-loads the trigger keywords**
  (models undertrigger and descriptions get truncated, so put the "use when..." phrases
  first);
- a fixed, scannable section order: **When to use**, **Inputs** (instruct the skill to ASK
  for any missing input), **Steps** (atomic, numbered, each carrying the *why* so it
  generalizes), **Output** plus save location, **Rules**, and a short **self-check**;
- **No invented commands or tools.** Reference only what exists in the user's harness. This
  toolkit is shell-free: file tools, plus web tools where the skill genuinely researches the
  web.
- Generalize: abstract the example's specific values into named inputs. Keep `SKILL.md` under
  ~500 lines and push long reference material into a `references/` file beside it.
- If the task reads the practice workspace, wire it to `practice-workspace.md`: read context,
  write output to the folder the table names, wiki-link the files it touches.
- **A generated skill inherits the toolkit's hard rules. Write them into its Rules section,
  in its own words, whenever they apply:**
  - **Outward communications are draft-only.** If the skill touches an email, a DM, a
    message, or anything else addressed to another person, it writes the draft into the
    workspace for the user to send. It sends nothing itself. A user asking for a skill that
    "confirms the call" or "sends the weekly follow-up" gets a skill that **drafts** the
    confirmation or the follow-up.
  - **The connector write boundary.** If the skill touches a connected tool, it reads
    `Practice Workspace - Integrations.md` to learn which tool the user has. It never names a
    vendor and never asks for a credential, an API key, or an OAuth flow. The one write it may
    make is a guest-free hold on the user's own calendar. Every other connected tool is
    read-only: read from it, draft into the workspace. A generated skill never sends, invites,
    notifies, or shares, and it never creates or modifies a calendar event that already has
    attendees, because the calendar service emails every attendee an update.
  - If the user's request needs a send to work at all, say so plainly, build the drafting
    half, and leave the send to them.

## 7. Write it to `skills/<name>/`, then tell the user how to upload it

**Write the skill to `skills/<name>/SKILL.md` in the workspace, always, without asking.** One
folder per skill, because that folder is the unit the user uploads.

This plugin writes files. It cannot install a skill into a Claude account, and that step
happens in the Claude interface. Say where the file went, give the trigger phrase, and hand
the user these three steps:

> Claude loads skills from your account, so the file on disk is the source and the upload is
> what makes it live.
>
> 1. Compress the `skills/<name>/` folder (right-click, **Compress** on macOS, **Send to >
>    Compressed folder** on Windows).
> 2. In Claude: **Customize -> Skills -> "+" -> Create skill -> Upload a skill**, and choose
>    the zip.
> 3. Toggle it on. Type `/` and it is there.

The file in `skills/` stays the durable source. To update the skill, edit that file and
upload it again.

## Validate

Fire a real prompt at the new skill and confirm it triggers and the output matches the
intent. If it under-triggers, sharpen the trigger phrases in the description.

## If all the user wants is a cadence, they need no skill

A skill is for running the procedure **on demand**, whenever they want, from anywhere. To
have the procedure run **without them, on a cadence**, a scheduled task can follow the SOP
directly, with nothing installed and nothing uploaded. Write the procedure down with
`/fcxo-write-sop`, then create the scheduled task in the Claude interface and point its
prompt at the SOP.

## Notes

- Goal-first or content-first, both work: bring an example and it extracts, bring a goal and
  it researches. Usually it is a mix, and an SOP is both.
- Discovery of *what* to automate is `/fcxo-brainstorm`; this skill builds the chosen one.
