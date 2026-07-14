---
name: fcxo-new-client
description: "Add a new client or engagement to a fractional executive's practice workspace. Use when the user wants to add a client, onboard one, or start a new engagement, project, or retainer."
argument-hint: "<client name> [engagement id or description]"
allowed-tools: Read, Write, Edit, Glob
---

# fcxo-new-client – add a client or engagement

Add a client (and optionally a first engagement) to an existing practice workspace.
For first-time setup of the workspace itself, use `/fcxo-init`.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md` first for the layout.

## Step 1 – Locate the workspace

Find `me/*- Profile.md` in or under the current folder. If there is no workspace, tell
the user to run `/fcxo-init` first (or point this skill at their workspace path). Stop
if none is found.

## Step 2 – Parse the request

From `$ARGUMENTS`: the client name (required) and, if given, an engagement
id/description. Derive a kebab-case `<slug>` from the client name.

- If the client folder `clients/<slug>/` already exists, do not recreate it – offer to
  add a **new engagement** to that existing client instead.
- If only a client name is given, ask once (batched) whether to also create a first
  engagement now, and for the engagement basics (see Step 4). If the user says
  "just the client", create the client and stop after Step 3.

## Step 3 – Create the client

Create `clients/<slug>/` with:

- `<Client> - Profile.md` – from the `Client - Profile.md` template if present (copied
  in and renamed to the client name). Capture what the user knows: company, industry,
  key contacts, how the relationship started, any links. Light frontmatter
  `type: client`, `status: active`. Per the wiki convention in
  `practice-workspace.md`, write a `[[wikilink]]` to each engagement this client has
  (e.g. `[[<Client> - <Descriptor> Engagement]]`) once Step 4 creates one.
- `communications/` and `documents/` folders. Create each by writing its first real
  file into it (writing a file creates the folder; no shell needed). Do **not** add a
  per-folder `README.md` – generic basenames collide when the workspace opens in
  Obsidian. If there is no content yet, leave the folder out until there is.

Do not invent facts. Use `TODO:` for anything the user did not provide.

## Step 4 – Create the engagement (if requested)

Pick an engagement id: a short kebab slug the user gives, or derive one (e.g.
`retainer-2026`, `q3-assessment`). Create `clients/<slug>/engagements/<id>/` with:

- `<Client> - <Descriptor> Engagement.md` (e.g. `Orlend Labs - Retainer
  Engagement.md`) – from the `Engagement.md` template, copied in and renamed to this
  self-identifying name. Record **scope, price, rate, status, start date, billing
  cadence**. These fields are read later by `/fcxo-invoice`, `/fcxo-decision-report`,
  and `/fcxo-pipeline-review` (which discover the file by `type: engagement` and the
  `- Engagement.md` suffix), so capture them even if rough. Light frontmatter
  `type: engagement`, `status: active`. Wire the relationship both ways (wiki
  convention in `practice-workspace.md`): this engagement file links back to
  `[[<Client> - Profile]]`, and the client profile links to this engagement.
- `deliverables/`, `meetings/`, `notes/` folders. Create each by writing its first real
  file into it (no shell needed); do **not** add a per-folder `README.md`, and leave a
  folder out until it has content.

## Step 5 – Confirm

State what was created and where, in a few sentences. Suggest next steps that fit the
engagement: `/fcxo-call-summary` after the kickoff, `/fcxo-decision-report` for the
first deliverable, or `/fcxo-invoice` when it is time to bill.

## Notes

- If the user already has a convention for client folders, match it.
- Light frontmatter only; never enforce it.
