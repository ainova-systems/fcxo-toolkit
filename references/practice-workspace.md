# Practice workspace – shared conventions

Every skill in this plugin is **workspace-aware**. This file is the single source of
truth for the workspace layout and the read/write conventions. Skills reference it
instead of restating the structure.

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
- Client files are prefixed with the client name: `<Client> - Profile.md`, `<Client> - <Descriptor> Engagement.md`.
- Leads end with the role: `<Lead> - Lead.md`.
- Dated outputs lead with the date and name their subject: `<YYYY-MM-DD> - <Client> Call Summary.md`, `<YYYY-MM-DD> - <Topic> Decision Report.md`, `<YYYY-MM-DD> - Pipeline Review.md`.
- Never create `README.md` (it collides the moment a second folder has one). Use a single self-identifying `Practice Workspace - Guide.md` at the root, and let folder names speak for themselves.

Skills **discover files by frontmatter `type:` and by filename suffix** (for example
`me/*- Profile.md`, `clients/*/engagements/**/*- Engagement.md`, `leads/*- Lead.md`),
never by a fixed generic name.

## Relationships (wikilinks)

The workspace is a **wiki**: files reference related files with Obsidian wikilinks
(`[[Note Name]]`, by basename, which is the other reason every name is unique), so the
context forms a connected graph of relationships, not isolated notes. Whenever a skill
creates or updates a file, it writes the links that express the real relationships:

- an engagement links to its client profile, and the profile links back to its engagement(s);
- a lead links to who referred it (a client profile or person) and to the relevant offer;
- a call summary / meeting note / deliverable links to its client and engagement;
- a pipeline / portfolio review links to every client, engagement, and lead it names;
- outreach links to the lead or client it addresses.

Link by basename only (`[[Northwind Labs - Profile]]`), never a path; an alias is fine
(`[[Northwind Labs - Profile|Northwind]]`). The result is a graph a reader and Obsidian's
graph view can follow.

## Layout

```
practice/
├── Practice Workspace - Guide.md   # how to use this workspace (written by fcxo-init)
├── me/
│   ├── <Owner> - Profile.md     # name, role, contacts, billing details, currency
│   ├── <Owner> - Positioning.md # how the user describes what they do (voice source)
│   ├── <Owner> - Offers.md      # services, tiers, rates
│   ├── <Owner> - Voice.md       # post voice + style guide, learns from feedback (fcxo-post)
│   └── brand/                   # document design system (written by fcxo-init / fcxo-brand)
│       ├── DESIGN.md            # the document design system (tokens + recipes), edited by you
│       └── <logo file>          # optional logo, inlined into documents at render time
├── clients/<slug>/
│   ├── <Client> - Profile.md
│   ├── engagements/<id>/
│   │   ├── <Client> - <Descriptor> Engagement.md   # scope / price / status / rate
│   │   ├── deliverables/   # reports, assessments, finished outputs
│   │   ├── meetings/       # call notes + summaries
│   │   └── notes/
│   ├── communications/     # outward message drafts, relationship timeline
│   └── documents/          # contract / SOW
├── leads/
│   └── <Lead> - Lead.md    # pipeline, one file per lead
├── content/                # LinkedIn / brand drafts
├── finance/                # invoices, tracking
├── research/               # reusable research, dated skill outputs
└── templates/              # note templates
```

`<slug>` and `<id>` are short folder keys (e.g. `northwind-labs`, `retainer-2026`);
they only group files. The **filename** still stands alone, so an engagement file is
`Northwind Labs - Retainer Engagement.md`, not `Engagement.md`.

## Reading context (do this before asking the user)

- **Role** → the owner profile `me/*- Profile.md` (`type: profile`), frontmatter `role:`. Drives every skill's lens. If absent, ask once and offer to record it.
- **Voice / positioning** → `me/*- Positioning.md` (`type: positioning`). The source for how outreach, posts, and proposals describe the user. Never invent positioning (the writing rule lives in `copy-principles.md`); read this first.
- **Rates / offers** → `me/*- Offers.md` (`type: offers`). Used by qualify, proposal, invoice.
- **Voice / post style** → `me/*- Voice.md` (`type: voice`). How the user's posts sound, plus the do's and don'ts; `fcxo-post` reads it and appends durable feedback to it. Built from positioning, past `content/`, or a brainstorm when absent.
- **Billing details** → `me/brand/DESIGN.md` `from` block (legal name, address, tax id, payment info, currency), falling back to the owner profile. Used by invoice.
- **Brand / document look** → `me/brand/DESIGN.md` (the document design system: tokens in frontmatter - accent, fonts, logo, invoice series, VAT - plus component recipes in the body, all in this one file). Written by `fcxo-init` (default) and `fcxo-brand`; read by every document skill. If absent, documents use the plugin default.
- **Client / engagement** → `clients/<slug>/…`. Scope, price, status, rate live in the engagement file `clients/*/engagements/**/*- Engagement.md` (`type: engagement`).
- **Leads** → `leads/*- Lead.md` (`type: lead`).

