---
name: fcxo-help
description: "List the FCxO Toolkit skills grouped by job, explain when to use each, and recommend the right skill for a stated goal. Use when the user asks what can this do, list the skills, what skills are there, how do I use the toolkit, where do I start, or which skill for X."
argument-hint: "[optional: a goal, e.g. 'bill a client' or 'write a post']"
allowed-tools: Read, Glob
---

# fcxo-help – what the toolkit does and which skill to use

Give the user a clear map of the toolkit: every skill, grouped by the job it does, with
one line on when to use it, the recommended starting point, and - if they state a goal -
a direct pointer to the right skill. Read-only; this skill explains, it does not run the
others.

## Always read the live skills (never a hardcoded list)

The skill set changes over time, so never recite a memorized list - discover what
actually exists each time:

- `Glob ${CLAUDE_PLUGIN_ROOT}/skills/*/SKILL.md` and read each file's frontmatter `name`
  and `description`. That is the source of truth for what a skill is and when to use it.
- Read `${CLAUDE_PLUGIN_ROOT}/README.md` for the human grouping (Foundation, Find & win,
  Communicate, Deliver, Market, Run, Documents & brand) and the starting sequence.
- If a skill folder exists that the README does not list (or the README lists one with no
  folder), say so plainly rather than hide the mismatch.

## Present the map

Group the skills by job (from the README) and, under each group, show `/<skill>` plus the
one-line "use when..." taken from its description. Keep it scannable - one line per skill.
Lead with the starting point: `/fcxo-init` sets up the practice workspace, and every other
skill reads context from it and writes results back into it.

If the user has no workspace yet, say so and point them to `/fcxo-init` first.

## Recommend for a goal

If the user passed a goal (in `$ARGUMENTS`) or states one, map it to the single best skill
and say why in one line. For example: "bill a client" -> `/fcxo-invoice`; "write a post"
-> `/fcxo-post`; "log what a client emailed" -> `/fcxo-log-message`; "reply to a client"
-> `/fcxo-reply`; "research a prospect" -> `/fcxo-prospect-research`; "turn this into a
skill" -> `/fcxo-learn-skill`. If two skills fit, name the primary and the alternative. If
nothing fits, say so and suggest `/fcxo-learn-skill` to build a new skill for it.

## Notes

- This is the in-session guide; the same skills also show in the harness picker (`/`, `+`).
- Keep descriptions in the user's words - "use when..." phrasing - not internal jargon.
