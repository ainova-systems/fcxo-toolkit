# FCxO Toolkit

An operational toolkit for **fractional executives** – fractional CEO, CMO, CSO, CTO, COO, CFO. Every fractional runs the same business under the hood: find clients, win them, deliver decision-level work, market yourself, and run your own back-office. This plugin turns those recurring jobs into skills you run with `/`.

It is **role-aware** (you set your role once, every skill adapts its lens), **works standalone** (web research plus what you tell it – no required integrations), and gets sharper when you connect your own tools (CRM, email, calendar, LinkedIn). Every skill is **shell-free**: it runs on file tools, web search, and the connectors your agent surface already gives you. There are no scripts and no shell commands, so it behaves the same in Claude Code and in Claude Cowork, with nothing to install.

## Install

```
# From the marketplace
/plugin marketplace add ainova-systems/fcxo-toolkit
/plugin install fcxo-toolkit@fcxo

# Or from a local clone (run from the repo folder, or pass its path)
/plugin marketplace add ./
/plugin install fcxo-toolkit@fcxo

# Or load it for a single session without installing
claude --plugin-dir .
```

Then run `/fcxo-init` once to set up your practice workspace. Skills register namespaced as `/fcxo-toolkit:fcxo-*`; the short `/fcxo-*` form works when there is no name clash.

## How it works

Everything is anchored on a small **practice workspace** – a plain folder of markdown notes you own, Obsidian-friendly, with no scripts or automation. You create it once with `/fcxo-init`. After that, every skill reads context from it (your role, positioning, rates, clients, pipeline) and writes its output back into the right place, so you never re-paste your context.

If you have no workspace, the skills still run – they just ask for the context they need and hand the result back in chat.

## The practice workspace

```
practice/
├── Practice Workspace - Guide.md          # how to use this workspace
├── Practice Workspace - Integrations.md   # which connected tool serves which capability
├── me/
│   ├── <Owner> - Profile.md      # name, role, contacts, billing details
│   ├── <Owner> - Positioning.md  # how you describe what you do
│   ├── <Owner> - Offers.md       # services, tiers, rates
│   └── <Owner> - Voice.md        # how your posts sound
├── leads/<slug>/                 # a company before the contract
│   ├── <Lead> - Lead.md          # the record: source, fit, qualification, next step
│   ├── communications/           # messages and reply drafts, one thread per file
│   ├── meetings/                 # call prep, call summaries
│   └── proposals/                # proposals and SOWs (+ the rendered .html sibling)
├── clients/<slug>/               # the same company after the contract
│   ├── <Client> - Profile.md
│   ├── engagements/<id>/
│   │   ├── <Client> - <Descriptor> Engagement.md   # scope / price / status
│   │   ├── deliverables/
│   │   ├── meetings/
│   │   └── notes/
│   ├── communications/
│   ├── proposals/                # expansion / renewal proposals
│   └── documents/                # signed contract / SOW
├── content/                      # LinkedIn / brand material
├── finance/                      # invoices, tracking
├── research/                     # reusable research, dated skill outputs
├── templates/                    # what fields a document type has: <Doc>.md
├── design/                       # how a document type looks: DESIGN.md + <Doc>.html
└── skills/<name>/SKILL.md        # the skills you build yourself
```

**One company, one folder.** Every company you deal with gets a folder, and it has the same shape whether the deal is won or not. Its messages, meetings, and proposals live beside its record. Before there is a contract the folder sits in `leads/<slug>/`; once you win the deal the same folder moves to `clients/<slug>/` and gains `engagements/`. Nothing is renamed and no link breaks, because every file inside was already named for the company.

Every filename is self-identifying and unique across the workspace, so the folder opens cleanly as an Obsidian vault (no two notes share a basename). Owner files are prefixed with your name, company files with the company name, leads end with `- Lead`, and dated outputs lead with the date. Skills find a file by its frontmatter `type:` and filename suffix, never by a fixed generic name. `templates/` and `design/` are the one exception: their files are named for the document type they define, so `templates/Proposal.md` and `design/Proposal.html` pair up by basename.

Light, optional frontmatter (e.g. `type:`, `role:`, `status:`) helps both you and the skills read a note at a glance. Nothing is enforced or validated – it is just structure.

## Connected tools

The toolkit ships **no connectors and asks for no credentials**. There is no API key to paste and no OAuth flow to sit through. It reaches your email, calendar, or drive through the connectors your agent surface already gives you – the ones built into Claude Cowork, or installed from its marketplace. You connect a tool once, in the surface, and it stays yours.

What your workspace holds is the registry: `Practice Workspace - Integrations.md`, a plain table saying which connected tool serves which capability and what a skill may do with it. A skill that needs a connected tool reads that file to learn which one you have. Switch calendars and you change one row in one markdown file, and every skill follows. When a capability has no row, the skill asks you for the few facts it needs and carries on.

**The write boundary is narrow.** A guest-free hold on your own calendar is the only thing any skill writes to a connected tool. Every other connected tool is read-only: a skill reads from it and drafts into your workspace. Nothing sends, invites, notifies, or shares.

## Skills

**Foundation**

