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

`$ARGUMENTS`: **a company name is the whole input.** `/fcxo-proposal Talvora Analytics`
is a complete instruction, and it is the normal one. The company's folder already holds
the deal - the qualification, the buyer's own words, the scope agreed on the call, the
start date, the price - so read it (Step 1) and write the proposal from it. Anything else
in the argument (an engagement shape, a start date, a correction) is an override on top of
the record, never a precondition for running. If the argument is empty, ask which company
this proposal is for; that is the only question a full workspace ever justifies here.

**This skill drafts. It never sends anything, under any wording of the request** - so
"do not send" never has to be said, and a "send the proposal" request produces a saved
draft plus a line telling the user it is theirs to send.

## Step 1 - Read the context (this is the job; do not ask for what the folder holds)

Pull everything the workspace already knows. **Read the company's folder in full** - every
file under it, not a sample - because that folder is the deal:

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
- **The company folder** - resolve it from the name in the argument: `Glob leads/**/*- Lead.md`
  and `Glob clients/*/*- Profile.md` and match on the company name, the folder slug, or the
  `domain:` frontmatter. `<company>` is `leads/<slug>/` before the contract and
  `clients/<slug>/` after it.
- **The record** - `<company>/<Lead> - Lead.md` (or `<Client> - Profile.md`): the
  qualification verdict, the buyer's pains, and the engagement shape `/fcxo-qualify`
  recommended.
- **The whole history** - `Glob <company>/communications/*` and `Glob <company>/meetings/*`
  and **read every file both globs return, oldest to newest**. The inbound messages and the
  call summaries hold the buyer's problem in the buyer's own words, which makes them the
  richest raw material for the situation section. Read them before writing a line of it,
  and quote the buyer's phrasing. Never paraphrase it into expert categories.
- **The newest message governs.** A deal moves, and the folder records it in order. Where a
  later message contradicts an earlier call summary or lead note - a scope requirement
  added, a next step withdrawn, a date fixed - **the later message wins, and the proposal
  reflects the deal as it stands today**. Carry a requirement that arrived this morning into
  the relevant section, and drop a next step the buyer has since cancelled. An action item
  in a three-day-old call summary is not a mandate.
- **Existing-client expansion** - when the proposal extends a current relationship, also
  read the engagement files under `clients/<slug>/engagements/**/*- Engagement.md` and the
  engagement's `meetings/`: what was delivered, what worked, what the new scope builds on.

Never re-ask what a file already answers. With **no workspace**, run standalone: ask
for the role, what they sell and at what price, the buyer's problem, and the intended
scope, then return the proposal in chat.

## Step 2 - Fill genuine gaps (rarely, and only one batched question)

A company folder that has been through qualification and a call answers everything a
proposal needs, so the normal number of questions is **zero**. Write the proposal and stop.

Ask only when a **load-bearing** fact exists in no file and cannot be derived from the
offers - the price for a shape the offers do not cover, or a start date nobody has ever
named. Then ask **once, as a single batched question**, and never interview the user
section by section. A detail that is merely nice to have is not a reason to ask: state the
assumption in the proposal's terms section and move on.

## Step 3 - Draft the proposal

Look for `templates/Proposal.md` under the workspace root yourself. When it is there, that
data template is the shape to write against: it lists the fields and sections a proposal
has in this practice, so follow its structure and fill it. The six sections below are the
default when no template exists.

Write clean markdown in this structure, aiming at roughly two pages:

1. **Their situation** - the problem as the buyer experiences it, in the buyer's
   language taken from the lead file ("releases keep slipping", "we can't see our
   cash position"), never expert categories. Two or three sentences that show the
   user understood; no restated company history.
2. **Proposed engagement** - what the user will do about it: the scope, the concrete
   deliverables, the working cadence (days, calls, on-site), and a short **explicitly
   out of scope** list. The out-of-scope list is the scope-creep guard - make it
   specific to this deal.
3. **Timeline and start** - when work begins, key milestones if any, and the term.
4. **Investment** - the price, anchored to `me/*- Offers.md`. Recommend **one**
   option. Add a small tier table only when the user's offers genuinely have tiers;
   never manufacture options to look flexible.
5. **Terms** - invoicing cadence, payment terms, notice period, and the assumptions
   the price depends on (access, tooling, response times).
6. **Next step** - the single action that accepts the proposal (reply to confirm,
   sign the attached agreement, book the kickoff call). Propose a single step.

Apply `${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md` throughout: plain
language, short sentences, positive statements of what IS, position from strength, no
em-dash, no AI-slop vocabulary. A **social proof** line (a relevant result or case) is
allowed only when the workspace holds real, anonymized material for it - for example a
case study in `content/` or a documented outcome in a prior engagement file. If none
exists, omit the slot entirely; never invent or embellish proof.

## Step 4 - Save and link

A proposal is a document, so it lives in the company's `proposals/` folder. One
destination covers both stages:

`<company>/proposals/<YYYY-MM-DD> - <Company> Proposal.md`

where `<company>` is `leads/<slug>/` before the contract and `clients/<slug>/` after it.
Never leave a proposal loose beside a lead record.

- **Frontmatter**: `type: proposal`, `status: draft`, `updated: <date>`.
- **Links**: the proposal links `[[<Owner> - Offers]]` plus its company -
  `[[<Lead> - Lead]]` at lead stage, `[[<Client> - Profile]]` for a client (and the
  prior engagement file when the proposal extends it).
- **Back-link**: update the lead or client record with a dated note that a proposal was
  drafted and a `[[wikilink]]` to the proposal file. When the user says they sent it,
  update both files' `status` to reflect that the proposal is out.
- `clients/<slug>/documents/` holds the **signed** contract or SOW, so a draft proposal
  never goes there.
- **No workspace**: skip the write and return the proposal in chat, offering to save
  it once a workspace exists.

## Draft only, and where this hands off

The user sends the proposal; this skill never sends anything. From here:

- **Branded document** - when the user wants a client-facing, branded version, run
  `/fcxo-report <Company> proposal`. It finds this markdown, fills `design/Proposal.html`,
  and writes the `.html` sibling next to it in the same `<company>/proposals/` folder,
  leaving the content as written. This skill owns the content; `/fcxo-report` owns the
  rendering.
- **On acceptance** - run `/fcxo-new-client` to create the client and engagement; the
  proposal's scope, price, and cadence carry straight into the engagement file.
- **Later** - the proposal's scope and out-of-scope sections are what
  `/fcxo-status-update` checks against when it watches for scope creep. Write them as
  if they will be quoted back in month three, because they will be.

## Quality gate (before delivering)

- Every price and rate traces to `me/*- Offers.md` or the user's words in this
  conversation; nothing invented.
- The proposal matches the **latest** state of the thread: every requirement the buyer
  added in their most recent message is in it, and nothing the buyer has withdrawn survives
  in it.
- Nothing was asked that the company folder already answers.
- The situation section is in the buyer's language and could only describe this buyer.
- The out-of-scope list exists and is specific to this deal.
- One recommended option, one next step; a tier table only if real tiers exist.
- Any social proof is real, anonymized workspace material; the slot is omitted otherwise.
- The whole proposal fits roughly two pages, and no em-dash or AI-slop tell survives.
