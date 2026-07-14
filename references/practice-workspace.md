# Practice workspace – shared conventions

Every skill in this plugin is **workspace-aware**. This file is the single source of
truth for the workspace layout and the read/write conventions. Skills reference it, so
none of them restates the structure.

## Locating the workspace

A practice workspace is a folder named `practice/` (or whatever the user named it)
containing a `me/` folder with an owner profile (a `*- Profile.md` file carrying
frontmatter `type: profile`). To locate it:

1. If the user names a path, use it.
2. Otherwise look for `me/*- Profile.md` in or under the current working folder.
3. If none exists, run **standalone**: ask the user for the few facts you need
   (at minimum their role), produce the result in chat, and offer to save it once a
   workspace exists. Never block on a missing workspace.

## Naming rule (read this first)

**Every filename is self-identifying and unique across the whole workspace.** A file
must carry its full context in its own name, so it stays unique when the workspace is
opened as an Obsidian vault (Obsidian and most note tools forbid two files sharing a
basename). Never use a generic stub that repeats across folders.

- Owner files are prefixed with the owner name: `<Owner> - Profile.md`, `<Owner> - Positioning.md`, `<Owner> - Offers.md`.
- Company files are prefixed with the company name: `<Client> - Profile.md`, `<Client> - <Descriptor> Engagement.md`, `<Lead> - Lead.md`.
- Dated outputs lead with the date and name their subject: `<YYYY-MM-DD> - <Company> Call Summary.md`, `<YYYY-MM-DD> - <Company> Proposal.md`, `<YYYY-MM-DD> - <Topic> Decision Report.md`.
- Never create `README.md` (it collides the moment a second folder has one). Use a single self-identifying `Practice Workspace - Guide.md` at the root, and let folder names speak for themselves.
- `templates/` and `design/` are the two deliberate exceptions: their files are named for the **document type** they define (`Proposal.md`, `Proposal.html`), because that pairing is what makes them findable. They are blanks, never records.

Skills **discover files by frontmatter `type:` and by filename suffix** (for example
`me/*- Profile.md`, `clients/*/engagements/**/*- Engagement.md`, `leads/**/*- Lead.md`),
never by a fixed generic name.

## One company, one folder

The central rule of the layout. **Every company the practice deals with gets its own
folder**, and that folder always has the same shape, whether the deal is won or not:

- Before there is a contract, the folder lives in `leads/<slug>/`.
- Once the deal is won, the **same folder moves** to `clients/<slug>/` and gains
  `engagements/`. Nothing is renamed and no link breaks, because every file inside was
  already named for the company.

So a lead's inbound email, its discovery call, and its proposal live beside the lead
record in typed sub-folders. A proposal is **never** a loose file sitting next to lead
records: it is a document, and documents live in `proposals/`.

## Layout

```
practice/
├── Practice Workspace - Guide.md          # how to use this workspace (written by fcxo-init)
├── Practice Workspace - Integrations.md   # which connected tool serves which capability
├── me/                          # who the owner is (identity, never documents)
│   ├── <Owner> - Profile.md     # name, role, contacts, billing details, currency
│   ├── <Owner> - Positioning.md # how the user describes what they do (voice source)
│   ├── <Owner> - Offers.md      # services, tiers, rates
│   └── <Owner> - Voice.md       # post voice + style guide, learns from feedback (fcxo-post)
├── leads/<slug>/                # a company before the contract
│   ├── <Lead> - Lead.md         # the record: source, fit, qualification, next step
│   ├── communications/          # inbound messages and reply drafts, one file per message
│   ├── meetings/                # call prep, call summaries
│   └── proposals/               # proposals and SOWs (+ the rendered .html sibling)
├── clients/<slug>/              # the same company after the contract
│   ├── <Client> - Profile.md
│   ├── engagements/<id>/
│   │   ├── <Client> - <Descriptor> Engagement.md   # scope / price / status / rate
│   │   ├── deliverables/   # reports, assessments, finished outputs
│   │   ├── meetings/       # call prep, call notes + summaries
│   │   └── notes/
│   ├── communications/     # relationship timeline, message threads, reply drafts
│   ├── proposals/          # expansion / renewal proposals (+ rendered .html sibling)
│   └── documents/          # signed contract / SOW
├── content/                # LinkedIn / brand drafts, case studies
├── finance/                # invoices, tracking
├── research/               # reusable research and DATED skill outputs (scans, reviews)
├── templates/              # DATA templates: what fields a document type has
├── design/                 # DESIGN templates: how a document type looks
│   ├── DESIGN.md           # brand tokens + component recipes (the one file the user edits)
│   ├── <logo file>         # optional logo, inlined into documents at render time
│   └── <Doc>.html          # one branded HTML layout per document type
└── skills/<name>/SKILL.md  # the user's own skills, staged by fcxo-learn-skill
```

