---
name: fcxo-post
description: "Draft or finalize a short professional post in the fractional executive's own voice, enforcing a voice and style guide that learns from feedback. Use when the user wants to write a post, draft social content, finalize a post, or work on their personal brand."
argument-hint: "[draft <topic> | finalize <draft path> | channel]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-post – a post worth reading, in your voice

Write a short professional post that hands the reader one real, usable idea and sounds
like the user, not like AI. Two modes: **draft** (a topic -> a post) and **finalize**
(an existing draft -> polished, voice-enforced, ready to publish). Default channel is
LinkedIn (where fractional executives are), but the post works for any channel; take it
from `$ARGUMENTS` or ask.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` for the workspace layout
and the draft-only rule.

## The voice guide is the source of truth

Every post obeys one file: `me/<Owner> - Voice.md` under the workspace root - the user's
voice and post style guide, the content analogue of `me/brand/DESIGN.md`. It holds the voice, the
format rules, the do's and don'ts, and a **Learned from feedback** section that grows
every time the user corrects a post. Resolve it before writing:

1. **Exists and populated** -> use it. Apply its voice and rules to this post.
2. **Missing or thin** -> build it from what the project already knows: read
   `me/*- Positioning.md` (how the user describes what they do) and any existing posts in
   `content/` (learn the real voice from what they have actually written). Draft an initial
   `me/<Owner> - Voice.md` from those signals (seed it from
   `${CLAUDE_PLUGIN_ROOT}/references/post-voice.default.md`) and show it for a quick confirm.
3. **Still unclear** (no positioning, no prior content) -> offer `/fcxo-brainstorm` scoped
   to "your post voice" to interview the user and capture it, or ask them to paste 2-3 posts
   they like (their own, or a voice to emulate) and learn from those. Then seed
   `me/<Owner> - Voice.md`.

If no `Voice.md` can be built yet, fall back to `post-voice.default.md` and tell the user
once that running this skill starts building their own.

## Draft

- Take the topic from `$ARGUMENTS` (or ask). Ground the post in the user's real
  experience and context (their work, an anonymized client win, a lesson), never generic
  filler.
- Apply the voice guide: the voice, the structure (one connected thought with a real
  payoff), and the do's and don'ts (no AI-slop tells - the guide lists them).
- If the post derives from a specific client win or engagement, link it per the wiki
  convention (`[[<Client> - Profile]]`), keeping the client detail anonymized in the post.
- Save the draft to `content/<YYYY-MM-DD> - <Topic>.md` (frontmatter `type: post`,
  `status: draft`). Draft only; the user publishes.

## Finalize

- Read the draft (from `$ARGUMENTS` or the latest in `content/`). Run it against the
  voice guide as a hard gate: enforce the voice, cut every AI-slop tell, confirm one
  connected thought and a real payoff, fix anything off-voice.
- Show the substantive changes you made so the user sees what was enforced.
- Update the file to `status: final`. Still draft-only for publishing.

## Self-learning (capture feedback into the voice guide)

This is what makes the skill improve with use. After you draft or finalize, the user
often reacts ("I don't open with a question", "too salesy", "always end on a concrete
detail", "drop the emoji"). When the feedback is a **durable rule** - something that
should hold for every future post, not just a fix to this one - **append it to the
"Learned from feedback" section of `me/<Owner> - Voice.md`** as a short, dated, plain
rule, so the next post applies it automatically.

- Durable -> write it to `Voice.md`. A one-off edit to just this post -> apply it here,
  do not touch `Voice.md`.
- When unsure if feedback is durable, ask one short question ("make this a standing rule
  for all your posts?") before writing it.
- Never delete the user's existing rules; add to them. The guide is the memory.

## Hard rules

- **Draft, never publish.** The skill prepares the post; the user posts it.
- **Voice from the user, never invented.** If the voice guide and positioning are both
  empty, ask or learn from examples; do not fabricate a persona.
- **Anonymize** any client or private detail in a public post.
- Shell-free; wiki-link by basename.

## Notes

- Channel-aware: LinkedIn is the default; pass another channel and the same voice applies
  with channel-appropriate length.
- `fcxo-brainstorm` builds the voice when it is not yet captured; `fcxo-case-study` (later)
  can feed this skill anonymized win stories.
