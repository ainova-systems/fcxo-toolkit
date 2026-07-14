---
name: fcxo-setup-template
description: "Turn a document layout the user designed into the template every future document of that type comes out in. Use when the user says set up a template, add a document template, use this design for my proposals, brand my reports, make my invoices look like this, or I designed a layout in Claude and want to reuse it."
argument-hint: "<document type: Proposal | Report | Invoice | a new one> [design sample: an .html path, pasted HTML, or nothing]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-setup-template - the layout, taught once and reused

The user designs a document in Claude, likes how it looks, and wants every future
proposal, report, or invoice to come out that way. This skill is that bridge: it takes a
design sample and installs it as a document type in the workspace, wired to the user's
brand so the layout follows the brand tokens on every render.

A document type is a **pair matched by basename**:

| Half | File | What it holds |
|---|---|---|
| Data template | `templates/<Doc>.md` | the fields and sections that document type has |
| Design template | `design/<Doc>.html` | how it looks on the page |

A document skill writes markdown against the data template; a render fills the design
template of the same name. `templates/Proposal.md` + `design/Proposal.html` is one type.
This skill creates or replaces both halves together, so they genuinely correspond.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` (where the pair lives) and
`${CLAUDE_PLUGIN_ROOT}/references/document-system/README.md` (the render contract and the
exact injection points). The workspace root is the folder holding `me/`; with no workspace
at all, point the user to `/fcxo-init` first, since this skill writes into the workspace.

## How this differs from /fcxo-brand

`/fcxo-brand` changes the **tokens**: the accent color, the logo, the heading font, the
billing block - the brand every document already uses. `/fcxo-setup-template` changes the
**layout**: the shape of one document type, its regions, and its fields. Brand once,
layouts per document type. A layout installed here keeps reading the tokens, so a later
color change through `/fcxo-brand` restyles it without touching it.

## Step 1 - Take the document type and the sample

`$ARGUMENTS` names the document type (`Proposal`, `Report`, `Invoice`, or a new one the
user names) and, optionally, the sample. Accept the sample in any form:

- an **HTML file** the user designed in Claude (a path, or a file they saved from an
  artifact) - read it with `Read`;
- **pasted HTML** in the conversation;
- **no sample** - offer to start from the plugin's own layout,
  `${CLAUDE_PLUGIN_ROOT}/references/document-system/report.html`, and adjust it with the
  user from there.

If `design/<Doc>.html` already exists, say so and confirm this is a replace before
overwriting. Ask one short question for anything genuinely missing (usually just the
document type).

## Step 2 - Read the sample

Work through the sample and write down three things, because each one maps to a different
part of the result:

1. **Page structure** - the blocks in order (header lockup, title, meta grid, body
   sections, totals, terms, signature, footer). This becomes the fixed HTML skeleton.
2. **Repeating regions** - anything the document has N of: line items, sections,
   findings, next steps, pricing tiers. Each becomes one repeating-region marker.
3. **Hard-coded brand values** - the colors, font stacks, and type sizes the sample froze
   into its CSS. These become tokens.

Everything else - the millimetre spacing, the grid columns, the border widths, the
proportions - is the user's design decision. Keep it exactly as designed.

## Step 3 - Normalize it into the design template

Rewrite the sample into a template that follows the render contract in the document-system
README. Four changes, and no others:

1. **Brand values become tokens.** Replace each hard-coded color, font stack, and type size
   with the matching `var(--token)` from the user's `design/DESIGN.md` vocabulary
   (`--accent`, `--accent-dim`, `--accent-pale`, `--ink`, `--paper`, `--rule`, the status
   tokens `--green` / `--amber` / `--red`, `--font-head` / `--font-body`, the `--fs-*`
   scale, `--tracking-label`, `--logo-size`). Structural geometry stays literal. If the
   sample uses an accent the workspace brand does not have, keep the layout on the token
   and tell the user to run `/fcxo-brand` if they want that color to become their brand.
2. **The injection points go in**, as the shipped templates carry them:
   - `<style>/* @DESIGN_SYSTEM */</style>` - where the render writes the component CSS
     generated from the DESIGN.md recipes;
   - `<style>:root{ /* @BRAND_TOKENS */ }</style>` - where the render emits the tokens;
   - `{{PLACEHOLDER}}` for every single value the document fills, in upper snake case
     (`{{BRAND_NAME}}`, `{{LOGO_SRC}}`, `{{FOOTER_LEGAL}}` are shared by every document;
     the rest are named for this type, e.g. `{{PROPOSAL_TITLE}}`, `{{CLIENT_NAME}}`,
     `{{VALID_UNTIL}}`);
   - one **repeating-region marker** per region found in step 2, as an HTML comment naming
     the region and showing the row markup once, the way `invoice.html` carries
     `<!-- @LINE_ITEMS -->` and `report.html` carries `<!-- @SECTIONS -->` and
     `<!-- @NEXT_STEPS -->`. Name a new one `<!-- @<REGION> -->` in the same style.
3. **The logo goes in shell-free** - inline `<svg>` inside `.mark`, or a `data:` URL as the
   `.mark img` src.
4. **A TEMPLATE NOTES comment goes at the top**, listing the injection steps in order, the
   way the shipped templates do. This is what lets any render skill fill the layout without
   having seen it before. Keep the screen-only toolbar with the Print button and its
   `.no-print` rule, so the user can print straight from the browser.

## Step 4 - Derive the data template from the layout

The design now states exactly what the document needs: every `{{PLACEHOLDER}}` is a field
and every repeating region is a section with N entries. Write `templates/<Doc>.md` as the
blank that matches:

- frontmatter `type: template`, plus `document: <proposal | report | invoice | ...>` naming
  the type the finished document carries;
- one heading per document region, in the order the design lays them out;
- one field line per placeholder, named so the correspondence is obvious;
- a repeating region shown as a list or table with one example row and a note that it
  repeats.

A document skill fills this markdown; the render turns it into the branded HTML. Keep it a
blank, never a record with real client data in it.

## Step 5 - Save the pair and say who uses it

Write both halves under the workspace root:

- `design/<Doc>.html`
- `templates/<Doc>.md`

Then tell the user which skill now produces this document type: a **Proposal** pair is
written by `/fcxo-proposal` and rendered by `/fcxo-report`; a **Report** pair is both
written and rendered by `/fcxo-report`; an **Invoice** pair is used by `/fcxo-invoice`. For
a document type no toolkit skill owns yet, say so plainly and offer `/fcxo-learn-skill` to
build the skill that writes it.

## Step 6 - Verify the pair

Both halves have to line up, so check both directions and report every mismatch:

- every `{{PLACEHOLDER}}` in the design has a field in the data template that supplies it;
- every repeating-region marker has a section in the data template that feeds it;
- every section in the data template has somewhere to land in the design;
- every branded value in the CSS is a `var(--token)`, with no re-hardcoded literal left;
- the file is self-contained: no external stylesheet, no web font, no remote image.

List anything unmatched and ask the user how to resolve it (add the field, drop the
placeholder, or leave it optional and blank).

## Rules

- **One self-contained `.html`.** No external CSS, no web fonts, no remote images. A logo is
  inline `<svg>` or a `data:` URL.
- **Print-first.** The layout has to survive Ctrl/Cmd-P to PDF on A4: page setup in CSS,
  print rules that keep a heading with its section and keep rows and blocks unsplit.
- **Shell-free.** File tools only (`Read`, `Write`, `Edit`, `Glob`).
- **The user's geometry is the point.** Change the brand layer to tokens and add the
  injection points. Leave their spacing, proportions, and structure alone.

## Notes

- The two halves are matched by basename and nothing else, so a design template with no
  data twin renders nothing, and a data template with no design twin still saves as plain
  markdown with no branded output. Create both.
- Replacing a layout is safe: existing documents are already-rendered files and do not
  change. The new layout applies to the next document of that type.
