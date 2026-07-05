---
name: fcxo-reply
description: "Draft one reply inside an existing client thread, using only the logged thread as context and mirroring its tone and language. Draft only - you send it. Use when the user wants to reply to a client, answer an email in a thread, or continue a conversation."
argument-hint: "[client or thread + anything you want said]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-reply – one fluent reply, inside the thread

Draft a single reply that continues an existing conversation, grounded only in the
thread on disk. Draft only: the user reviews and sends it.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` for the workspace layout,
the wiki convention, and the draft-only rule.

## Load the thread

- `Glob clients/<slug>/communications/*`, find the thread, and **read every prior
  message in it** (logged by `fcxo-log-message`). Tone, open questions, and who said
  what come from the actual thread, not a reconstruction.
- Identify the **active correspondent** you are answering. The reply belongs to their
  thread; it does not become a separate message to a third party who is not in it.

## Write as a continuing participant

- **Mirror the thread**: greeting register (formal vs informal), tone, length (at most
  the incoming message's length), punctuation, and **language** (reply in the language
  the correspondent used).
- **Cadence**: in a rapid same-day exchange, open straight on the content with no fresh
  greeting or sign-off. Use a full greeting only for the first message of a thread or a
  genuine new contact.
- Assume shared context: do not restate what was just said or re-introduce yourself. Add
  only the new content.
- Strip filler ("just wanted to", "hope this finds you well") and uninvited closing
  prompts unless the correspondent used them.

## Save it (draft only)

- Append the reply into the **same thread file** under a dated "Draft reply" entry,
  verbatim in a fenced block, marked as a draft. Never send.
- Keep the thread's wikilinks intact (`[[<Client> - Profile]]`, the engagement).

## Hard rules

- **Draft, never send.** The user is the send mechanism.
- **One reply, one thread.** Answer the active correspondent inside their thread; if you
  need something from a third party, ask the correspondent for it inside the reply.
- **Standalone fallback**: if there is no thread file, ask the user to paste the message
  they are answering, draft from that, and offer to log it with `fcxo-log-message`.

## Notes

- Log incoming and sent messages first with `fcxo-log-message` so this skill has a
  thread to read.