`<slug>` and `<id>` are short folder keys (e.g. `harbor-analytics`, `retainer-2026`);
they only group files. The **filename** still stands alone, so an engagement file is
`Orlend Labs - Retainer Engagement.md`, not `Engagement.md`.

## Templates and design pair by name

`templates/` and `design/` are two halves of one document type, matched by basename:

| Document type | Data template (fields) | Design template (layout) |
|---|---|---|
| Proposal | `templates/Proposal.md` | `design/Proposal.html` |
| Report | `templates/Report.md` | `design/Report.html` |
| Invoice | `templates/Invoice.md` | `design/Invoice.html` |

A document skill writes markdown against the **data** template, and a render fills the
**design** template of the same name. A design template with no data twin renders
nothing; a data template with no design twin still saves as plain markdown and simply
has no branded output. `/fcxo-setup-template` creates or replaces a pair from a design
sample, so the user can design a document in Claude, hand the result to the toolkit,
and have every future document of that type come out in that layout.

Records (`Lead.md`, `Client - Profile.md`, `Engagement.md`) are **data templates only**:
they are internal notes, so they have no design twin.

## Integrations: the workspace names the tool, the toolkit ships none

This plugin ships **no connectors and no credentials**. It reaches your email, calendar
or drive through the connectors your agent surface already gives you - the ones built
into Claude Cowork, or installed from its marketplace. You connect a tool once, in the
surface, and it belongs to you.

What the workspace holds is the **registry**: `Practice Workspace - Integrations.md`, a
plain table saying which connected tool serves which capability, and what a skill is
allowed to do with it.

| Capability | Connected tool | Skills that use it | Access |
|---|---|---|---|
| Calendar | (whatever the user connected) | `/fcxo-book-call` | Read availability. Create a guest-free hold. |
| Email | (whatever the user connected) | `/fcxo-log-message` | Read only. |
| Web research | built-in web search | `/fcxo-prospect-research`, lead scans | Read only. |

A skill that needs a connected tool **reads this file to find out which one**, rather
than naming a vendor in its own instructions. So a user on a different calendar changes
one row in one markdown file and every skill follows. When the file has no row for a
capability, the skill degrades gracefully: it asks the user for the few facts it needs
and carries on.

**The write boundary lives here, and it is narrow.** A calendar hold on the user's own
calendar is the only write any skill makes through a connector. Every other connected
tool is read-only: a skill reads from it and drafts into the workspace. Nothing sends,
invites, notifies, or shares.

## Reading context (do this before asking the user)

