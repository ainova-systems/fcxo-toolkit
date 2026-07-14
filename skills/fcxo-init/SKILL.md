---
name: fcxo-init
description: "First-time setup of a fractional executive's practice workspace. Use when the user wants to set up their practice, initialize an FCxO workspace, or get started with the FCxO Toolkit."
argument-hint: "[target folder]"
allowed-tools: Read, Write, Glob
---

# fcxo-init – set up your practice workspace

Create the practice workspace once. Every other skill in this plugin reads context
from it. This is **first-time setup only** – to add clients later, use
`/fcxo-new-client`.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` first – it is the source
of truth for the layout and conventions.

## Step 1 – Check for an existing workspace

Look for a `me/*- Profile.md` (the owner profile, `type: profile`) in or under the
target folder (default: current folder).

- **If one exists**, this practice is already set up. Do not overwrite. Tell the user
  it exists, show where, and point them to `/fcxo-new-client` to add clients or
  `/fcxo-init <other-path>` to set up a separate practice. Stop.
- **If none exists**, proceed.

## Step 2 – Gather setup details (one batched ask)

Ask for everything at once, in plain language. Keep it short; everything is editable
later. Collect:

- **Name** – the user's name (and company/brand name if they trade under one). This is
  the `<Owner>` prefix used to name every `me/` file, so capture it before scaffolding.
- **Role** – fractional CEO / CMO / CSO / CTO / COO / CFO (or other). Store the short
  code in frontmatter (`cto`, `cmo`, …).
- **What they do** – one or two sentences on their service and who they serve. This
  seeds `me/<Owner> - Positioning.md`; do not polish or rewrite it into marketing copy.
- **Offers & rates** – their service tiers / packages and prices, even rough. Seeds
  `me/<Owner> - Offers.md`.
- **Billing details** – legal/billing name, address, tax/VAT id if any, payment info
  (IBAN or "see notes"), and **currency**. Seeds the billing block in
  `me/<Owner> - Profile.md`.
- **Target folder** – where to create the workspace (default: current folder, in a
  `practice/` subfolder).

If the user gives only partial answers, create the files anyway with clear `TODO:`
placeholders for the gaps – never block setup on a missing detail.

If the user struggles to articulate their positioning or offers ("I don't know how to
put it", vague half-answers), offer `/fcxo-brainstorm` scoped to their practice: it
interviews them question by question, checkpoints every answer, and seeds
`me/<Owner> - Positioning.md` and `me/<Owner> - Offers.md` from their captured words.
Finish the scaffold first (with `TODO:` placeholders), then run the brainstorm to fill
them.

## Step 3 – Obsidian question (explicit yes/no)

Ask plainly: **"Will you open this workspace in Obsidian? (yes/no)"**

- **Yes** → also create a minimal `.obsidian/` (an empty `app.json` and
  `workspace.json` with `{}`), so the folder opens as a vault immediately.
- **No** → plain markdown only, no `.obsidian/`.

## Step 4 – Scaffold

Create the structure from the shared reference using the file tools only (no shell):
writing a file into a folder creates the folder, so each directory comes into being as
you write its starter file. Use the owner name gathered in Step 2 as the `<Owner>`
prefix on every `me/` file so each filename is self-identifying and unique. Write these
starter files:

### `practice/Practice Workspace - Guide.md`
The single orientation file at the workspace root (never a `README.md` – generic
basenames collide when the folder opens in Obsidian). Cover: what this folder is, the
layout, that the FCxO skills read and write here, and these four conventions:

- **One company, one folder.** Every company gets its own folder holding its record and
  its typed sub-folders `communications/`, `meetings/`, `proposals/`. Before a contract
  the folder is `leads/<slug>/`; once the deal is won the same folder moves to
  `clients/<slug>/` and gains `engagements/` and `documents/`. Messages, call prep and
  summaries, and proposals live inside the company folder, so a proposal is a document in
  `proposals/` and never a loose file beside a lead record.
- **Self-identifying naming.** Every file carries its full context in its own name (owner
  files prefixed with the owner name, client files with the client name, leads ending in
  `- Lead`, dated outputs leading with the date), so no two notes share a basename and
  skills discover a file by its `type:` and filename suffix.
- **The naming exception.** `templates/` and `design/` are the two folders whose files are
  named for the **document type** they define (`Proposal.md`, `Proposal.html`). They hold
  blanks, never records, and the type name is what makes them findable. Every template is
  copied out and renamed to a self-identifying name on use.
- **Templates and design pair by basename.** `templates/<Doc>.md` says what fields a
  document type has; `design/<Doc>.html` says how it looks. A document skill writes the
  markdown against the data template, and a render fills the design template of the same
  name. `/fcxo-setup-template` replaces a layout with one the user designed in Claude.

Per the wiki convention in `practice-workspace.md`, the guide links the main entry points
so the workspace opens as a connected graph: `[[<Owner> - Profile]]`,
`[[<Owner> - Positioning]]`, `[[<Owner> - Offers]]`, and
`[[Practice Workspace - Integrations]]`.

End the guide with two lines: run `/fcxo-help` to see every skill, where to start, and the
order the skills run in; any skill also runs on its own, and one that cannot find the file
it would have read asks for what it needs. Do not copy the sequences into the guide -
`/fcxo-help` reads them from the plugin, so they stay current as the toolkit changes.

### `practice/Practice Workspace - Integrations.md`
The second root file, beside the guide: the registry that says which connected tool serves
which capability. This toolkit ships no connectors and no credentials. The user connects
email, calendar or a drive once in their own agent surface (the connectors built into
Claude Cowork, or ones installed from its marketplace), and records them here. A skill that
needs a connected tool reads this file to learn which one the user has, so changing tools
means editing one row and every skill follows.

Seed the table with the rows **empty** (`not connected yet`) and say in Step 5 that the
user fills in whichever tools they connect. Never ask for a credential, an API key, or an
OAuth flow during init, and never name a vendor.

```markdown
---
type: integrations
updated: <YYYY-MM-DD>
---

# Practice Workspace - Integrations

This toolkit ships no connectors. Connect the tools you want in your own agent surface -
the connectors built into Claude Cowork, or ones from its marketplace - then name them in
the table below. The FCxO skills read this table to find out which tool you use.

| Capability | Connected tool | Skills that use it | Access |
|---|---|---|---|
| Calendar | not connected yet | `/fcxo-book-call` | Read availability. Create a guest-free hold. |
| Email | not connected yet | `/fcxo-log-message` | Read only. |
| Web research | built-in web search | `/fcxo-prospect-research`, lead scans | Read only. |

A capability with no tool named still works: the skill asks you for the few facts it needs
and carries on.

## The write boundary

A guest-free hold on your own calendar is the only write a skill makes through a connected
tool. Every other connected tool is read-only: a skill reads from it and drafts into this
workspace for you to send. No skill sends, invites, notifies, or shares. A skill never
creates or modifies an event that already has attendees, because the calendar service
emails every attendee an update, and that is an outward message you did not send.
```

### `practice/me/<Owner> - Profile.md`
```markdown
---
type: profile
role: <code>
updated: <YYYY-MM-DD>
---

# <Name> – <Role>

<Company/brand, if any>

## Contact
- Email: <or TODO>
- LinkedIn: <or TODO>

## Billing
- Billing name: <legal name or TODO>
- Address: <or TODO>
- Tax / VAT id: <or TODO>
- Payment: <IBAN / details or TODO>
- Currency: <e.g. EUR>
```

Per the wiki convention in `practice-workspace.md`, link the sibling `me/` files from the
profile body (`[[<Owner> - Positioning]]`, `[[<Owner> - Offers]]`) so the owner files form
a connected set.

### `practice/me/<Owner> - Positioning.md`
Frontmatter `type: positioning`, then the user's own words from Step 2, verbatim, under
a short heading. Add a one-line note that this file is the source of truth for how
outreach and posts describe them, and they should refine it in their own voice. Do
**not** rewrite it into polished copy. Per the wiki convention in
`practice-workspace.md`, cross-link the sibling `me/` files (`[[<Owner> - Profile]]`,
`[[<Owner> - Offers]]`).

```markdown
---
type: positioning
updated: <YYYY-MM-DD>
---
```

### `practice/me/<Owner> - Offers.md`
Frontmatter `type: offers`, then a simple table of their tiers/packages and prices (or
`TODO` rows if not given). Per the wiki convention in `practice-workspace.md`,
cross-link the sibling `me/` files (`[[<Owner> - Profile]]`, `[[<Owner> - Positioning]]`):

```markdown
---
type: offers
updated: <YYYY-MM-DD>
---

| Offer | What's included | Price | Typical buyer |
|-------|-----------------|-------|---------------|
```

### Concern folders (no README placeholders)
`clients/`, `leads/`, `content/`, `finance/`, `research/`, `sop/`, `templates/`, `design/`,
`skills/`. Do **not** drop a per-folder `README.md` – generic basenames collide when the
workspace opens in Obsidian, and the folder names are self-explanatory. The
`Practice Workspace - Guide.md` already describes what each folder holds. A folder comes
into being when its first real file is written (e.g. `templates/` and `design/` exist once
you write the templates below; the others appear when `/fcxo-new-client` or a skill writes
the first file into them). If the user keeps the workspace under git and wants the empty
folders tracked now, add a `.gitkeep` (never a `README.md`); a plain Obsidian folder does
not need one.

`clients/` and `leads/` follow the **one company, one folder** rule from the shared
reference: a company is a folder, `leads/<slug>/` before the contract and
`clients/<slug>/` after it, and both hold the same typed sub-folders `communications/`,
`meetings/`, and `proposals/` (a client adds `engagements/` and `documents/`). Do not
scaffold example company folders at init; `/fcxo-new-client` and the lead skills create
them on first use, with the sub-folders coming into being as the first message, meeting,
or proposal is written.

`sop/` holds the procedures the user follows, written down, one file each
(`sop/<Procedure> - SOP.md`), with each procedure's configuration beside it. `me/` is
identity, so a procedure lives here and not there. `/fcxo-write-sop` produces the first one,
from a procedure the user already runs or from a bare goal it researches and grounds in the
practice; say in the guide that a procedure in `sop/` can be **scheduled** (a scheduled task follows
it, with nothing installed) or **built into a skill** (`/fcxo-learn-skill` writes
`skills/<name>/SKILL.md`, which the user uploads to their Claude account and calls with
`/`). A schedule runs it without them. A skill lets them run it whenever they want.

### `practice/templates/` (the data templates)
Drop ready-to-copy templates: `Client - Profile.md`, `Engagement.md`, `Lead.md`,
`Proposal.md`, `Invoice.md`. Each is a minimal markdown skeleton with the light
frontmatter (`type:` etc.) from the shared reference. Keep them short. These keep the
document-type name because `templates/` is the stated naming exception: they are blanks,
**copied out and renamed to a self-identifying name on use** (a `<Client> - Profile.md`, a
`<Client> - <Descriptor> Engagement.md`, a `<Lead> - Lead.md`, a
`<YYYY-MM-DD> - <Company> Proposal.md`); state that in the guide so the convention is
clear.

`Proposal.md` carries `type: proposal` in its frontmatter and the section structure
`/fcxo-proposal` writes, so a proposal is a document type from day one and never drifts
into a loose file beside a lead record:

```markdown
---
type: proposal
status: draft
updated: <YYYY-MM-DD>
---

# <Company> Proposal

## Their situation
## Proposed engagement
### Explicitly out of scope
## Timeline and start
## Investment
## Terms
## Next step
```

### `practice/design/` (the design system and the design templates)
Read `${CLAUDE_PLUGIN_ROOT}/references/document-system/DESIGN.default.md` and write it to
`practice/design/DESIGN.md` (file tools, no shell), filling in `brandName` and the `from`
block (and `footerLegal` if known) from the details gathered in Step 2, leaving the rest at
the clean default (accent `#1F6B85`). DESIGN.md is the whole document design system (tokens
plus recipes) in one file, and the logo lives beside it in `design/`. Do **not** ask brand
questions during init – a sensible default lets documents work immediately. Just record
that the default is in place.

Then seed the design half of the template pair: copy
`${CLAUDE_PLUGIN_ROOT}/references/document-system/report.html` to
`practice/design/Proposal.html` as the starting proposal layout. A file in `templates/` and
a file in `design/` with the **same basename are a pair**: `templates/Proposal.md` holds the
fields, `design/Proposal.html` holds the look, and a render fills the layout from the
markdown. Tell the user in Step 5 that `/fcxo-setup-template` replaces any layout with one
they designed in Claude, and that a new document type is a new pair (`templates/<Doc>.md`
plus `design/<Doc>.html`).

## Step 5 – Confirm

Summarize what was created in plain language (where it is, what's inside, what's still
`TODO`). List the root files by name, `Practice Workspace - Guide.md` and
`Practice Workspace - Integrations.md`, alongside the `me/`, `sop/`, `templates/` and
`design/` starters. Say in one line what `sop/` is for: the procedures they follow, written
down, one file each, which `/fcxo-write-sop` produces and which they then schedule or build
into a skill. Mention that the scaffolded files are wikilinked to each other (the guide and
the `me/` files cross-link), so the workspace opens as a connected graph in Obsidian.

**Say one sentence about integrations.** The toolkit ships no connectors: the user connects
email or a calendar in their own agent surface and writes the tool's name into the empty
rows of `Practice Workspace - Integrations.md`, and the skills read it from there. Add that
a guest-free hold on their own calendar is the only write any skill makes through a
connected tool; everything else is read-only and every outward message stays a draft.

Suggest the natural next steps: `/fcxo-new-client` to add their first
client,
fill in the `TODO`s in `me/`, or `/fcxo-brainstorm` to extract their positioning and
offers through a guided interview if writing them cold feels hard. Keep it to a few
sentences.

**Recommend branding, don't force it.** Note that invoices and documents already work
on a clean default look, that `/fcxo-brand` adjusts the color, logo, and billing details
in `design/DESIGN.md` to make them theirs, and that `/fcxo-setup-template` swaps any
`design/<Doc>.html` layout for one they designed in Claude. Two lines – don't run either
now.

## Notes

- Light, optional frontmatter only – never validate or enforce it.
- No scripts, no automation, no sync. Just structure and starter content.
- If the user already has a similar folder of their own, offer to adopt it (point the
  skills at it) so they keep one workspace.