## Writing output (where each concern goes)

| Output | Destination |
|--------|-------------|
| Lead research, qualification | `leads/<Lead> - Lead.md` |
| Outreach draft | `clients/<slug>/communications/` (existing client) or `leads/<Lead> - Lead.md` |
| Proposal / SOW | `leads/<YYYY-MM-DD> - <Lead> Proposal.md` (lead-stage) or `clients/<slug>/documents/<YYYY-MM-DD> - <Client> Proposal.md` (existing client) |
| Logged message or thread (incoming / sent), reply draft | `clients/<slug>/communications/<YYYY-MM-DD> - <Subject>.md` (append to the existing thread, never fork) |
| Client status update draft | `clients/<slug>/communications/<YYYY-MM-DD> - <Client> Status Update.md` |
| Call prep | append to `leads/<Lead> - Lead.md` (lead call) or `clients/<slug>/engagements/<id>/meetings/<YYYY-MM-DD> - <Client> Call Prep.md` (client call) |
| Call summary, meeting notes | `clients/<slug>/engagements/<id>/meetings/<YYYY-MM-DD> - <Client> Call Summary.md` |
| Decision report, onboarding plan, renewal review, report, deliverable | `clients/<slug>/engagements/<id>/deliverables/<YYYY-MM-DD> - <Topic>.md` (a rendered report keeps its `.html` sibling there) |
| Questionnaire spec, survey findings | `clients/<slug>/engagements/<id>/notes/<YYYY-MM-DD> - <Client> <Topic>.md` (practice-wide → `research/`) |
| Case study | `content/<YYYY-MM-DD> - <Descriptor> Case Study.md` (anonymized descriptor, never the client name) |
| LinkedIn / brand drafts | `content/` |
| Invoice | `finance/` |
| Reusable research | `research/` |
| Pipeline / portfolio review (dated copy, optional) | `research/<YYYY-MM-DD> - Pipeline Review.md` |

When no workspace is present, skip the write and return the result in chat.

## Frontmatter (light, optional, never enforced)

Use a few keys so a note is readable at a glance and other skills can find it. The
`type:` key is how skills discover a file when the filename suffix is ambiguous. Do
not validate or block on them.

```yaml
---
type: profile | positioning | offers | voice | lead | client | engagement | deliverable | meeting | communication | invoice | post | proposal | survey | case-study | brainstorm
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

- **Self-identifying filenames.** Every file carries its full context in its own name and is unique across the workspace. Never write a generic `README.md`, `Profile.md`, or `Engagement.md`; prefix with the owner, client, or lead name (see the naming rule above). Discover files by `type:` and suffix, not by fixed names.
- **Wiki relationships.** Write `[[wikilinks]]` between related files (engagement to client, lead to referrer and offer, summary/deliverable to client and engagement, review to every item it names) so the workspace is a connected graph, not islands. Link by basename, never a path.
- **Outward communications are draft-only.** Never send email or messages. Deliver the draft into the workspace (or chat) for the user to send.
- **No invented positioning.** The writing rule lives in `copy-principles.md`. Read `me/*- Positioning.md`; if it is empty, ask rather than fabricate.
- **Workspace optional.** Every skill must degrade gracefully to standalone: ask for the few facts it needs and return the result in chat. The setup skills are the exception, since they operate on the workspace itself: `fcxo-init` and `fcxo-new-client` ask the user to run `/fcxo-init` first when no workspace exists.
- **Shell-free.** Skills use the file tools (`Read`, `Write`, `Edit`, `Glob`) and never `Bash`. Folders are created by writing a file into them; documents embed logos as inline `<svg>` or a `data:` URL, never via a shell. This keeps every skill runnable in a Claude Cowork session, not only in Claude Code. (`WebFetch` / `WebSearch` are fine where a skill genuinely researches the web.)
