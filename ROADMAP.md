# Roadmap

Working backlog for the FCxO Toolkit. The shipped 0.1.0 set is the 24 skills in the
README. This file tracks what comes next and the open release tasks, so nothing is lost.

## Release follow-ups

Done as part of public-release prep (repo `ainova-systems/fcxo-toolkit`):

- [x] `.claude-plugin/marketplace.json` catalog (marketplace name `fcxo`, plugin source `.`).
- [x] `plugin.json` `homepage`, `repository`, and `author.url` fields.
- [x] CHANGELOG reference-link footer for the `v0.1.0` tag.
- [x] Least-privilege pass over every skill's `allowed-tools`: web tools only where the
      body genuinely fetches, `Edit` present exactly where an existing file is modified,
      and no `Bash` anywhere - the plugin is shell-free by design.
- [x] Public repo created and pushed with a clean history.

Remaining (owner action): tag `v0.1.0`. After the tag, submit the plugin to the
Anthropic community directory (public repo required; automated validation) so it
surfaces in the in-product Discover tab and the claude.com/plugins catalog that
Claude Cowork users browse.

## Planned skills (v2)

| Skill | What it does | Why it matters | Priority |
|-------|--------------|----------------|----------|
| `fcxo-assessment` | Structured practice/area assessment that composes `fcxo-research-form` + `fcxo-report` | A repeatable diagnostic some engagements open with. Deferred until the two building blocks have seen real use: its value hangs on a proven method, and composing unproven parts risks confident-but-wrong output. The full heavy engine (multi-agent repo scan + verification) stays an internal tool, not part of this shell-free plugin. | Medium |

Shipped out of the original v2 list in 0.1.0: `fcxo-proposal`, `fcxo-onboarding-plan`,
`fcxo-status-update`, `fcxo-renewal-review`, `fcxo-call-prep`, `fcxo-case-study`,
`fcxo-research-form`, `fcxo-report`; `fcxo-network-touch` shipped as the `touch` mode
of `fcxo-outreach`.

## Design constraints for new skills

- Stay lean: a new skill must answer a real, recurring job. Prefer adding a mode to an
  existing skill over a near-duplicate (e.g. payment reminders as a mode of `fcxo-invoice`,
  capacity tracking folded into `fcxo-pipeline-review`, network nurture as the `touch`
  mode of `fcxo-outreach`).
- Standalone-first and draft-only still apply.
- Read context from the practice workspace; write output to the destination in
  `references/practice-workspace.md`.
