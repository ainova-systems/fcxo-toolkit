---
# ============================================================================
# FCxO DOCUMENT DESIGN SYSTEM  -  the single source of truth for every
# client-facing document (invoice and report today; proposals render through
# the same system).
#
# The whole design system is THIS ONE FILE: the user edits it (plus a logo)
# and nothing else. The frontmatter below is
# NORMATIVE: every token here is emitted verbatim into a :root{} block at render
# time. The prose body gives the roles, recipes, and rationale - it tells the
# render step how the fixed component CSS is rebuilt from these tokens.
#
# Render mapping (deterministic):
#   - each color key emits  --<key>: <value>   (status keys emit --green/--amber/--red)
#   - each typography stack / size emits its --font-* / --fs-* / --tracking-label
#   - logo.size emits --logo-size
#   - the component recipes in the body reference ONLY these variables for any
#     branded value (color, font, type size); structural geometry (mm spacing,
#     grid columns, border widths) is literal in each recipe.
# Brand changes the tokens; the component recipes and the template HTML stay fixed.
# ============================================================================

colors:
  ink: "#14181d"          # primary text                 -> --ink
  ink-2: "#2b313a"        # body paragraphs              -> --ink-2
  ink-3: "#5b626c"        # secondary / fine print       -> --ink-3
  ink-muted: "#8b919b"    # labels, muted keys           -> --ink-muted
  paper: "#ffffff"        # sheet background             -> --paper
  paper-tint: "#f6f7f8"   # zebra rows, callouts         -> --paper-tint
  accent: "#1F6B85"       # single accent                -> --accent
  accent-dim: "#154E64"   # ~18% darker accent           -> --accent-dim
  accent-pale: "#E7F0F3"  # light tint of accent         -> --accent-pale
  rule: "#e3e6ea"         # hairline rules               -> --rule
  rule-strong: "#14181d"  # strong header rule           -> --rule-strong
  status:
    green: "#16794c"      # paid / positive              -> --green
    amber: "#9a6207"      # pending / warning            -> --amber
    red: "#b4322b"        # overdue / negative           -> --red

typography:
  # System stacks - print-safe, no network fonts.
  font-body: 'ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, Helvetica, Arial, sans-serif'   # -> --font-body
  font-head: "var(--font-body)"   # -> --font-head  (brand may swap to a serif, e.g. "Georgia, serif")
  font-num: 'ui-monospace, "SFMono-Regular", Menlo, Consolas, monospace'   # -> --font-num
  scale:                  # type sizes in pt -> --fs-*
    body: "10.5pt"        # -> --fs-body
    small: "8.5pt"        # -> --fs-small
    label: "8pt"          # -> --fs-label
    h1: "22pt"            # -> --fs-h1
    h2: "15pt"            # -> --fs-h2
    docno: "22pt"         # -> --fs-docno
  tracking-label: "0.12em"   # uppercase label tracking -> --tracking-label
  weights:                # used literally by the component recipes
    regular: 400
    semibold: 600
    bold: 700
  line-heights:           # used literally by the component recipes
    base: 1.5
    paragraph: 1.55
    h1: 1.12
    h2: 1.2

page:
  size: A4                # 210 x 297 mm
  margin: "18mm 16mm 16mm"   # sheet content padding: top, sides, bottom
  print-margin: "14mm"       # @page margin when printing from the browser
  orphans: 3
  widows: 3

logo:
  size: "34px"            # -> --logo-size
  src: null               # inline <svg> markup OR a data: URL; null = wordmark-only.
                          # Store the logo file alongside this DESIGN.md in design/.

# ---- Content / billing (not style tokens) ----------------------------------
brandName: "Your Practice"   # fills the {{BRAND_NAME}} wordmark in the lockup
from:
  name: "TODO: your billing name"
  address: "TODO"
  taxId: "TODO"
  email: "TODO"
  payment: "TODO: IBAN or payment details"
  currency: "EUR"
invoice:
  series: "INV-"
  termsDays: 14
  vat:
    charge: false
    rate: 0
    label: "VAT"
footerLegal: "TODO: legal footer line (registered name, code, address, email)"
---

# FCxO document design system

This is the look and feel behind every client-facing document. It is light,
print-first, and built from system fonts so a finished document is one
self-contained HTML file the user opens in a browser and prints to PDF (no PDF
engine, no installed fonts, no external assets).

