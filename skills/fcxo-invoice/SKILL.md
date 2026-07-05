---
name: fcxo-invoice
description: "Generate a branded invoice for a fractional executive from an engagement and their billing details. Use when the user wants to create an invoice, bill a client, or generate a bill."
argument-hint: "<client/engagement> [period or hours/amount]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-invoice – a branded invoice, ready to print

Produce an invoice the user can send, using the billing details and rate already in the
workspace so they don't re-enter them every month. The output is **one self-contained
`.html` file**: the user opens it in a browser and prints (Ctrl/Cmd-P → Save as PDF).
No PDF engine, no installed fonts.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` (where data lives) and
`${CLAUDE_PLUGIN_ROOT}/references/document-system/README.md` (the build model).

## Step 1 – Gather the data

- **Brand + biller** – read `me/brand/DESIGN.md` under the workspace root (the folder
  holding `me/`). If it doesn't exist, use
  `${CLAUDE_PLUGIN_ROOT}/references/document-system/DESIGN.default.md` and tell the
  user (once) they can run `/fcxo-brand` to set their color, logo, and billing details.
  Pull biller name/address/tax/payment/currency and `footerLegal` from its frontmatter
  (or from `me/*- Profile.md` if the brand file is incomplete). The header wordmark
  (`{{BRAND_NAME}}`) uses `brandName`; the `From` block uses the legal `from.name`. They
  are often the same, but fall back to `from.name` for the wordmark only if `brandName` is
  missing.
- **Client** – billing name and address from the client profile; ask if absent.
- **Rate / scope / terms** – from the engagement's `*- Engagement.md` file.
- **What to bill** – from `$ARGUMENTS`: a retainer period ("June"), hours × rate, or a
  fixed amount. Ask in one batched question only for what's genuinely missing.

## Step 2 – Compute the invoice

- **Invoice number** – look in the workspace `finance/` folder for the last invoice and
  increment;
  if none, start from the DESIGN.md `invoice.series` (e.g. `INV-2026-001`) and confirm.
- **Dates** – issue date today; due date = issue + `invoice.termsDays` (default 14).
- **Line items** – description, qty, unit price, amount. Retainer → one line for the
  period; hourly → hours × rate.
- **Totals** – subtotal; VAT only if `invoice.vat.charge` is true (use its rate
  and label); total due. Double-check the arithmetic.
- **Amount in words** – render the total in words in the invoice currency.

## Step 3 – Build the file

Build from `${CLAUDE_PLUGIN_ROOT}/references/document-system/invoice.html` per the render
contract in `${CLAUDE_PLUGIN_ROOT}/references/document-system/README.md`. The template
HTML structure is fixed; only the brand/design layer comes from DESIGN.md:

1. **Tokens → `:root`.** Emit every DESIGN.md frontmatter token into the
   `/* @BRAND_TOKENS */` block as `--token: value`: each `colors` key → `--<key>` (the
   `status` keys → `--green` / `--amber` / `--red`); `typography.font-*` → `--font-*`;
   `typography.scale.*` → `--fs-*`; `typography.tracking-label` → `--tracking-label`;
   `logo.size` → `--logo-size`. Use `font-head` verbatim (it is `var(--font-body)` by
   default, or a real stack like `Georgia, serif`). Emit the full set, not just `--accent`,
   or the derived shades and fonts fall back to the defaults. (`brandName` is content, not
   a token: it fills the `{{BRAND_NAME}}` wordmark in step 4.)
2. **Recipes → `/* @DESIGN_SYSTEM */`.** Generate the component CSS from the DESIGN.md
   "Components" recipes (header lockup, parties, ledger, totals, amount-in-words, terms,
   callout, chip, signature, footer, shared utilities, plus the sheet / page setup),
   writing every branded value as `var(--token)` and never as a re-hardcoded literal;
   structural geometry (mm spacing, grid columns, border widths, weights, line-heights) is
   the literal value each recipe states. The result is the fixed stylesheet rebuilt from
   the tokens, so the look is identical across renders.
3. **Logo, shell-free.** Place the logo in `.mark`: an **SVG** is inlined as `<svg>`
   markup directly (replace the `<img>`), no encoding; a **raster** uses the `data:` URL in
   DESIGN.md `logo.src` as the `.mark img` src. With no logo, remove `.mark`
   (wordmark-only). Never shell out to encode an image; if only a raw raster file exists
   with no stored `data:` URL, ask for an SVG or a pre-encoded `data:` URL, or fall back to
   wordmark-only.
4. **Fill the template.** Fill every `{{PLACEHOLDER}}` and emit one `<tr>` per line item at
   `<!-- @LINE_ITEMS -->`. Omit the VAT row entirely if VAT is off.

Render the payment account / IBAN **compact, with no spaces** (`LT001234…`, not
`LT00 1234 …`) so it copies cleanly into a bank transfer.

## Step 4 – Confirm and save

Show the user the computed numbers (subtotal, VAT, total, number, dates) in chat to
confirm before finalizing. Save the invoice as `finance/<invoice-no>.html` and record a
companion note `finance/<invoice-no>.md` with frontmatter `type: invoice`,
`status: draft`, the number, client, amount, and dates (so `/fcxo-pipeline-review`
can read it). Per the wiki convention in `practice-workspace.md`, the
markdown note links the engagement it bills (`[[<Client> - <Descriptor> Engagement]]`)
and the client (`[[<Client> - Profile]]`); keep these links out of the client-facing
`.html`. Then present the `.html` file and tell the user to open it and print to PDF.

## Notes

- Never invent a tax id, IBAN, or VAT rate – use the brand file or ask.
- The skill prepares the invoice; the user opens, prints, and sends it. When the user
  says they sent it or got paid, update the companion note's `status:` (`draft` →
  `sent` → `paid`) so the pipeline review reflects reality.
- Keep the invoice one page. If line items overflow, the browser paginates cleanly via
  the print CSS.
