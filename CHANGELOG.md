# Changelog

All notable changes to the FCxO Toolkit plugin.

The format is based on [Keep a Changelog](https://keepachangelog.com/), and this
project adheres to [Semantic Versioning](https://semver.org/).

## [0.2.3] - 2026-07-15

### Added

- `Practice Workspace - Init.md` - the root file `fcxo-init` writes so the user can make the
  workspace self-orienting on their agent surface. Claude Cowork does not open a folder's
  files on its own, but it reads a project's Instructions at the start of every task, so the
  workspace map has to live there. This file holds the exact block to paste into the Cowork
  project's Instructions field: what the workspace is, where each folder sits, and the
  read-time rules (one company one folder, read before asking, draft only). Paste it once and
  the assistant finds and reads the right files without being told where they are. `fcxo-init`
  now writes it and, at the end of setup, tells the user to paste it in.

## [0.2.2] - 2026-07-15

### Changed

- `fcxo-write-sop` – **the flexible front door to an SOP.** It produces an SOP from any
  starting point, doing the least work that yields a real one. Given a procedure the user
  already runs, with past outputs, a note, or a clear description, it captures the SOP from
  that evidence and stops: if the evidence answers the procedure, it never opens a web
  search. Given a bare goal and no procedure yet, the SOP does not exist to capture, so it
  builds one - it researches how the work is actually done, pulls the user's own specifics
  from the workspace (positioning, offers, ICP, records) rather than inventing them, asks
  what is genuinely open, and drafts a proposed procedure the user owns and refines by
  running it. `WebFetch` and `WebSearch` join its allowed tools for the research rung, and it
  stays shell-free (no `Bash`, no subagents). Either way the result is the same file,
  `sop/<Procedure> - SOP.md`, which is then scheduled or built into a skill. Its research
  half now overlaps `fcxo-learn-skill`, which is unchanged.

## [0.2.1] - 2026-07-14

### Added

- `fcxo-write-sop` – write down a procedure the user already runs by hand. It reads the
  outputs of the last few runs, which are the specification of the output format, settles
  the steps in a short interview, and saves `sop/<Procedure> - SOP.md` with whatever
  configuration the procedure needs beside it.
- `sop/` – the workspace folder for the procedures the owner follows, one file each. It
  carries the toolkit's central loop: a procedure in `sop/` can be **scheduled** (a
  scheduled task follows the SOP, with nothing installed) or **built into a skill**
  (`/fcxo-learn-skill` writes `skills/<name>/SKILL.md`, which the user uploads and calls
  with `/`). A schedule runs the procedure without them; a skill lets them run it whenever
  they want. `me/` is identity, so a procedure and its configuration moved out of `me/` and
  into `sop/`. `fcxo-init` scaffolds the folder.

### Changed

- **A skill is named for what it does.** The workspace contract now states it: the schedule
  owns the cadence, so `lead-opportunity-scan` keeps its name whether it runs weekly, every
  second week, or on demand. Capability and cadence are separate layers, and a name that
  fixes the cadence is a restriction with no upside.
- **The install path for a built skill is stated honestly.** `skills/<name>/` is where a
  skill lives as **source**. It becomes usable by being uploaded to the user's Claude
  account: compress the skill folder into a ZIP whose root is that folder, then
  **Customize -> Skills -> "+" -> Create skill -> Upload a skill**. Cowork has no local
  skills folder, and `.claude/skills/` is a Claude Code convention that does nothing there.
  The toolkit is shell-free, so it builds no ZIP and installs nothing; the user does that
  step.
- **A company name is a complete argument.** The workspace contract now states the rule
  every document skill follows: the user names the company and the thing they want, and
  the skill resolves the rest - the company folder, the whole history inside it, the data
  template, the design template, the save path, the filename. A skill that asks for what
  the workspace already states has failed at the job the workspace exists for.
  (`references/practice-workspace.md`.)
- `fcxo-report` – **render-only is the default.** `/fcxo-report <Company> proposal` now
  resolves the company folder, finds the newest matching document inside it, renders it in
  `design/<Doc>.html` with the content unchanged, and saves the self-contained `.html`
  beside the markdown. Branded, self-contained, and beside-the-markdown stop being things
  the user asks for. Composing a report from engagement material is what it does when no
  source document exists.
- `fcxo-proposal` – a bare company name is the whole input. It reads every file in the
  company's `communications/` and `meetings/` in date order, and **the newest message
  governs**: a requirement the buyer added this morning lands in the proposal, and a next
  step the buyer has since withdrawn does not survive in it. A qualified folder answers
  everything a proposal needs, so the normal number of questions is zero. Draft-only is
  unconditional, so "do not send" never has to be said.
- `fcxo-log-message` – a pasted message is a complete instruction. The company is resolved
  from the sender's email domain first, then the correspondent, then the company name; a
  `Re:` subject continues the thread that carries the same subject without it.
- `fcxo-learn-skill` – a file path is a complete input. It reads the procedure's
  configuration and its previous dated runs (the specification of the output format) on its
  own, derives the skill name from the procedure's title, and stages the skill at
  `skills/<name>/SKILL.md` in the workspace by default rather than asking where it goes.

## [0.2.0] - 2026-07-14

### Added

- `fcxo-book-call` – turn "we should talk" into a call in the diary: read the calendar
  through the connector the workspace names, hold the time on the **owner's own calendar
  with no guests**, and draft the reply that proposes the slot into the company's
  `communications/`. **This is the first skill that writes to a connected tool**, so read
  the boundary before you upgrade: the hold is a private note to self and carries no
  guests, because adding a guest makes the calendar service email that person an
  invitation the user never sent. A skill never touches an event that already has
  attendees, for the same reason. The user sends the reply, and sends the real invitation
  once the time is agreed.
- `fcxo-setup-template` – install a document layout the user designed as a reusable
  document type: it writes the `templates/<Doc>.md` and `design/<Doc>.html` pair together
  and wires the layout to the workspace brand tokens, so the design follows the brand and
  a change of accent color carries through every document of that type. Design a document
  in Claude once, and every future document comes out in that layout.
- `Practice Workspace - Integrations.md` – the connector registry at the workspace root.
  The toolkit ships no connectors and asks for no credentials; it uses the ones the
  user's agent surface already provides. This file is the plain table mapping capability
  to connected tool, to the skills that use it, to the access they have. A skill that
  needs a connected tool reads this file to learn which one the user has, so a user on a
  different calendar changes one row and every skill follows. The write boundary lives
  here too: a guest-free calendar hold is the only write any skill makes through a
  connector, and every other connected tool is read-only.

### Changed

- **One company, one folder.** The workspace contract now keeps a company's record, its
  messages, its meetings, and its proposals together in one folder: `leads/<slug>/` before
  the contract, `clients/<slug>/` after it. The same folder moves when the deal is won, so
  nothing is renamed and no wikilink breaks.
- Proposals moved out of a flat `leads/` into `<company>/proposals/`, beside the company
  they belong to. A proposal is a document, and documents live in a document folder.
- The design system moved from `me/brand/` to `design/` at the workspace root. `me/` holds
  who the owner is; `design/` holds how their documents look.
- `templates/<Doc>.md` and `design/<Doc>.html` now pair by basename: the data half says
  what fields a document type has, the design half says how it looks, and a document skill
  writes against one while the render fills the other.
- `templates/Proposal.md` was missing while `fcxo-proposal` shipped; the workspace now
  seeds it, so the proposal type has both halves.

## [0.1.1] - 2026-07-07

### Fixed

- Marketplace plugin `source` changed from the bare string `"."` to the object form
  `{ "source": "github", "repo": "ainova-systems/fcxo-toolkit" }`. Claude Code's CLI
  validator accepts a string source, but Claude Cowork (the claude.ai server-side
  marketplace sync, `remoteMarketplaceOps`) rejects it with `External plugin sources
  must be objects with a 'source' field (github, url, or git-subdir)`, so adding the
  marketplace in Cowork failed with "Marketplace sync failed". The object source lets
  Cowork and the community-directory sync resolve the plugin, while CLI installs keep
  working.

## [0.1.0] - 2026-07-05

First public release: 24 skills. A shell-free toolkit (file tools only) that runs the
same in Claude Code and Claude Cowork, with nothing to install.

### Skills

**Foundation**

- `fcxo-help` – list the skills grouped by job, explain when to use each, and recommend
  the right skill for a stated goal; reads the live skill folders so it never goes stale.
- `fcxo-init` – first-time setup of the practice workspace; records role, positioning,
  offers, and billing details so every other skill reads context automatically.
- `fcxo-new-client` – add a client and/or engagement, following the workspace conventions.
- `fcxo-brainstorm` – relentless discovery interview with per-answer checkpointing to a
  capture file. `fcxo-init` hands off to it when the user can't articulate their
  positioning or offers yet.
- `fcxo-learn-skill` – the skill builder: from anything the user brings (a goal, content,
  a reference skill, a URL, or the just-completed task) it researches how others do it
  (parallel subagents or inline web), finalizes scope with a short interview, and writes a
  standards-compliant `SKILL.md` (on-demand or scheduled), staged for review.

**Find & win clients**

- `fcxo-prospect-research`, `fcxo-qualify`, `fcxo-call-summary`.
- `fcxo-outreach` – cold / warm / referral outreach plus a `touch` mode for periodic
  warm-network check-ins (no ask, no pitch); draft-only.
- `fcxo-call-prep` – one-page prep before a discovery, sales, or client call: objective,
  tight agenda, pain-funnel questions in the role's lens, qualification gaps to close,
  likely objections, the one next step to land.
- `fcxo-proposal` – the conversion artifact between qualify and new-client: a proposal /
  SOW / letter of agreement with a deal-specific out-of-scope list (the scope-creep
  guard), one price anchored to the user's offers, and one next step. Draft-only.

**Communicate**

- `fcxo-log-message` – record an incoming or sent client message into its
  `communications/` thread (append, never fork; one thread = one file; verbatim text +
  commentary + wikilinks). Builds the relationship timeline; never sends.
- `fcxo-reply` – draft one reply inside an existing thread from the thread file only,
  mirroring its tone and language; appended draft-only.

**Deliver & retain**

- `fcxo-onboarding-plan` – the 30-60-90 (Diagnose - Design - Deliver) kickoff plan,
  grounded in the engagement's recorded scope, with visible early wins in weeks 2-3 and
  an explicit access/data ask for the kickoff call.
- `fcxo-status-update` – the highest-frequency recurring deliverable: a periodic client
  update drafted from the workspace record (engagement goals, meetings, threads,
  deliverables, the previous update), plus a private scope-creep check that never enters
  the client draft. The natural scheduled task.
- `fcxo-decision-report` – board-ready decision memo in the user's role lens.
- `fcxo-research-form` – the "collect data" front of the delivery pattern: a short,
  typed stakeholder questionnaire as a platform-neutral form spec (never calls a forms
  API), plus honest synthesis of the responses (distributions, verbatim themes, splits
  and outliers flagged).
- `fcxo-report` – the "produce the deliverable" back of the pattern: compose a
  client-ready report (executive summary, evidence-backed findings with severity,
  recommendations with effort/impact, next steps) and render it on the document system
  as one self-contained HTML to print to PDF (new `report.html` template with cover,
  section, finding, and severity-chip components).
- `fcxo-renewal-review` – the renewal / QBR checkpoint: evidence-backed results vs plan
  for the client plus a clearly separated internal read (renew / expand / reshape /
  wind down, one recommendation) and talking points; run 4-6 weeks before term end.

**Market**

- `fcxo-post` – draft or finalize a short professional post in the user's voice,
  enforced by a per-workspace voice guide (`me/<Owner> - Voice.md`) that learns from
  feedback; channel-aware (LinkedIn default).
- `fcxo-case-study` – turn a finished engagement into an anonymized, reusable win story
  (full version for proposals + a short seed for posts), with anonymization as a hard
  rule and every outcome traced to the workspace record.

**Run the practice**

- `fcxo-invoice`, `fcxo-pipeline-review`.

**Documents & brand**

- `fcxo-brand` and a shared document design system. The whole design system is a single
  human-readable `DESIGN.md` per workspace (`me/brand/DESIGN.md`): brand tokens (colors,
  typography, page, logo, billing) in its frontmatter and the component recipes in its
  prose body, all in one file. On render the tokens become a `:root` block and the
  recipes become the component CSS (referencing only those variables), filling a fixed
  template into one self-contained HTML file the user opens and prints to PDF. Set the
  look once with `fcxo-brand`; every document matches. `fcxo-invoice` and `fcxo-report`
  build their branded documents from this system.

### Workspace model

- Lightweight markdown **practice workspace** as the shared context anchor: every skill
  reads context from it and writes its output back into the right place, so the user
  never re-pastes their context.
- **Obsidian-compatible naming**: every filename is self-identifying and unique across
  the workspace (no two notes share a basename). Skills discover files by frontmatter
  `type:` and a filename suffix, never by a fixed generic name; there is no generic
  `README.md`, `Profile.md`, or `Engagement.md`.
- **Connected wiki graph**: skills write Obsidian `[[wikilinks]]` for the real
  relationships (engagement to client both ways, lead to its referrer and the relevant
  offer, proposal to its lead and offers, call prep / summary / deliverable to its
  client and engagement, status update and renewal review to the engagement they
  report on, the dated pipeline review to every client/engagement/lead it names,
  outreach to the lead or client it addresses, invoice to its engagement and client),
  so the workspace forms a connected graph.
- **The record compounds**: logged messages and call summaries feed status updates;
  status updates and deliverables feed the renewal review; the renewal review feeds
  case studies; case studies feed proposals and posts.

[0.2.3]: https://github.com/ainova-systems/fcxo-toolkit/releases/tag/v0.2.3
[0.2.2]: https://github.com/ainova-systems/fcxo-toolkit/releases/tag/v0.2.2
[0.2.1]: https://github.com/ainova-systems/fcxo-toolkit/releases/tag/v0.2.1
[0.2.0]: https://github.com/ainova-systems/fcxo-toolkit/releases/tag/v0.2.0
[0.1.1]: https://github.com/ainova-systems/fcxo-toolkit/releases/tag/v0.1.1
[0.1.0]: https://github.com/ainova-systems/fcxo-toolkit/releases/tag/v0.1.0
