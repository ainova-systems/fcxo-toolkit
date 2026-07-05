---
name: fcxo-log-message
description: "Record an incoming or sent client message (email, DM, note) into its communications thread. Use when the user wants to log a message, save a client email, record what a client said, or update a conversation thread."
argument-hint: "[client + the message, or a paste of the email/DM]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-log-message – build the relationship timeline

Record an incoming or just-sent message into the client's `communications/` so the
thread becomes the durable relationship timeline every other skill can read. This skill
never sends anything; it only files what already happened.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` for the workspace layout,
the wiki convention, and the draft-only rule.

## Resolve the thread

- Identify the **client** (the `clients/<slug>/` it belongs to) and the **thread** (the
  subject or conversation the message continues). If unclear, ask once.
- `Glob clients/<slug>/communications/*` and read the candidates to find the existing
  thread file by subject and correspondent. **One thread = one file.**
- **No workspace** -> format the entry the same way, return it in chat, and offer to
  file it once a workspace exists.

## Record it (append, never fork)

- **Thread exists** -> `Edit` it: add the new message as a dated entry. Do not create a
  second file for the same conversation.
- **No thread yet** -> `Write` a new file
  `clients/<slug>/communications/<YYYY-MM-DD> - <Subject>.md` with light frontmatter
  (`type: communication`, `status:`, `updated:`), mirroring sibling files.

Each entry carries:

- a label line: sender + direction (incoming / sent) + timestamp;
- the **verbatim** message in a fenced block (do not paraphrase the source);
- one or two lines of commentary (what it means, what it asks);
- a **Next action** line if one is implied.

At the top of a new thread file, wire the relationship per the wiki convention: link
`[[<Client> - Profile]]` and the relevant `[[<Client> - <Descriptor> Engagement]]`.

## Hard rules

- **Never send.** This skill files messages only; outward drafts are written by
  `fcxo-reply` / `fcxo-outreach` and the user sends them.
- **Verbatim in, commentary separate.** Keep the original text exact; your notes live in
  the commentary, not inside the quote.
- Shell-free (file tools only); wiki-link by basename, never a path.

## Notes

- To draft an answer inside the thread you just logged, use `fcxo-reply`.
- The logged thread feeds `fcxo-reply`, a weekly status update, renewal / QBR, and case
  studies later.
