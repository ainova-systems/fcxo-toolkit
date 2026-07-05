# Document design system

The brand layer behind every client-facing document (invoice and report today;
proposals render through the same system). One source of truth for look-and-feel, so
all documents match and the user never hand-builds a template.

The whole design system is **one human-readable file**: `DESIGN.md`. It holds the brand
tokens in its frontmatter and the component recipes in its prose body - the user edits
this one file (plus a logo) and nothing else.

## Files

- `DESIGN.default.md` – the default design system. YAML frontmatter with the normative
  tokens (colors by role, typography, page setup, logo, and the billing block:
  `from` / `invoice` / `footerLegal`) plus a Markdown body of roles, component recipes,
  and rationale. The brand skill copies this to `me/brand/DESIGN.md` under the workspace
  root and the user edits it. Light, print-first, system fonts (no network).
- `invoice.html` – the invoice template. Self-contained-ready: a fixed HTML structure plus
  clearly marked injection points (`/* @DESIGN_SYSTEM */`, `/* @BRAND_TOKENS */`,
  `<!-- @LINE_ITEMS -->`, `{{...}}`). Only the brand/design layer is injected; the body
  structure never changes.

## Delivery model (deliberately simple)

A finished document is **one self-contained `.html` file**. The user opens it in a
browser and prints (Ctrl/Cmd-P → Save as PDF). No PDF engine, no installed fonts, no
external assets. It is also assembled **shell-free**, with the file tools only (no
`Bash`), so it works the same in a Claude Cowork session or Claude Code: a logo is
embedded as inline `<svg>` (preferred) or a `data:` URL, never base64-encoded via a
shell at build time. The template includes a screen-only toolbar with a Print button;
the toolbar is hidden in print via `.no-print`.

If a user later wants one-click PDF without the browser step, that is an optional
enhancement (a headless renderer such as WeasyPrint, or the `pdf` skill) – not required
for the default flow.

## Render contract (performed by fcxo-invoice / fcxo-brand)

The render is deterministic - the same `DESIGN.md` always produces the same document:

1. Read `practice/me/brand/DESIGN.md` (or use `DESIGN.default.md` if none).
2. **Tokens → `:root`.** Emit every frontmatter token into the `/* @BRAND_TOKENS */`
   block as `--token: value`: each `colors` key → `--<key>` (the `status` keys →
   `--green` / `--amber` / `--red`); `typography.font-*` → `--font-*`;
   `typography.scale.*` → `--fs-*`; `typography.tracking-label` → `--tracking-label`;
   `logo.size` → `--logo-size`. Use `font-head` verbatim (default `var(--font-body)`, or a
   real stack like `Georgia, serif`). Always emit the full set, including the derived
   `--accent-dim` / `--accent-pale`, or they fall back to the defaults. (`brandName` is
   content, not a token: it fills the `{{BRAND_NAME}}` wordmark in step 4.)
3. **Recipes → `/* @DESIGN_SYSTEM */`.** Generate the component CSS from the DESIGN.md
   "Components" recipes (header lockup, parties, ledger, totals, amount-in-words, terms,
   callout, chip, signature, footer, shared utilities, plus the sheet / page setup). Write
   every branded value as `var(--token)`, never a re-hardcoded literal, so the brand lives
   only in the `:root` tokens; structural geometry (mm spacing, grid columns, border
   widths, fixed weights and line-heights) is the literal value each recipe states.
4. **Fill the template.** Embed the logo shell-free (inline `<svg>`, or the `data:` URL
   from `logo.src`; drop `.mark` for wordmark-only), fill every `{{PLACEHOLDER}}`, and emit
   the `<!-- @LINE_ITEMS -->` rows.
5. Save the result to `practice/finance/` and tell the user to open and print it.

**Determinism.** Because the component CSS references only the `:root` variables and the
template HTML structure is fixed, the look depends solely on DESIGN.md. A render may cache
the generated component CSS and regenerate it only when DESIGN.md changes.

## What brand controls

Brand changes **tokens only** - the frontmatter of DESIGN.md. In practice that is the
accent and its two derived shades (`accent` / `accent-dim` / `accent-pale`), the heading
font (`font-head`: system or a serif), the logo (`logo.src` / `logo.size`), and the
billing block. Everything else (ink, paper, rules, status colors, the type scale, the
component recipes, the page setup) stays fixed for consistency. The brand name is content
(the `{{BRAND_NAME}}` wordmark), not a style token. To extend the system, add a new
component recipe to DESIGN.md additively in the same token vocabulary; never fork the
token model.