You edit one file. The frontmatter above holds the tokens (colors, type, page,
logo, billing); the prose below describes how each component is built from those
tokens. To rebrand, change the tokens. The component recipes and the template
HTML do not change.

## How this design system renders

On render the document skill is deterministic:

1. It emits the frontmatter tokens into a `:root{ --token: value }` block (the
   color, type, tracking, and logo tokens above, each mapped as noted in the
   frontmatter comments).
2. It rebuilds the fixed component CSS from the recipes in this file. Every
   branded value (color, font, type size) is written as `var(--token)`, never as
   a re-hardcoded literal, so the brand lives in one place. Structural geometry
   (mm spacing, grid columns, border widths, fixed weights and line-heights) is
   literal, exactly as each recipe states.
3. It fills the fixed `invoice.html` template (same HTML structure every time)
   with the billing data and line items.

Because the brand layer is only variables and the layout is fixed, two renders
of the same DESIGN.md produce the same document. A render may cache the generated
component CSS and regenerate it only when this file changes.

## Overview and Voice

The identity is a quiet, precise, document-grade brand: a single accent color on
a clean white sheet, generous margins, tabular numbers, and uppercase micro-labels
that orient the eye without shouting. It should read like a well-kept ledger from
a serious practice.

Voice rules for anything rendered through this system:

- One accent, used sparingly. The accent marks the lockup, eyebrow labels, rules,
  and status - never body text.
- Numbers are facts: always right-aligned, always in the tabular mono figure font,
  so columns line up to the decimal.
- Labels are small and calm: 8pt uppercase with wide tracking, in a muted ink, so
  they guide without competing with the content.
- White space does the work. Sections breathe on millimetre spacing; nothing is
  crowded.
- Never invent a color, size, or component outside the tokens and recipes here.

## Colors (roles)

| Role | Hex | Use |
|------|-----|-----|
| ink | `#14181d` | primary text, strong values, the strong header rule |
| ink-2 | `#2b313a` | body paragraphs, table cells, terms |
| ink-3 | `#5b626c` | secondary text, table head labels, footer, fine print |
| ink-muted | `#8b919b` | eyebrow / kv labels, muted keys, the doc-number series |
| paper | `#ffffff` | sheet background |
| paper-tint | `#f6f7f8` | zebra table rows, callout fill |
| accent | `#1F6B85` | lockup, accent eyebrows, callout border, status emphasis |
| accent-dim | `#154E64` | chip text (about 18% darker than accent) |
| accent-pale | `#E7F0F3` | chip fill, tinted emphasis |
| rule | `#e3e6ea` | hairline rules and cell borders |
| rule-strong | `#14181d` | the 1.5px rule under the document header |
| status green | `#16794c` | paid / positive state |
| status amber | `#9a6207` | pending / warning state |
| status red | `#b4322b` | overdue / negative state |

Ink and paper are rarely changed. To rebrand, in practice you change `accent` and
its two derived shades (`accent-dim`, `accent-pale`).

## Typography

Three font stacks: a sans body, a heading font (the body sans by default; a brand
may swap it to a serif), and a mono figure font for all numbers. Sizes are in
points so they are stable in print.

| Style | Font | Size | Weight | Line | Tracking | Use |
|-------|------|------|--------|------|----------|-----|
| Body | body | 10.5pt | 400 | 1.55 | - | paragraphs, table cells (`--ink-2`) |
| Bold inline | body | 10.5pt | 600 | 1.55 | - | `<strong>` emphasis (`--ink`) |
| Small | body | 8.5pt | 400 | 1.5 | - | footer legal, signature note (`--ink-3`) |
| Label / eyebrow | body | 8pt | 600 | - | 0.12em, uppercase | section labels, kv keys (`--ink-muted`; accent variant for Bill to / From) |
| H1 | head | 22pt | 600 | 1.12 | -0.015em | top-level headings |
| H2 | head | 15pt | 600 | 1.2 | -0.01em | section headings |
| Doc number | head | 22pt | 600 | 1.0 | -0.02em | the invoice number |
| Wordmark | head | 14pt | 700 | - | 0.01em | brand name in the lockup |
| Party name | head | 13pt | 600 | - | - | Bill to / From names |
| Grand total | num | 12.5pt | 700 | - | - | the total-due value |
| Numbers | num | 10.5pt | 400 | - | - | all amounts, quantities, dates (tabular figures) |

