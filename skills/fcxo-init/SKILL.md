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
layout, that the FCxO skills read and write here, and the **self-identifying naming
rule** – every file carries its full context in its own name (owner files prefixed with
the owner name, client files with the client name, leads ending in `- Lead`, dated
outputs leading with the date), so no two notes share a basename and skills discover a
file by its `type:` and filename suffix. Note that the `templates/` files are copied out
and renamed to a self-identifying name on use. Per the wiki convention in
`practice-workspace.md`, the guide links the main entry points so the workspace opens as
a connected graph: `[[<Owner> - Profile]]`, `[[<Owner> - Positioning]]`, and
`[[<Owner> - Offers]]`.

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
`clients/`, `leads/`, `content/`, `finance/`, `research/`, `templates/`. Do **not** drop
a per-folder `README.md` – generic basenames collide when the workspace opens in
Obsidian, and the folder names are self-explanatory. The `Practice Workspace - Guide.md`
already describes what each folder holds. A folder comes into being when its first real
file is written (e.g. `templates/` exists once you write the templates below; the others
appear when `/fcxo-new-client` or a skill writes the first file into them). If the user
keeps the workspace under git and wants the empty folders tracked now, add a `.gitkeep`
(never a `README.md`); a plain Obsidian folder does not need one.

### `practice/templates/`
Drop ready-to-copy templates: `Client - Profile.md`, `Engagement.md`, `Lead.md`,
`Invoice.md`. Each is a minimal markdown skeleton with the light frontmatter (`type:`
etc.) from the shared reference. Keep them short. These keep simple template names
because they are **copied out and renamed to a self-identifying name on use** (a
`<Client> - Profile.md`, a `<Client> - <Descriptor> Engagement.md`, a
`<Lead> - Lead.md`); note that in the guide so the convention is clear.

### `practice/me/brand/` (seed the default brand)
Read `${CLAUDE_PLUGIN_ROOT}/references/document-system/DESIGN.default.md` and write it to
`practice/me/brand/DESIGN.md` (file tools, no shell), filling in `brandName` and the
`from` block (and `footerLegal` if known) from the details gathered in Step 2 (leave the
rest at the clean default, accent `#1F6B85`). DESIGN.md is the whole document design system
(tokens + recipes) in one file. Do **not** ask brand
questions during init – a sensible default lets invoices work immediately. Just record that
the default is in place.

## Step 5 – Confirm

Summarize what was created in plain language (where it is, what's inside, what's still
`TODO`). Mention that the scaffolded files are wikilinked to each other (the guide and the
`me/` files cross-link), so the workspace opens as a connected graph in Obsidian rather
than isolated notes. Suggest the natural next steps: `/fcxo-new-client` to add their first
client,
fill in the `TODO`s in `me/`, or `/fcxo-brainstorm` to extract their positioning and
offers through a guided interview if writing them cold feels hard. Keep it to a few
sentences.

**Recommend branding, don't force it.** Note that invoices and documents already work
on a clean default look, and `/fcxo-brand` adjusts the color, logo, and billing details
to make them theirs. One line – don't run it now.

## Notes

- Light, optional frontmatter only – never validate or enforce it.
- No scripts, no automation, no sync. Just structure and starter content.
- If the user already has a similar folder of their own, offer to adopt it (point the
  skills at it) rather than creating a parallel one.
