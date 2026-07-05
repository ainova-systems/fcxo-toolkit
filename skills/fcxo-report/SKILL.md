---
name: fcxo-report
description: "Compose a client-ready report from engagement material and render it as a branded document the user prints to PDF. Use when the user wants a client report, an assessment report, a branded report, to render a deliverable as a document, or to turn a memo into a client PDF."
argument-hint: "<topic or source deliverable> [client/engagement] [language]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-report - the deliverable, composed and rendered

Produce the client-facing deliverable at the end of an engagement phase: compose a
report from the material the engagement already holds, then render it on the user's
document brand. Two steps in one skill: **compose** (clean markdown, the durable record
in the workspace) and **render** (one self-contained `.html` file the user opens in a
browser and prints, Ctrl/Cmd-P, Save as PDF). No PDF engine, no installed fonts, no
external assets.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` (where data lives) and
`${CLAUDE_PLUGIN_ROOT}/references/document-system/README.md` (the render contract).

## Step 1 - Gather the material

- **Workspace root** - the folder holding `me/`. Pull the user's **role** from
  `me/*- Profile.md`; it sets the lens for the whole report.
- **Sources** - whatever the engagement holds under `clients/<slug>/engagements/<id>/`:
  a decision memo from `/fcxo-decision-report`, survey findings from
  `/fcxo-research-form`, call summaries in `meetings/`, working notes, and whatever
  findings the user pastes in this conversation. `$ARGUMENTS` names the topic or the
  source file, optionally the client/engagement and a language (default: match the
  user).
- **Standalone** (no workspace) - compose the report in chat from what the user
  provides, and offer to render once a workspace exists: the render needs the brand
  file (`me/brand/DESIGN.md` or the plugin default) and a place to save.

Ask one short batched question only for what is genuinely missing (at minimum: what was
reviewed and for whom).

## Step 2 - Compose (markdown, the durable record)

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

**Brand** - read `me/brand/DESIGN.md` under the workspace root. If it does not exist,
use `${CLAUDE_PLUGIN_ROOT}/references/document-system/DESIGN.default.md` and tell the
user (once) they can run `/fcxo-brand` to set their color, logo, and details.

Build from `${CLAUDE_PLUGIN_ROOT}/references/document-system/report.html` per the
render contract in the document-system README. The template HTML structure is fixed;
only the brand/design layer comes from DESIGN.md:

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
   is the literal value each recipe states. The template's third `<style>` block
   (cover, executive summary, section heading rule, finding, chip severity variants,
   next steps, page breaks) is fixed template CSS; leave it exactly as shipped.
3. **Logo, shell-free.** Inline an SVG as `<svg>` markup in `.mark`, or use the
   `data:` URL from DESIGN.md `logo.src` as the `.mark img` src; with no logo, remove
   `.mark` (wordmark-only). Never shell out to encode an image.
4. **Fill the template.** Fill every `{{PLACEHOLDER}}` (`{{BRAND_NAME}}`,
   `{{DOC_TYPE}}`, `{{REPORT_TITLE}}`, `{{CLIENT_NAME}}`, `{{AUTHOR_LINE}}`,
   `{{REPORT_DATE}}`, `{{EXEC_SUMMARY}}`, `{{FOOTER_LEGAL}}`). Emit one
   `<section class="report-section">` per report section at `<!-- @SECTIONS -->`:
   findings as `.finding` blocks with a `chip--green` / `chip--amber` / `chip--red`
   severity chip; recommendation tables on the `.ledger` pattern with
   Recommendation / Effort / Impact columns; asides as `.callout`. Emit one `<li>` per
   step at the `@NEXT_STEPS` marker.

Content flows across pages; the template's print CSS keeps headings with their
sections and keeps findings and table rows unsplit. Keep every `[[wikilink]]` out of
the HTML - it is the client-facing file.

## Step 4 - Save and deliver

- **Markdown (the durable record)** - save to
  `clients/<slug>/engagements/<id>/deliverables/<YYYY-MM-DD> - <Topic> Report.md` with
  frontmatter `type: deliverable`, `status: draft`, `updated: <date>`. Per the wiki
  convention in `practice-workspace.md`, link the client (`[[<Client> - Profile]]`)
  and the engagement (`[[<Client> - <Descriptor> Engagement]]`).
- **HTML (client-facing)** - save the rendered file as a sibling in the same folder:
  `.../deliverables/<YYYY-MM-DD> - <Topic> Report.html`. No wikilinks inside.
- Tell the user to open the `.html` in a browser and print to PDF. When they say it
  went to the client, update the markdown note's `status:` (`draft` → `sent`).

## Where this sits (hand-offs)

- `/fcxo-decision-report` and `/fcxo-proposal` own their content and hand the
  rendering to this skill: re-render their finished markdown without rewriting it
  (light restructuring into the report sections is fine; the substance is theirs).
- `/fcxo-brand` owns the look. Never adjust tokens or recipes here; send the user
  there for brand changes.

## Quality gate (before delivering)

- The executive summary alone delivers the verdict and the top recommendation.
- Every finding cites its evidence; every assumption is marked as one.
- Recommendations carry effort and impact and stand in one clear priority order.
- The HTML is one self-contained file: full token set emitted, no external assets, no
  wikilinks, prints clean on A4.
