---
name: fcxo-proposal
description: "Draft the proposal, SOW, or letter of agreement that converts a qualified lead into an engagement: the buyer's problem in their words, a concrete scope with clear edges, one price anchored to the user's offers, and one next step. Use when the user wants to write a proposal, draft an SOW / statement of work, prepare a letter of agreement, send terms, or price an engagement."
argument-hint: "<lead or client> [engagement shape] [start date]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-proposal - the document that wins the deal and sets the scope

Produce the conversion artifact between `/fcxo-qualify` (the verdict) and
`/fcxo-new-client` (the engagement): a proposal, SOW, or short letter of agreement the
buyer can say yes to. It has two jobs at once - win the deal, and draw the scope line
that prevents scope creep for the life of the engagement. Keep it short: senior buyers
skim, and a 2-page proposal beats a 10-page one.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` for the workspace layout
and conventions. The workspace root is wherever the folder holding `me/` is - use it as
found, never assume a fixed path prefix.

## Input

`$ARGUMENTS`: a lead name or slug, an existing client name, or a short description of
the deal. Optionally the engagement shape and a start date. If empty, ask which lead or
client this proposal is for.

## Step 1 - Read the context (before asking anything)

Pull everything the workspace already knows:

- **Role** - `me/*- Profile.md` (`role:` frontmatter). Sets the lens and vocabulary of
  the proposal: a CFO proposal talks cash discipline and reporting, a CMO proposal talks
  pipeline and brand, a CTO proposal talks delivery and architecture. Frame in the
  role's terms without asserting domain specifics the user has not supplied.
- **Offers and rates** - `me/*- Offers.md`. This is the **price anchor**. Every number
  in the investment section comes from here or from the user in this conversation.
  Never invent pricing, a discount, or a tier that is not recorded.
- **Positioning** - `me/*- Positioning.md`. The only source for how the user describes
  their value. Use those words; never fabricate positioning (rule in
  `${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md`).
- **The lead** - `leads/*- Lead.md` for the named lead: the qualification verdict, the
  buyer's pains in their own words, and the engagement shape `/fcxo-qualify`
  recommended. This file is the raw material for the situation and scope sections.
- **Existing-client expansion** - when the proposal extends a current relationship,
  read `clients/<slug>/<Client> - Profile.md` and the prior engagement files instead:
  what was delivered, what worked, what the new scope builds on.

Never re-ask what a file already answers. With **no workspace**, run standalone: ask
for the role, what they sell and at what price, the buyer's problem, and the intended
scope, then return the proposal in chat.

## Step 2 - Fill genuine gaps (one short batched question)

If real gaps remain after reading - typically the start date, the term or duration,
invoicing preferences, or a scope detail the lead file lacks - ask them **once, as a
single batched question**. Do not interview the user section by section.

## Step 3 - Draft the proposal

Write clean markdown in this structure, aiming at roughly two pages:

1. **Their situation** - the problem as the buyer experiences it, in the buyer's
   language taken from the lead file ("releases keep slipping", "we can't see our
   cash position"), never expert categories. Two or three sentences that show the
   user understood; no restated company history.
2. **Proposed engagement** - what the user will do about it: the scope, the concrete
   deliverables, the working cadence (days, calls, on-site), and a short **explicitly
   out of scope** list. The out-of-scope list is the scope-creep guard - make it
   specific to this deal, not boilerplate.
3. **Timeline and start** - when work begins, key milestones if any, and the term.
4. **Investment** - the price, anchored to `me/*- Offers.md`. Recommend **one**
   option. Add a small tier table only when the user's offers genuinely have tiers;
   never manufacture options to look flexible.
5. **Terms** - invoicing cadence, payment terms, notice period, and the assumptions
   the price depends on (access, tooling, response times).
6. **Next step** - the single action that accepts the proposal (reply to confirm,
   sign the attached agreement, book the kickoff call). One step, not a menu.

Apply `${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md` throughout: plain
language, short sentences, positive statements of what IS, position from strength, no
em-dash, no AI-slop vocabulary. A **social proof** line (a relevant result or case) is
allowed only when the workspace holds real, anonymized material for it - for example a
case study in `content/` or a documented outcome in a prior engagement file. If none
exists, omit the slot entirely; never invent or embellish proof.

## Step 4 - Save and link

- **Lead stage**: save to `leads/<YYYY-MM-DD> - <Lead> Proposal.md` with frontmatter
  `type: proposal`, `status: draft`, `updated: <date>`. Per the wiki convention in
  `practice-workspace.md`, the proposal links `[[<Lead> - Lead]]` and
  `[[<Owner> - Offers]]`. Then update the lead file: a dated note that a proposal was
  drafted, plus a `[[wikilink]]` to the proposal file. When the user says they sent
  it, update both files' `status` to reflect that the proposal is out.
- **Existing client**: save to
  `clients/<slug>/documents/<YYYY-MM-DD> - <Client> Proposal.md`, linked to
  `[[<Client> - Profile]]` (and the prior engagement file when the proposal extends
  it).
- **No workspace**: skip the write and return the proposal in chat, offering to save
  it once a workspace exists.

## Draft only, and where this hands off

The user sends the proposal; this skill never sends anything. From here:

- **Branded document** - when the user wants a client-facing, branded version, render
  the finished proposal with `/fcxo-report`. This skill owns the content, not the
  rendering.
- **On acceptance** - run `/fcxo-new-client` to create the client and engagement; the
  proposal's scope, price, and cadence carry straight into the engagement file.
- **Later** - the proposal's scope and out-of-scope sections are what
  `/fcxo-status-update` checks against when it watches for scope creep. Write them as
  if they will be quoted back in month three, because they will be.

## Quality gate (before delivering)

- Every price and rate traces to `me/*- Offers.md` or the user's words in this
  conversation; nothing invented.
- The situation section is in the buyer's language and could only describe this buyer.
- The out-of-scope list exists and is specific to this deal.
- One recommended option, one next step; a tier table only if real tiers exist.
- Any social proof is real, anonymized workspace material; the slot is omitted otherwise.
- The whole proposal fits roughly two pages, and no em-dash or AI-slop tell survives.
