# Data handling

How the FCxO Toolkit handles your data. Short version: your files stay yours, nothing
is sent to us or to any third party, and no message ever leaves your machine without
you sending it yourself.

## What this plugin is

The toolkit is **instruction-only**: every skill is a plain-markdown `SKILL.md` that
Claude reads and follows. There is no executable code, no scripts, no hooks, no MCP
servers, no background processes, and no build step. You can read everything the
plugin does - it is all in this repository.

## What it reads and writes

- Skills read and write **markdown and HTML files inside your own practice workspace
  folder** (a local folder you create and own). Nothing is written anywhere else.
- Documents (invoices, reports) are generated as **self-contained local HTML files**;
  no external assets are fetched at render time and nothing is uploaded.
- Billing details you choose to record (in your profile or `me/brand/DESIGN.md`) stay
  in your local files. Skills read them to fill your own documents and never transmit
  them.

## Network access

The plugin calls **no APIs of its own** and sends **no data to the author or any
third party**. It has no telemetry, no tracking, and no external storage.

The only network activity is Claude's built-in web tools (`WebSearch` / `WebFetch`),
which these skills use to research the public web:

- `fcxo-prospect-research`, `fcxo-qualify`, `fcxo-call-prep` - research a prospect or
  company you name.
- `fcxo-outreach` - find one real, specific hook before drafting.
- `fcxo-decision-report` - verify facts a memo relies on.
- `fcxo-learn-skill` - study how a task is done elsewhere (may also use Claude's
  subagents for parallel research).
- `fcxo-brand` - fetch a website or document you point it at, to derive brand colors.

Search queries derive from what you ask for (for example, a prospect's company name).
Workspace files are not uploaded by these tools.

## Outward communication is draft-only

No skill sends anything. Emails, DMs, outreach, posts, proposals, and status updates
are always delivered as **drafts into your workspace** (or chat) for you to review and
send yourself. The Gmail-style "send" action does not exist in this toolkit.

## Model processing

Content that skills read from your workspace is processed by Claude as part of your
own conversation, under the data terms of **your** Claude plan. The plugin adds no
processing, retention, or storage of its own on top of that. If you handle several
clients' confidential material, keep each client in its own folder and scope what a
task reads to the folder it needs.

## Contact

Questions about this policy: info@ainovasystems.com (Ainova Systems).
