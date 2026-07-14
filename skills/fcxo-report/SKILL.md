---
name: fcxo-report
description: "Render an existing document (proposal, report, deliverable) as a branded, self-contained HTML file beside its markdown, or compose the report first when none exists. Use when the user wants a client report, an assessment report, a branded document, to render a proposal or deliverable, or to turn a memo into a client PDF."
argument-hint: "<company> <document type> | <topic or source file> [language]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-report - the deliverable, composed and rendered

Turn a document into the client-facing artifact: one self-contained `.html` file on the
user's document brand, which they open in a browser and print (Ctrl/Cmd-P, Save as PDF).
No PDF engine, no installed fonts, no external assets.

Two modes, and Step 0 decides which one you are in. **Render-only** is the common one: the
document already exists (a proposal, a decision report), so it is rendered exactly as
written. **Compose** is for when the deliverable has not been written yet: build the
markdown from the engagement's material first, save it as the durable record, then render
it the same way.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` (where data lives) and
`${CLAUDE_PLUGIN_ROOT}/references/document-system/README.md` (the render contract).

## Step 0 - Resolve what you are rendering (do this before anything else)

`$ARGUMENTS` is normally just a company and a document type: `Talvora Analytics proposal`,
`Orlend Labs renewal review`, `Halverd Robotics report`. That is enough. Resolve the rest
yourself and ask nothing.

1. **The company.** `Glob leads/**/*- Lead.md` and `Glob clients/*/*- Profile.md`, and
   match the name in the argument against the company names, the folder slugs, and the
   `domain:` frontmatter. That match gives you `<company>` = `leads/<slug>/` or
   `clients/<slug>/`.
2. **The document.** The document-type word in the argument tells you where to look inside
   that folder: a **proposal** or SOW lives in `<company>/proposals/*.md`; a **report**,
   **assessment**, **decision report**, **review**, or any other deliverable lives in
   `clients/<slug>/engagements/*/deliverables/*.md`. `Glob` that folder and take the
   **most recent by date prefix**; when two carry the same date, take the one whose name
   matches the document type. A path or filename in the argument overrides all of this:
   use the file named.
3. **Then branch, and this is the default:**
   - **A source markdown exists → render-only.** The content is already written and owned
     by the skill that wrote it (`/fcxo-proposal`, `/fcxo-decision-report`). **Skip Step 2
     entirely. Keep the content exactly as written**: no rewriting, no reordering, no
     "improving" a heading, no added section, no dropped sentence. Go straight to Step 3,
     render, and save the `.html` sibling. Ask nothing.
   - **No source markdown exists → compose it** (Step 1, Step 2), then render.

Branded, self-contained, and saved beside the markdown is what this skill always does;
none of that needs to be asked for.

## Step 1 - Gather the material (compose mode only)

- **Workspace root** - the folder holding `me/`. Pull the user's **role** from
  `me/*- Profile.md`; it sets the lens for the whole report.
- **Sources** - whatever the engagement holds under `clients/<slug>/engagements/<id>/`:
  a decision memo from `/fcxo-decision-report`, survey findings from
  `/fcxo-research-form`, call summaries in `meetings/`, working notes, and whatever
  findings the user pastes in this conversation. The language defaults to the user's.
- **Standalone** (no workspace) - compose the report in chat from what the user
  provides, and offer to render once a workspace exists: the render needs the brand
  file (`design/DESIGN.md` or the plugin default) and a place to save.

Ask one short batched question only for what is genuinely missing (at minimum: what was
reviewed and for whom).

## Step 2 - Compose (markdown, the durable record; compose mode only)

The report is a senior deliverable; hold the same bar as `/fcxo-decision-report`:

1. **Executive summary** - the verdict and what to do about it. A reader who stops
   here knows the outcome.
2. **Context and method** - what was reviewed, over what period, and how; the
   engagement scope this report answers.
3. **Findings** - each finding evidence-backed (a fact, a number, a named source) and
   severity-marked where severity fits (high / medium / low). Mark assumptions as
   assumptions; never present one as a finding.
4. **Recommendations** - each with effort and impact, in one clear priority order.
   The ranking is what the user is paid for; be decisive.
5. **Next steps / roadmap** - concrete, owned, time-bound where possible.

Frame findings and recommendations in the user's **role lens** (a CFO report weighs
cash and risk; a CMO report weighs growth and positioning; a CTO report weighs
architecture and delivery) without faking domain depth the user has not supplied. Apply
the copy principles for prose (no em-dash, no AI-slop, plain professional language;
read `${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md`).

## Step 3 - Render (branded, self-contained)

**Brand** - read `design/DESIGN.md` under the workspace root. If it does not exist,
use `${CLAUDE_PLUGIN_ROOT}/references/document-system/DESIGN.default.md` and tell the
user (once) they can run `/fcxo-brand` to set their color, logo, and details.

**Layout** - the design template is paired to the document type by basename, so a report
renders in `design/Report.html`, a proposal in `design/Proposal.html`, an invoice in
`design/Invoice.html`. The document type is the one you resolved in Step 0 (and it is
confirmed by the source's `type:` frontmatter and the folder it sits in), so pick
`design/<Doc>.html` for that type yourself: a proposal renders in `design/Proposal.html`
without anyone naming the file. The user designs a layout once, drops it in `design/`
(`/fcxo-setup-template` builds the pair from a design sample), and every future document
of that type comes out in it. When the workspace holds no design template for this type,
fall back to `${CLAUDE_PLUGIN_ROOT}/references/document-system/report.html`.

Fill the chosen layout per the render contract in the document-system README. The
template HTML structure is fixed; only the brand/design layer comes from DESIGN.md:

1. **Tokens → `:root`.** Emit every DESIGN.md frontmatter token into the
   `/* @BRAND_TOKENS */` block as `--token: value`: each `colors` key → `--<key>` (the
   `status` keys → `--green` / `--amber` / `--red`); `typography.font-*` → `--font-*`;
   `typography.scale.*` → `--fs-*`; `typography.tracking-label` → `--tracking-label`;
   `logo.size` → `--logo-size`. Emit the full set, including the derived
   `--accent-dim` / `--accent-pale`. The status tokens matter here: the severity chips
   use them.
2. **Recipes → `/* @DESIGN_SYSTEM */`.** Generate the component CSS from the DESIGN.md
   "Components" recipes this template uses (sheet / page setup, toolbar, header lockup,
   key-value grid, ledger, callout, chip, footer, shared utilities), writing every
   branded value as `var(--token)`, never a re-hardcoded literal; structural geometry
   is the literal value each recipe states. The layout's own template CSS (cover,
   executive summary, section heading rule, finding, chip severity variants, next steps,
   page breaks - the third `<style>` block in the plugin template) is fixed; leave it
   exactly as the layout ships it.
3. **Logo, shell-free.** Inline an SVG as `<svg>` markup in `.mark`, or use the
   `data:` URL from DESIGN.md `logo.src` as the `.mark img` src; with no logo, remove
   `.mark` (wordmark-only). Never shell out to encode an image.
4. **Fill the template.** Fill every `{{PLACEHOLDER}}` and every `<!-- @MARKER -->` the
   chosen layout declares; a workspace design template uses the same injection markers,
   so read it and fill what it asks for. A workspace layout carries the placeholders of
   its own document type (a proposal layout asks for the situation, the scope, the start
   date, the investment, the terms, the next step), and each one is filled from the
   matching section of the source markdown, **word for word in render-only mode**. Where
   the layout wants a value the markdown states in prose (a start date, a figure), lift it
   from that prose; never restate it in your own words and never compute a value the
   document does not carry. A placeholder with nothing behind it in the source is dropped
   with its block, never filled with an invention. In the plugin template that is `{{BRAND_NAME}}`,
   `{{DOC_TYPE}}`, `{{REPORT_TITLE}}`, `{{CLIENT_NAME}}`, `{{AUTHOR_LINE}}`,
   `{{REPORT_DATE}}`, `{{EXEC_SUMMARY}}`, `{{FOOTER_LEGAL}}`. Emit one
   `<section class="report-section">` per report section at `<!-- @SECTIONS -->`:
   findings as `.finding` blocks with a `chip--green` / `chip--amber` / `chip--red`
   severity chip; recommendation tables on the `.ledger` pattern with
   Recommendation / Effort / Impact columns; asides as `.callout`. Emit one `<li>` per
   step at the `@NEXT_STEPS` marker.

Content flows across pages; the template's print CSS keeps headings with their
sections and keeps findings and table rows unsplit. Keep every `[[wikilink]]` out of
the HTML - it is the client-facing file.

## Step 4 - Save and deliver

The rendered `.html` is always a **sibling of its markdown**: same folder, same basename,
`.html` extension. That rule alone decides the path, so it never needs to be given.

**A proposal** - the source markdown carries frontmatter `type: proposal`, or it sits in
a `proposals/` folder. `/fcxo-proposal` owns that markdown; this skill only adds the
rendered sibling beside it:

- **HTML (client-facing)** - `<company>/proposals/<YYYY-MM-DD> - <Company> Proposal.html`,
  in the same `proposals/` folder as the markdown. `<company>` resolves to `leads/<slug>/`
  before the contract and `clients/<slug>/` after it, which is what lets a proposal render
  at lead stage, when no engagement folder exists yet.
- Leave the markdown as `/fcxo-proposal` wrote it; if that skill has not saved it yet,
  save it to the same `proposals/` folder under the same name with the `.md` extension.

**An engagement deliverable** - a report, assessment, decision report, or review:

- **Markdown (the durable record)** - save to
  `clients/<slug>/engagements/<id>/deliverables/<YYYY-MM-DD> - <Topic> Report.md` with
  frontmatter `type: deliverable`, `status: draft`, `updated: <date>`. Per the wiki
  convention in `practice-workspace.md`, link the client (`[[<Client> - Profile]]`)
  and the engagement (`[[<Client> - <Descriptor> Engagement]]`).
- **HTML (client-facing)** - save the rendered file as a sibling in the same folder:
  `.../deliverables/<YYYY-MM-DD> - <Topic> Report.html`. No wikilinks inside.

Either way: no wikilinks inside the `.html`, it is the client-facing file. Tell the user
to open it in a browser and print to PDF (Ctrl/Cmd-P, Save as PDF). When they say it went
to the client, update the markdown note's `status:` (`draft` → `sent`).

## Where this sits (hand-offs)

- `/fcxo-decision-report` and `/fcxo-proposal` own their content and hand the
  rendering to this skill. Their finished markdown is rendered as written; the substance
  is theirs and this skill leaves it alone.
- `/fcxo-brand` owns the look. Never adjust tokens or recipes here; send the user
  there for brand changes.

## Quality gate (before delivering)

- **Render-only mode**: every sentence in the `.html` appears in the source markdown, and
  the source markdown is byte-for-byte what it was before the run.
- **Compose mode**: the executive summary alone delivers the verdict and the top
  recommendation; every finding cites its evidence; every assumption is marked as one;
  recommendations carry effort and impact and stand in one clear priority order.
- The HTML is one self-contained file: full token set emitted, no external assets, no
  wikilinks, prints clean on A4.
- The `.html` sits beside its markdown, under the same basename.
