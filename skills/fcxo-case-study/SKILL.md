---
name: fcxo-case-study
description: "Turn a finished or milestone-passing engagement into an anonymized, reusable win story that feeds proposals and posts. Use when the user wants a case study, a win story, to write up an engagement, a success story, or to turn this client work into marketing material."
argument-hint: "<client or engagement> [milestone]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-case-study - the engagement becomes proof

Close the loop from retention to acquisition: take an engagement that finished (or
passed a real milestone) and write it up as a win story the practice can reuse. The
result feeds two places - the social proof slot in `/fcxo-proposal`, and `/fcxo-post`
for a public post. A documented, anonymized case is the strongest asset a fractional
executive owns; this skill builds it from the record, not from memory.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` for the workspace layout
and conventions. The workspace root is wherever the folder holding `me/` is - use it as
found, never assume a fixed path prefix.

## Input

`$ARGUMENTS`: a client name, slug, or engagement, optionally the milestone the story
covers. If empty, ask which engagement this case study is about.

## Step 1 - Read the record (before asking anything)

The evidence lives in the workspace; pull it before interviewing the user:

- **Role** - `me/*- Profile.md` (`role:` frontmatter). Sets the lens: a CFO case is a
  cash and control story, a CMO case a pipeline and brand story, a CTO case a delivery
  and architecture story. Frame in the role's terms without asserting domain specifics
  the record does not support.
- **The engagement** - `clients/<slug>/engagements/<id>/*- Engagement.md`: the scope,
  price shape, duration, and what "done" meant.
- **Deliverables** - `clients/<slug>/engagements/<id>/deliverables/`: what was actually
  produced. Artifacts are the proof backbone.
- **Status updates and the renewal review** - if the engagement has status updates or a
  renewal / QBR review, read them; the results-versus-plan evidence usually lives there.
- **Call summaries** - `clients/<slug>/engagements/<id>/meetings/`: turning points,
  decisions, and anything the client said in their own words.
- **Client profile** - `clients/<slug>/<Client> - Profile.md`: industry, size, stage,
  for the anonymized descriptor.

Then ask the user **once, in a single batched question**, only for what the record
lacks - most importantly the **measurable outcome** (the number or observable change
the story rests on) and **the client's own words**, if any exist. With **no
workspace**, run standalone: ask for the role, the engagement in brief, the outcome,
and the client's industry and size, then return the case study in chat.

## Anonymization - the hard rule

This rule is absolute for the public-facing text:

- **The client is never named by default.** Refer to them by an honest descriptor:
  industry plus size or stage, for example "a 40-person B2B SaaS company" or "a
  family-owned manufacturer entering e-commerce". Honest means the descriptor is true
  as written; never upgrade the size, stage, or industry to sound bigger.
- **Naming the client requires explicit permission.** Only name them if the user
  confirms in this conversation that they have the client's explicit permission, and
  record that confirmation in the file's internal notes (who confirmed, when).
- **Strip indirect identifiers.** A unique product name, an exact revenue figure, a
  location-plus-niche combination can identify the client as surely as the name. Round
  or generalize any detail that would let a reader work out who it is - and say so
  honestly ("roughly", "about") rather than presenting a blurred number as exact.

## Structure - evidence over adjectives

Apply `${CLAUDE_PLUGIN_ROOT}/references/copy-principles.md` throughout: plain language,
positive statements of what IS, position from strength, artifact-based proof over
time-claims, no em-dash, no AI-slop vocabulary. The story is four beats:

1. **Situation** - the client's felt problem when the user arrived, in buyer language
   ("releases kept slipping", "no one could see the cash position"), not expert
   categories. Two or three sentences.
2. **What the user did** - the decisions and interventions, told plainly in the role's
   lens. Concrete actions and calls made, no adjectives doing the work of facts.
3. **Outcome** - measurable where the record supports it. Every number traces to a
   workspace file or the user's words in this conversation; **never fabricate or round
   up**. Where the record holds no number, state the observable change instead and keep
   it honest - an artifact shipped, a decision that held, a renewal signed.
4. **What this proves** - one sentence, stated positively, on what the engagement shows
   about the user's practice.

## Two artifacts in one file

The output file carries both versions of the story:

- **The full case study** - proposal-ready, half a page to a page. This is what
  `/fcxo-proposal` cites in its social proof slot.
- **The short version** - 3 to 6 sentences carrying the same situation, action, and
  outcome. This is the seed `/fcxo-post` turns into a post; mention that hand-off to
  the user when delivering.

## Save and link

Save to `content/<YYYY-MM-DD> - <Descriptor> Case Study.md`, where `<Descriptor>` is
the anonymized industry descriptor (for example
`2026-07-05 - B2B SaaS Case Study.md`), never the client name - the filename itself
must leak nothing. Frontmatter: `type: case-study`, `status: draft`,
`updated: <date>`.

Structure the file in two clearly marked parts:

- **Public text** (the full case study and the short version) - fully anonymized and
  **link-free**: no wikilinks, no client name, nothing that must be stripped before
  pasting into a proposal or post.
- **Internal notes** (workspace-private, marked "internal only - not for publication") -
  wikilinks to `[[<Client> - Profile]]` and the engagement file per the wiki
  convention, the source files each outcome traces to, and the recorded naming
  permission if the client is named.

With no workspace, skip the write and return both versions in chat, offering to save
them once a workspace exists.

## Draft only

The user pastes the case study into proposals and publishes posts themselves; this
skill never sends or publishes anything.

## Quality gate (before delivering)

- The public text contains no client name (unless permission is confirmed and
  recorded) and no detail that identifies the client indirectly.
- The descriptor is honest: true industry, true size or stage, nothing upgraded.
- Every outcome traces to a workspace file or the user's words; internal notes cite
  the sources; nothing fabricated or rounded up.
- The public sections are link-free and paste-ready; wikilinks live only in the
  internal notes.
- Both versions exist: the full case study and the 3-6 sentence short version.
- No em-dash or AI-slop tell survives, and the closing sentence states positively what
  the engagement proves.