- **Role** → the owner profile `me/*- Profile.md` (`type: profile`), frontmatter `role:`. Drives every skill's lens. If absent, ask once and offer to record it.
- **Voice / positioning** → `me/*- Positioning.md` (`type: positioning`). The source for how outreach, posts, and proposals describe the user. Never invent positioning (the writing rule lives in `copy-principles.md`); read this first.
- **Rates / offers** → `me/*- Offers.md` (`type: offers`). Used by qualify, proposal, invoice.
- **Voice / post style** → `me/*- Voice.md` (`type: voice`). How the user's posts sound, plus the do's and don'ts; `fcxo-post` reads it and appends durable feedback to it. Built from positioning, past `content/`, or a brainstorm when absent.
- **Leads** → `leads/**/*- Lead.md` (`type: lead`). One per lead folder. A lead's history is its siblings: `communications/`, `meetings/`, `proposals/`.
- **Client / engagement** → `clients/<slug>/…`. Scope, price, status, rate live in the engagement file `clients/*/engagements/**/*- Engagement.md` (`type: engagement`).
- **Billing details** → `design/DESIGN.md` `from` block (legal name, address, tax id, payment info, currency), falling back to the owner profile. Used by invoice.
- **Brand / document look** → `design/DESIGN.md` (the document design system: tokens in frontmatter - accent, fonts, logo, invoice series, VAT - plus component recipes in the body, all in this one file) plus the `design/<Doc>.html` layout for the document being produced. Written by `fcxo-init` (default), `fcxo-brand` (tokens) and `fcxo-setup-template` (layouts); read by every document skill. If absent, documents use the plugin default.

## Writing output (where each concern goes)

`<company>` below resolves to `leads/<slug>/` before the contract and `clients/<slug>/`
after it. The sub-folder is the same either way, which is the whole point of the
one-company-one-folder rule.

| Output | Destination |
|--------|-------------|
| Lead research, qualification | `leads/<slug>/<Lead> - Lead.md` |
| Logged message (incoming / sent), reply draft, outreach draft | `<company>/communications/<YYYY-MM-DD> - <From\|To> <Name> - <Subject>.md` (append to the existing thread, never fork) |
| Client status update draft | `clients/<slug>/communications/<YYYY-MM-DD> - To <Name> - <Client> Status Update.md` |
| Call prep | `<company>/meetings/<YYYY-MM-DD> - <Company> Call Prep.md` |
| Call summary, meeting notes | `<company>/meetings/<YYYY-MM-DD> - <Company> Call Summary.md` |
| Booked call (calendar hold + the reply that proposes it) | the hold goes in the owner's calendar; the drafted reply goes to `<company>/communications/` |
| Proposal / SOW | `<company>/proposals/<YYYY-MM-DD> - <Company> Proposal.md` (a rendered document keeps its `.html` sibling there) |
| Signed contract / SOW | `clients/<slug>/documents/` |
| Decision report, onboarding plan, renewal review, report, deliverable | `clients/<slug>/engagements/<id>/deliverables/<YYYY-MM-DD> - <Topic>.md` (a rendered report keeps its `.html` sibling there) |
| Questionnaire spec, survey findings | `clients/<slug>/engagements/<id>/notes/<YYYY-MM-DD> - <Client> <Topic>.md` (practice-wide → `research/`) |
| Case study | `content/<YYYY-MM-DD> - <Descriptor> Case Study.md` (anonymized descriptor, never the client name) |
| LinkedIn / brand drafts | `content/` |
| Invoice | `finance/` |
| Reusable research | `research/` |
| Dated skill output (pipeline review, lead scan) | `research/<YYYY-MM-DD> - <Topic>.md` |
| A skill the user built | `skills/<name>/SKILL.md` |
| A document layout the user designed | `design/<Doc>.html`, paired with `templates/<Doc>.md` |

When no workspace is present, skip the write and return the result in chat.

## Relationships (wikilinks)

The workspace is a **wiki**: files reference related files with Obsidian wikilinks
(`[[Note Name]]`, by basename, which is the other reason every name is unique), so the
context forms a connected graph of relationships. Whenever a skill
creates or updates a file, it writes the links that express the real relationships:

- an engagement links to its client profile, and the profile links back to its engagement(s);
- a lead links to who referred it (a client profile or person) and to the relevant offer;
- a message, call summary, proposal, or deliverable links to its company (and engagement);
- a proposal links its lead or client **and** the owner's offers, and the lead or client links back to the proposal;
- a pipeline / portfolio review or a lead scan links to every company it names.

