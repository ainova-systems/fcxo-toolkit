# Data handling

How the FCxO Toolkit handles your data. Short version: your files stay yours, the plugin
sends nothing to us, and no message ever reaches another person unless you send it
yourself. One skill writes to a calendar you have already connected, as a hold on your
own calendar with no guests. The detail is below.

## What this plugin is

The toolkit is **instruction-only**: every skill is a plain-markdown `SKILL.md` that
Claude reads and follows. There is no executable code, no scripts, no hooks, no MCP
servers, no background processes, and no build step. You can read everything the
plugin does - it is all in this repository.

## What it reads and writes

- Skills read and write **markdown and HTML files inside your own practice workspace
  folder** (a local folder you create and own). Your workspace is the only place the
  toolkit stores anything of yours.
- **One skill writes outside it, and what it writes is a calendar entry.**
  `fcxo-book-call` creates a hold on your own calendar, through a calendar you already
  connected in your agent surface. The hold carries **no guests**. Adding a guest would
  make the calendar service email that person an invitation you never sent, so no skill
  ever adds one, and no skill creates or modifies an event that already has attendees.
  If the event you asked about has guests, the skill stops and says why, and you change
  it yourself. The reply proposing the slot is a draft in your workspace, and you send it.
- Documents (invoices, reports) are generated as **self-contained local HTML files**;
  no external assets are fetched at render time and nothing is uploaded.
- Billing details you choose to record (in your profile or `design/DESIGN.md`) stay
  in your local files. Skills read them to fill your own documents and never transmit
  them.

## Network access

The plugin ships **no APIs, no credentials, and no keys of its own**. It has no
telemetry, no tracking, and no external storage, and it sends **no data to the author**.

Network activity comes from two places, and both already belong to you.

**Claude's built-in web tools** (`WebSearch` / `WebFetch`), which these skills use to
research the public web:

- `fcxo-prospect-research`, `fcxo-qualify`, `fcxo-call-prep` - research a prospect or
  company you name.
- `fcxo-outreach` - find one real, specific hook before drafting.
- `fcxo-decision-report` - verify facts a memo relies on.
- `fcxo-learn-skill` - study how a task is done elsewhere (may also use Claude's
  subagents for parallel research).
- `fcxo-brand` - fetch a website or document you point it at, to derive brand colors.

Search queries derive from what you ask for (for example, a prospect's company name).
Workspace files are not uploaded by these tools.

**Connectors you have already installed** in your agent surface - the ones built into
Claude Cowork, or added from its marketplace. The toolkit ships none of them and asks you
for no credential; it uses the ones you connected, and `Practice Workspace -
Integrations.md` in your workspace records which tool serves which job. A connected tool
is a third party, and data does reach it:

- **Calendar** (`fcxo-book-call`) - the skill reads your availability and creates one
  guest-free hold. The event's title, time, and description are stored by your calendar
  provider, exactly as if you had typed the event in yourself. The hold has no guests, so
  the provider emails nobody.
- **Email, and every other connected tool** - read-only. A skill reads what you point it
  at and drafts into your workspace.

Your calendar and email providers hold that data under their own terms, and your agent
surface governs it under the permissions you granted the connector. The plugin adds no
processing or storage on top.

## Outward communication is draft-only

No skill sends anything. Emails, DMs, outreach, posts, proposals, and status updates
are always delivered as **drafts into your workspace** (or chat) for you to review and
send yourself. The Gmail-style "send" action does not exist in this toolkit.

**A calendar invitation is an outward message.** When an event has a guest, the calendar
service emails that person on your behalf, which makes an invitation the likeliest way a
toolkit like this could ever message a prospect by accident. The rule that closes it is
absolute: a hold goes on your own calendar with no guests, and no skill creates or
modifies an event that already has attendees. If the event has guests, the skill stops,
says why, and leaves the change to you. You send the real invitation once the time is
agreed.

## Model processing

Content that skills read from your workspace is processed by Claude as part of your
own conversation, under the data terms of **your** Claude plan. The plugin adds no
processing, retention, or storage of its own on top of that. If you handle several
clients' confidential material, keep each client in its own folder and scope what a
task reads to the folder it needs.

## Contact

Questions about this policy: info@ainovasystems.com (Ainova Systems).
