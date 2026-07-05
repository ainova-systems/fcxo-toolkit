# Changelog

All notable changes to the FCxO Toolkit plugin.

The format is based on [Keep a Changelog](https://keepachangelog.com/), and this
project adheres to [Semantic Versioning](https://semver.org/).

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
  so the workspace forms a connected graph instead of isolated notes.
- **The record compounds**: logged messages and call summaries feed status updates;
  status updates and deliverables feed the renewal review; the renewal review feeds
  case studies; case studies feed proposals and posts.

[0.1.0]: https://github.com/ainova-systems/fcxo-toolkit/releases/tag/v0.1.0