Link by basename only (`[[Orlend Labs - Profile]]`), never a path; an alias is fine
(`[[Orlend Labs - Profile|Orlend]]`). The result is a graph a reader and Obsidian's
graph view can follow.

## Frontmatter (light, optional, never enforced)

Use a few keys so a note is readable at a glance and other skills can find it. The
`type:` key is how skills discover a file when the filename suffix is ambiguous. Do
not validate or block on them.

```yaml
---
type: profile | positioning | offers | voice | lead | client | engagement | deliverable | meeting | communication | proposal | invoice | post | survey | case-study | brainstorm | template
role: cto            # on the owner profile
status: draft | new | active | won | lost | sent | paid
updated: 2026-06-08
---
```

## Roles

Accepted role values: `ceo`, `cmo`, `cso`, `cto`, `coo`, `cfo` (others allowed). Each
skill adapts its framing to the role – the lens, the questions that matter, the
vocabulary – without faking domain depth the user does not have. When the role is set
in the owner profile, use it silently; only ask if it is missing and the skill's output
genuinely depends on it.

## Hard rules

- **One company, one folder.** A company's record, messages, meetings, and proposals live together under `leads/<slug>/` or `clients/<slug>/`. Never scatter a company's work across top-level folders, and never leave a document loose beside a record.
- **Self-identifying filenames.** Every file carries its full context in its own name and is unique across the workspace. Never write a generic `README.md`, `Profile.md`, or `Engagement.md`; prefix with the owner or company name. Discover files by `type:` and suffix. `templates/` and `design/` are the stated exception, because they hold document-type blanks.
- **Wiki relationships.** Write `[[wikilinks]]` between related files (engagement to client, lead to referrer and offer, proposal to company and offers, summary/deliverable to company, review to every item it names) so the workspace is a connected graph. Link by basename, never a path.
- **Outward communications are draft-only.** Never send email or messages. Deliver the draft into the workspace (or chat) for the user to send. This holds for connected tools. A calendar hold on the user's **own** calendar is a private note to self and is allowed. Adding a guest to an event makes the calendar send that person an invitation, which is an outward message, so never add a guest and never create or modify any event that already has attendees. If the event you would touch has guests, stop, say why, and let the user change it themselves.
- **The connector write boundary.** A guest-free calendar hold is the only write a skill makes through a connected tool. Email, chat, CRM, drive and every other connector are read-only: read from them, draft into the workspace. No skill sends, invites, notifies, or shares. A skill the user builds inherits this rule.
- **No invented positioning.** The writing rule lives in `copy-principles.md`. Read `me/*- Positioning.md`; if it is empty, ask the user. Never fabricate positioning.
- **Workspace optional.** Every skill must degrade gracefully to standalone: ask for the few facts it needs and return the result in chat. The setup skills are the exception, since they operate on the workspace itself: `fcxo-init` and `fcxo-new-client` ask the user to run `/fcxo-init` first when no workspace exists.
- **The `fcxo-` prefix is reserved** for the skills that ship with this plugin. A skill the user builds gets a plain descriptive name (`weekly-lead-opportunity-scan`), so it can never shadow a toolkit skill.
- **Shell-free.** Skills use the file tools (`Read`, `Write`, `Edit`, `Glob`) and never `Bash`. Folders are created by writing a file into them; documents embed logos as inline `<svg>` or a `data:` URL, never via a shell. This keeps every skill runnable in a Claude Cowork session as well as in Claude Code. (`WebFetch` / `WebSearch` are fine where a skill genuinely researches the web.)
- **The toolkit ships no connectors.** A skill that needs a connected tool reads `Practice Workspace - Integrations.md` to learn which one the user has, and uses it through the agent surface's own connector. Never name a vendor inside a skill, and never ask the user for a credential, an API key, or an OAuth flow.
