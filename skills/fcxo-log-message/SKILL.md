---
name: fcxo-log-message
description: "Record an incoming or sent message from a client or a lead (email, DM, note) into its communications thread. Use when the user wants to log a message, save a client email, log an email from a prospect, record what a client or a lead said, file an inbound enquiry, or update a conversation thread."
argument-hint: "[client or lead + the message, or a paste of the email/DM]"
---

# fcxo-log-message – build the relationship timeline

Record an incoming or just-sent message into the company's `communications/` so the
thread becomes the durable relationship timeline every other skill can read. It files a
prospect's first email as readily as a client's, because a lead folder carries the same
`communications/` folder a client folder does. This skill never sends anything; it only
files what already happened.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` for the workspace layout,
the wiki convention, and the draft-only rule.

## Get the message

- **Pasted text** -> use it as given. **A pasted message is a complete instruction**: the
  message carries its own sender, date, and subject, and those three resolve the company
  and the thread on their own. Ask nothing further, and do not ask the user to confirm what
  the header already says.
- **The user points at a message they have not pasted** ("log the email from Harbor") ->
  `Read` `Practice Workspace - Integrations.md` and check the Email row. If a connected
  tool is registered there, pull the message from it **read-only** through the agent
  surface's own connector, and show the user what you fetched before filing it. If the row
  is empty, ask them to paste the message and carry on exactly as below.

Never name a vendor and never ask for a credential, an API key, or an OAuth flow. The
email connector is read-only: this skill reads a message and files it, and it sends
nothing.

## Resolve the company folder

`<company>` is `leads/<slug>/` before the contract and `clients/<slug>/` after it. The
destination sub-folder is `communications/` either way, so resolve the folder once and the
rest of this skill is identical for a lead and for a client.

- Identify the **company** the message belongs to. `Glob leads/**/*- Lead.md` and
  `Glob clients/*/*- Profile.md`, then match on, in order: the **email domain** of the
  sender against the record's `domain:` frontmatter and the addresses inside it, the
  **correspondent's name**, and the company name in the message. The domain is the strongest
  signal and usually settles it alone. If nothing matches, ask once, and offer
  `/fcxo-qualify` when it is a new prospect with no folder yet.
- Identify the **thread** (the subject or conversation the message continues). A `Re:`
  subject continues the thread that carries the same subject without it.
- `Glob <company>/communications/*` and read the candidates to find the existing thread
  file by subject and correspondent. **One thread = one file.**
- **No workspace** -> format the entry the same way, return it in chat, and offer to
  file it once a workspace exists.

## Record it (append, never fork)

- **Thread exists** -> `Edit` it: add the new message as a dated entry. Do not create a
  second file for the same conversation.
- **No thread yet** -> `Write` a new file
  `<company>/communications/<YYYY-MM-DD> - <From|To> <Name> - <Subject>.md` with light
  frontmatter (`type: communication`, `status:`, `updated:`), mirroring sibling files.

Each entry carries:

- a label line: sender + direction (incoming / sent) + timestamp;
- the **verbatim** message in a fenced block (do not paraphrase the source);
- one or two lines of commentary (what it means, what it asks);
- a **Next action** line if one is implied.

At the top of a new thread file, wire the relationship per the wiki convention: link the
company's record - `[[<Client> - Profile]]` for a client, `[[<Lead> - Lead]]` for a lead -
and, for a client, the relevant `[[<Client> - <Descriptor> Engagement]]`.

## Hard rules

- **Never send.** This skill files messages only; outward drafts are written by
  `fcxo-reply` / `fcxo-outreach` and the user sends them.
- **Verbatim in, commentary separate.** Keep the original text exact; your notes live in
  the commentary, outside the quoted block.
- Shell-free (file tools only); wiki-link by basename, never a path.

## Notes

- To draft an answer inside the thread you just logged, use `fcxo-reply`.
- A lead's threads travel with it: when the deal is won the folder moves to
  `clients/<slug>/`, and the whole correspondence moves inside it, so a client's timeline
  starts at the first prospect email.
- The logged thread feeds `fcxo-reply`, `fcxo-call-prep`, a weekly status update, renewal
  / QBR, and case studies later.