All numeric figures use the mono font with `font-variant-numeric: tabular-nums`
(and `font-feature-settings: 'tnum' 1`) so digits share one width and decimals
align down a column.

## Page Setup

- Sheet: A4, 210 x 297 mm. The content sits inside `18mm 16mm 16mm` padding
  (top, left/right, bottom).
- On screen the sheet is centred on a soft grey backdrop (`#eceef1`) with a light
  drop shadow, and a small no-print toolbar carries a Print button.
- In print the sheet becomes the page: `@page { size: A4; margin: 14mm; }` (from
  `page.size` and `page.print-margin`; the templates do not restate it), the screen
  chrome and shadow are removed, and links print as plain ink.
- The print block also carries
  `.sheet, .sheet * { -webkit-print-color-adjust: exact; print-color-adjust: exact; }`.
  Chrome's print dialog has "Background graphics" off by default, and without this
  every fill on the sheet - zebra ledger rows, status chip, callouts - prints white.
  Delivery is a printed PDF, so the brand depends on this line.
- Paragraphs cap their measure at roughly 74 characters for readable lines.
- Keep `orphans: 3` and `widows: 3` so a page break never strands one or two lines.
- Delivery is print-from-browser: open the HTML, Ctrl/Cmd-P, Save as PDF.

## Components

Each recipe lists which tokens carry the branded values and the literal geometry
that holds the layout. When the render step rebuilds the CSS, it writes the tokens
as `var(--...)` and the geometry as the literal values stated here.

### Header lockup

The top band: brand lockup on the left, document title and number on the right.

- Container: CSS grid `1fr auto`, gap `14mm`, items aligned to the top. Padding
  below `5mm`, then a `1.5px` solid bottom border in `var(--rule-strong)`.
- Lockup: inline-flex, items centred, gap `9px`. The mark is `var(--logo-size)`
  square; its `<img>`/`<svg>` fills the box with `object-fit: contain`. The
  wordmark is `var(--font-head)`, weight 700, 14pt, letter-spacing `0.01em`,
  color `var(--ink)`.
- Eyebrow (document title): the label style (see below) in `var(--ink-muted)`,
  shown as a block above the number.
- Doc number: `var(--font-head)`, weight 600, `var(--fs-docno)` (22pt),
  line-height 1, letter-spacing `-0.02em`, color `var(--ink)`. The series prefix
  is colored `var(--ink-muted)`.
- Head key/value (issue date, due date): grid `auto auto`, column-gap `4mm`,
  row-gap `0.6mm`, baseline-aligned, right-aligned, margin-top `3mm`. Keys use the
  label style; values use `var(--font-num)` at `var(--fs-body)` in `var(--ink)`.

### Parties key-value (Bill to / From)

Two equal columns of labelled fields.

- Container: grid `minmax(0,1fr) minmax(0,1fr)`, gap `14mm`, margin-top `6mm`.
  The `minmax(0,1fr)` forces a true 50/50 split so a long unbreakable value (an
  IBAN with no spaces) cannot blow out its column.
- Eyebrow per column ("Bill to", "From"): the label style colored `var(--accent)`.
- Party name: `var(--font-head)`, weight 600, 13pt, color `var(--ink)`, margin
  `2mm 0 3mm`.
- Field grid (`.kv`): columns `30mm 1fr`, column-gap `4mm`, row-gap `1mm`,
  baseline-aligned. Keys use the label style; values are `var(--fs-body)` in
  `var(--ink)` with `overflow-wrap: anywhere`. Monospace values (tax id, IBAN) add
  `var(--font-num)`.

### Ledger table

The line-items table.

- Table: full width, `border-collapse: collapse`, `var(--fs-body)`, color
  `var(--ink-2)`, margin-top `6mm`.
- Cells: text-align left, padding `2.6mm 4mm`, bottom border `0.5px` solid
  `var(--rule)`, vertical-align top.
- Head cells: the label style (8pt, 600, `0.12em`, uppercase) in `var(--ink-3)`,
  with a stronger `1px` solid `var(--ink)` bottom border.
- Zebra: even body rows fill with `var(--paper-tint)`.
- Numeric columns (`.n`): right-aligned, `var(--font-num)`, tabular figures. A
  cell can carry `var(--ink)` to darken an emphasized amount.

