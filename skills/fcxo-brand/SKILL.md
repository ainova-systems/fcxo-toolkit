---
name: fcxo-brand
description: "Set up or adjust the visual brand for a fractional executive's documents: accent color, logo, fonts, and billing details. Use when the user wants to set up branding, change a brand color, add a logo, or match their website."
argument-hint: "[color | logo path | website url | a sample document to match]"
allowed-tools: Read, Write, Edit, Glob, WebFetch
---

# fcxo-brand - your document look, set once and reused

Establish or adjust the brand that every client-facing document uses. The output is a
single human-readable design system stored in the workspace at
`me/brand/DESIGN.md` (plus a logo file in `me/brand/`). Documents read this one
file, so the user sets their look once and never touches a template. DESIGN.md is the
whole brand in one file.

`DESIGN.md` is YAML frontmatter with the normative tokens (colors, typography, page,
logo, billing) plus a Markdown body that describes how each component is built from those
tokens. Read `${CLAUDE_PLUGIN_ROOT}/references/document-system/README.md` for the render
model, and `${CLAUDE_PLUGIN_ROOT}/references/document-system/DESIGN.default.md` for the
exact token shape and the component recipes.

## The fallback chain (keep it one decision for the user)

Locate `me/brand/DESIGN.md` under the workspace root (the folder holding `me/`). If no
workspace exists at all (no `me/*- Profile.md` anywhere), point the user to
`/fcxo-init` first - the brand lives in the workspace.

1. **It exists** - this is an *adjust*. Apply the change the user asked for (new accent
   color, new logo, swap to a serif heading font, fix billing details) by editing the
   relevant token(s) in the frontmatter, and re-save. Done.
2. **It does not exist** - set it up. Offer these three paths and let the user pick one:
   - **Import** - the user pastes a design system or token set from Claude (a Claude
     design / artifact) or another top design tool, or points to a website URL or a
     branded document (a PDF, an image, another invoice). Read or fetch it (WebFetch for a
     URL), distil its dominant accent color, the heading-font feel (serif vs sans), and
     the logo, and map them into the DESIGN.md token + recipe shape. Recommended when they
     already have a brand somewhere.
   - **Build** - the user gives a color (and a logo if handy), or just a few words.
     Derive the full token set around that accent (`accent-dim` about 18% darker,
     `accent-pale` a light tint) and write DESIGN.md. Use this when there is nothing to
     copy from.
   - **Default** - copy `${CLAUDE_PLUGIN_ROOT}/references/document-system/DESIGN.default.md`
     as-is (accent `#1F6B85`). Good enough to send; refine later.

In all cases also capture the **business / billing details** into the `from` block
(name, address, taxId, email, payment, currency), the `invoice` block (series, termsDays,
vat), and the `footerLegal` line, since invoices need them. Pull what already exists from
`me/*- Profile.md` first; only ask for genuine gaps. `brandName` is content (the wordmark),
not a style token.

## Write DESIGN.md

Write `me/brand/DESIGN.md` from `DESIGN.default.md`, overriding only what the
chosen path produced:

- `colors.accent` (and derive `colors.accent-dim` about 18% darker and
  `colors.accent-pale` a very light tint of it). Leave ink / paper / rule / status at the
  defaults unless the user explicitly wants a change.
- `typography.font-head` (`"var(--font-body)"` for system, or a named serif stack like
  `"Georgia, serif"`).
- `logo.src` - store it so documents embed it with **no shell**. Prefer an **SVG**: keep
  the `<svg>` markup so documents inline it as text. For a **raster** logo, store a
  `data:` URL string; if the session cannot encode the image, ask the user for a `data:`
  URL or an SVG. Write any logo file into `me/brand/` with the file tools.
- `brandName`, the `from` block, `invoice`, and `footerLegal`.

Do not edit the component recipes in the prose body unless the user explicitly wants a
structural change - the recipes are shared and fixed so every document stays consistent.

## Show one sample

Render a sample invoice with the new brand (reuse the `fcxo-invoice` build steps with
placeholder data) and tell the user to open it in a browser to eyeball the look. Iterate
on the tokens (accent / logo / font) from their feedback. Keep iterations to the tokens.

## Extending the system (new components)

When a later document type needs a component the recipes don't have yet (e.g. a proposal
needs a pricing-tier card), add a new component **recipe** to the user's DESIGN.md
**additively**, in the same token vocabulary (reference `--accent`, `--ink`, the existing
type and spacing tokens). Never fork the token model. The system grows; it stays one
source of truth in one file.

## Notes

- Keep it genuinely simple. Most users should be done in one answer (a color) plus
  confirming billing details.
- Tokens control the brand; the component recipes and the template stay fixed so every
  document is consistent and print-clean.
- Store nothing sensitive beyond the user's own billing details they chose to record.
