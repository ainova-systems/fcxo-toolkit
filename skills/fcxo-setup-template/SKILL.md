---
name: fcxo-setup-template
description: "Give a document type its look, and keep the content template and the layout matched. Works from either end: a layout designed in Claude, or a markdown template that has no layout yet. Use when the user says set up a template, add a document template, use this design for my proposals, brand my reports, make my invoices look like this, give my invoice template a design, my proposal template has no layout, or I designed a layout in Claude and want to reuse it."
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
This skill builds whichever half is missing and matches it to the half already on disk, so
the two correspond.

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

## Step 1 - Look at what the workspace already has

`$ARGUMENTS` names the document type (`Proposal`, `Report`, `Invoice`, or a new one the
user names) and, optionally, a design sample.

**Before anything else, check both halves of the pair on disk**: `templates/<Doc>.md` and
`design/<Doc>.html`. What is already there decides which way this run goes, and it decides
which file this run is allowed to write. An existing file is the user's work. Adopt it. **A
half that was on disk when the run started is never written over.** Changing one needs a yes
from the user for that file by name.

| On disk | Where the structure comes from | What this run does |
|---|---|---|
| Neither half | the user's answers, or the closest plugin default | build both halves |
| **Data template only** | **`templates/<Doc>.md`** | **build the design half from it; the markdown is not written** |
| Design only | the design half | build the data template from its placeholders; the HTML is not written |
| Both | both, reconciled | verify both halves, report every mismatch, and write nothing without a yes for that specific half |

**The common case is the data template alone**, because a workspace starts with the markdown
blanks `/fcxo-init` seeded and grows layouts later. Read `templates/<Doc>.md`: its headings,
fields and repeating sections are the **spec** for the layout, so the design has to have
somewhere to put every one of them. Then take the design sample if the user has one, and
point them at the block below if they have not made one yet.

**Neither half exists** when the user names a document type the workspace has never carried.
There is no markdown spec to read, so get the spec from the user: ask what the document
contains, section by section, and which fields each section holds. If they would rather see
something first, start from the closest plugin default - `report.html` under
`${CLAUDE_PLUGIN_ROOT}/references/document-system/` for a report-shaped document,
`invoice.html` for a ledger-shaped one - show them the result, and iterate from there.

**With no design sample**, whichever state the workspace is in, build the layout from the
spec plus the workspace brand (`design/DESIGN.md`), starting from the plugin's own
`${CLAUDE_PLUGIN_ROOT}/references/document-system/report.html` skeleton. Show the user the
result and offer to iterate.

A sample can be an **HTML file** the user designed in Claude (a path on disk, or a file saved
out of an artifact), or **pasted HTML** in the conversation. Read a path with `Read`.

## Designing the layout in Claude - what to tell the user

The user has a markdown template and wants their documents to look like something they
designed. Say this to them, in these words:

1. Open a new chat with Claude.
2. Paste the contents of `templates/<Doc>.md` into it. (Offer to print the file for them.)
3. Ask for a one-page A4 HTML layout that carries exactly those sections.
4. Look at what comes back, say what to change, and keep going until it looks right.
5. Save the artifact and give me the file path, or paste the HTML straight into this
   conversation.

Then this skill turns that HTML into the workspace's design template for `<Doc>`, and every
future `<Doc>` comes out in that layout.

## When the design half is already a template

If `design/<Doc>.html` was on disk and already carries the injection points - a
`/* @DESIGN_SYSTEM */` comment, a `/* @BRAND_TOKENS */` comment, `{{PLACEHOLDER}}` values,
and a repeating-region marker - it is a finished design template already. Adopt it as it
stands, skip Steps 2 and 3, and go to Step 6 to check it against the data template.

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
2. **The injection points go in**, as the plugin's own templates carry them:
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
   way the plugin's templates do. This is what lets any render skill fill the layout without
   having seen it before. Keep the screen-only toolbar with the Print button and its
   `.no-print` rule, so the user can print straight from the browser.

## Step 4 - Match the data template

The two halves have to correspond, and which one you write depends on what Step 1 found.

**The data template already exists.** It is the user's, and it stays. Read it again and
match the design to it: every heading and field in the markdown needs a `{{PLACEHOLDER}}`
or a repeating region in the layout that displays it. Adjust the **design** to fit the
markdown. Where the sample's design carries something the markdown has no field for (a
validity date, a signature block), name it and ask: add the field, or drop it from the
layout. The markdown file is written only if the user says yes to that file by name.

The same constraint holds for the design half when **both** halves were on disk. Then this
step is a reconciliation and nothing more: report every mismatch in both directions, and
change `design/<Doc>.html` only after a yes for that file. Neither half is rewritten on the
strength of the other.

**There is no data template yet.** Write `templates/<Doc>.md` as the blank the design
implies. Every `{{PLACEHOLDER}}` is a field and every repeating region is a section with N
entries:

- frontmatter `type: template`, plus `document: <proposal | report | invoice | ...>` naming
  the type the finished document carries;
- one heading per document region, in the order the design lays them out;
- one field line per placeholder, named so the correspondence is obvious;
- a repeating region shown as a list or table with one example row and a note that it
  repeats.

A document skill fills this markdown; the render turns it into the branded HTML. Keep it a
blank: field names, an example row, and the note that the row repeats. Never put real client
data in it.

## Step 5 - Save the half this run built, and say who uses the pair

Write only the half this run built. Step 1 already decided which one that is:

- **the data template was on disk** - the only file written is `design/<Doc>.html`, and
  `templates/<Doc>.md` is never opened for writing;
- **the design half was on disk** - the only file written is `templates/<Doc>.md`, and
  `design/<Doc>.html` is never opened for writing;
- **neither half was on disk** - write both, `design/<Doc>.html` and `templates/<Doc>.md`;
- **both were on disk** - write neither, unless the user said yes to one file by name, and
  then write that one file.

A half that existed when the run started is never written over.

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

- **An existing half is the user's file.** Whatever was on disk when the run started stays
  exactly as it is. This run writes the missing half. Changing a file that was already there
  needs a yes from the user for that file by name.
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
  markdown with no branded output. A working document type has both, and this run supplies
  the one the workspace is missing.
- Replacing a layout is safe: existing documents are already-rendered files and do not
  change. The new layout applies to the next document of that type.