### Totals

A right-aligned stack of subtotal, optional VAT, and grand total.

- Container: margin-top `5mm`, grid `1fr 100mm`, gap `14mm` (the stack sits in the
  right 100mm column).
- Stack: grid `1fr auto`, row-gap `2.4mm`, column-gap `10mm`, baseline-aligned.
- Keys: the label style in `var(--ink-3)`. Values: `var(--font-num)`,
  `var(--fs-body)`, color `var(--ink)`, right-aligned.
- Divider before the grand total: spans both columns, `1px` solid `var(--ink)`
  top border, margin-top `1mm`.
- Grand total: key colored `var(--ink)` with `2mm` top padding; value in
  `var(--font-num)` at 12.5pt, weight 700, `2mm` top padding, right-aligned.

### Amount in words

A single italic line restating the total in words.

- Block: margin-top `4mm`, padding-top `2mm`, top border `0.5px` solid
  `var(--rule)`, italic, color `var(--ink-2)`.
- Leading key ("Amount in words"): not italic; the label style in
  `var(--ink-muted)` with `3mm` right margin.

### Payment terms

- Container: margin-top `4mm`, grid `30mm 1fr`, column-gap `4mm`, baseline-aligned.
- Key: the label style in `var(--ink-muted)`. Value: color `var(--ink-2)`; any
  `<strong>` inside is `var(--ink)` weight 600.

### Callout

A tinted note box for an important aside.

- Box: padding `5mm 6mm`, background `var(--paper-tint)`, a `2px` left border in
  `var(--accent)`, text color `var(--ink-2)`.
- Key: a block label in `var(--accent)`, the label style, `2mm` bottom margin.

### Chip

A small status pill.

- Inline-flex, items centred, gap `5px`, padding `3px 8px`, background
  `var(--accent-pale)`, text `var(--accent-dim)`, the label style, line-height 1.4,
  border-radius `4px`.

### Signature

- Container: margin-top `8mm`, grid `70mm 1fr`, gap `14mm`, items aligned to the
  bottom. The "Issued by" label uses the label style.
- Signer name: color `var(--ink)`, margin-top `8mm` (leaving room for a wet
  signature). Role line: the label style in `var(--ink-muted)`, margin-top `0.5mm`.
  Optional note: `var(--fs-small)` in `var(--ink-3)`, line-height 1.5.

### Footer

- Container: pushed to the page bottom (`margin-top: auto`), padding-top `3mm`, top
  border `0.5px` solid `var(--rule)`, flex with space-between, gap `10mm`,
  baseline-aligned, `var(--fs-small)`, color `var(--ink-3)`.
- Legal line capped at `120ch`.

### Shared text utilities

These underpin the components and stay fixed: the label/eyebrow style (8pt, weight
600, `var(--tracking-label)` tracking, uppercase, `var(--ink-muted)`; the accent
variant swaps the color to `var(--accent)`); the number style (`var(--font-num)`,
tabular figures); small (`var(--fs-small)`, `var(--ink-3)`); muted
(`var(--ink-muted)`); and the right-align / nowrap helpers. H1 and H2 follow the
typography table above.

## Logo and Lockup

- The logo sits in the header lockup, left of the wordmark, at `var(--logo-size)`
  (34px) square.
- Store it shell-free: prefer inline `<svg>` markup (embedded directly into the
  document), or a `data:` URL for a raster. Keep the file alongside this DESIGN.md
  in `design/`. With no logo, the lockup is wordmark-only.
- Never recolor, stretch, distort, or add effects to the logo. It scales within its
  square box with `object-fit: contain`.

## Do's and Don'ts

- Numbers are always right-aligned and in the tabular mono figure font, so columns
  align to the decimal.
- The accent is for the lockup, eyebrow labels, rules, callout border, and status
  only. Never set body text in the accent.
- Never invent a color outside the token list. If you need emphasis, use an
  existing role (ink, accent, a status color).
- Rebrand by changing tokens only. Never edit the fixed component geometry or the
  template HTML structure - that is what keeps every document consistent and
  print-clean.
- Keep one accent. Two competing accent colors break the quiet, document-grade look.
- To add a new component (e.g. a proposal pricing card), add a new recipe to this
  file in the same token vocabulary - never fork the token model.