| Skill | What it does |
|-------|--------------|
| `/fcxo-help` | Lists the skills grouped by job, explains when to use each, and points you to the right one for a goal. Start here. |
| `/fcxo-init` | First-time setup. Creates the practice workspace, records your role and billing details, optionally sets up Obsidian. |
| `/fcxo-new-client` | Adds a new client and/or engagement to your workspace. |
| `/fcxo-brainstorm` | Grills you on any plan, decision, or topic – one question at a time, every answer checkpointed to a capture file so nothing is lost. Onboarding hands off here to extract your positioning and offers. |
| `/fcxo-learn-skill` | Builds a new reusable skill from anything you bring – a goal, a sample output, an SOP, a reference skill, a URL, or the task you just did. It researches how others do it, finalizes scope with a short interview, and writes a standards-compliant skill (on-demand or scheduled), staged for your review. |

**Find & win clients**

| Skill | What it does |
|-------|--------------|
| `/fcxo-prospect-research` | Researches a target company and decision-maker into an actionable brief tuned to your role. |
| `/fcxo-outreach` | Drafts cold / warm / referral / network-touch outreach in your voice. Draft only – you send it. |
| `/fcxo-qualify` | Qualifies a lead: fit, urgency, budget signal, recommended engagement shape, go/no-go. |
| `/fcxo-call-prep` | Builds a one-page prep before a discovery, sales, or client call: objective, agenda, questions, the next step to land. |
| `/fcxo-call-summary` | Turns call notes or a transcript into action items, a follow-up email, and an internal note. |
| `/fcxo-proposal` | Drafts the proposal / SOW that converts a qualified lead: scope with clear edges, one price anchored to your offers, one next step. Draft only. |

**Communicate**

| Skill | What it does |
|-------|--------------|
| `/fcxo-log-message` | Logs an incoming or sent client message into its thread, building the relationship timeline. One thread, one file. |
| `/fcxo-reply` | Drafts one reply inside an existing thread, mirroring its tone and language. Draft only – you send it. |
| `/fcxo-book-call` | Turns "we should talk" into a held slot: reads your calendar, holds the time on your own calendar with no guests, and drafts the reply that proposes it. You send the reply, and you send the invitation once the time is agreed. |

**Deliver & retain**

| Skill | What it does |
|-------|--------------|
| `/fcxo-onboarding-plan` | Builds the 30-60-90 (Diagnose - Design - Deliver) plan a client expects at kickoff, grounded in the engagement's recorded scope. |
| `/fcxo-status-update` | Drafts the periodic client status update from the workspace record, plus a private scope-creep check. The retention artifact. |
| `/fcxo-decision-report` | Builds a board-ready decision memo: situation, options, recommendation, risks – in your role's lens. |
| `/fcxo-research-form` | Builds a short, typed stakeholder questionnaire as a platform-neutral form spec, then synthesizes the responses into findings. |
| `/fcxo-report` | Composes a client-ready report from engagement material and renders it branded: one self-contained HTML file you print to PDF. |
| `/fcxo-renewal-review` | Prepares the renewal / QBR checkpoint: evidence-backed results vs plan for the client, plus an honest internal read on whether to renew. |

**Market yourself**

| Skill | What it does |
|-------|--------------|
| `/fcxo-post` | Drafts or finalizes a short professional post in your voice, enforced by a per-workspace voice guide that learns from your feedback. Channel-aware (LinkedIn by default). |
| `/fcxo-case-study` | Turns a finished engagement into an anonymized, reusable win story that feeds proposals and posts. |

**Run the practice**

| Skill | What it does |
|-------|--------------|
| `/fcxo-invoice` | Generates a branded invoice from an engagement and your billing details, as an HTML file you open and print to PDF. |
| `/fcxo-pipeline-review` | Weekly review of pipeline and client portfolio: deals, risks, next actions. |

**Documents & brand**

| Skill | What it does |
|-------|--------------|
| `/fcxo-brand` | Sets up or adjusts your document look – accent color, logo, fonts, billing details – stored once and reused by every document. |
| `/fcxo-setup-template` | Installs a document layout you designed as a reusable type: it writes the `templates/<Doc>.md` and `design/<Doc>.html` pair, wired to your brand, so every future document of that type comes out in that layout. |

Client-facing documents (invoice and report today; proposals render through the same
system) share one small design system stored in your workspace. A finished document is
a single self-contained HTML file: you open it in a browser and print to PDF. No PDF
software, no fonts to install, nothing external. The look comes from a few brand tokens
– set them once with `/fcxo-brand` (give a color, drop a logo, or match an existing
document) and every document matches. Until you do, documents use a clean default.

### Planned (v2)

`fcxo-assessment` – a structured diagnostic composing `fcxo-research-form` and `fcxo-report`, coming once both building blocks have seen real use.

## Design principles

- **Draft, never send.** Any outward message (email, DM, outreach, proposal, status update) is delivered as a draft for you to review and send yourself. This holds for connected tools too: a skill reads from them and drafts into your workspace. A guest-free hold on your own calendar is the one write any skill makes, because adding a guest makes the calendar service email that person an invitation you did not send.
- **Your framing first.** Positioning and voice come from your own `me/*- Positioning.md`, in the words you already use to describe what you do.
- **Standalone-first.** No connector is required. Every skill runs on web research and what you tell it; connecting your own tools makes the result richer.
- **Lean.** A small set of skills that each earn their place, over a large set of half-baked ones.

## Data handling

Instruction-only skills, local files only, no telemetry, draft-only outward
communication. See [PRIVACY.md](PRIVACY.md) for the full disclosure.

## Versioning

See `CHANGELOG.md`. This repository is the source project; the installable plugin is built from it and follows [Semantic Versioning](https://semver.org/).

## License

MIT. See `LICENSE`.
