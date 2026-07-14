---
name: fcxo-help
description: "List the FCxO Toolkit skills grouped by job, explain when to use each, and answer a stated goal with the right skill or the sequence of skills that gets there. Use when the user asks what can this do, list the skills, what skills are there, how do I use the toolkit, where do I start, what comes next, which skill for X, or how do I win this client / get paid / set up my practice."
argument-hint: "[optional: a goal, e.g. 'bill a client' or 'write a post']"
allowed-tools: Read, Glob
---

# fcxo-help – what the toolkit does and which skill to use

Give the user a clear map of the toolkit: every skill, grouped by the job it does, with
one line on when to use it, the recommended starting point, and - if they state a goal -
the skill or the sequence of skills that gets there. Read-only; this skill explains, it
does not run the others.

## Always read the live skills (never a hardcoded list)

The skill set changes over time, so never recite a memorized list - discover what
actually exists each time:

- `Glob ${CLAUDE_PLUGIN_ROOT}/skills/*/SKILL.md` and read each file's frontmatter `name`
  and `description`. That is the source of truth for what a skill is and when to use it.
- Read `${CLAUDE_PLUGIN_ROOT}/README.md` for the human grouping (Foundation, Find & win,
  Communicate, Deliver, Market, Run, Documents & brand) and the starting sequence.
- Read `${CLAUDE_PLUGIN_ROOT}/references/workflows.md` for the sequences: which skill
  follows which, and the file one leaves behind that the next one reads. That file is the
  source of truth for order, so never invent a sequence of your own.
- If a skill folder exists that the README does not list (or the README lists one with no
  folder), say so plainly. Same for a skill named in `workflows.md` with no folder.

## Present the map

Group the skills by job (from the README) and, under each group, show `/<skill>` plus the
one-line "use when..." taken from its description. Keep it scannable - one line per skill.
Lead with the starting point: `/fcxo-init` sets up the practice workspace, and every other
skill reads context from it and writes results back into it.

If the user has no workspace yet, say so and point them to `/fcxo-init` first.

## Recommend for a goal

If the user passed a goal (in `$ARGUMENTS`) or states one, first decide whether one skill
does the job or a sequence does.

- **One skill, one job.** Map it and say why in one line: "bill a client" -> `/fcxo-invoice`;
  "write a post" -> `/fcxo-post`; "log what a client emailed" -> `/fcxo-log-message`; "reply
  to a client" -> `/fcxo-reply`; "research a prospect" -> `/fcxo-prospect-research`; "turn
  this into a skill" -> `/fcxo-learn-skill`. If two skills fit, name the primary and the
  alternative.
- **A goal that spans several skills.** "Where do I start", "win this client", "get paid",
  "set up my practice", "what comes next" - answer with the **sequence** from
  `workflows.md`: the skills in order, the one to run now, and the file it leaves behind
  that the next one reads. Two or three lines, then stop. Do not restate the whole file.
- **Nothing fits.** Say so, and suggest `/fcxo-learn-skill` to build a skill for it.

Whichever way you answer, say once that the order is optional: any skill runs on its own,
and one that cannot find the file it would have read asks the user for what it needs.

## Notes

- This is the in-session guide; the same skills also show in the harness picker (`/`, `+`).
- Keep descriptions in the user's words - "use when..." phrasing - and free of internal jargon.
