---
name: fcxo-pipeline-review
description: "Weekly pipeline and client-portfolio review for a fractional executive: prioritize leads, flag risks, and produce a short action plan. Use when the user wants to review their pipeline or plan their week."
argument-hint: "[week or focus]"
allowed-tools: Read, Write, Glob
---

# fcxo-pipeline-review – what to chase, what to protect, what to drop

Give the user a clear, prioritized picture of their week across new business and active
clients. A solo fractional has no sales team; this is the standup with themselves.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md`. Gather state from the
workspace: every lead record (`leads/**/*- Lead.md`) and every active engagement under
`clients/*/engagements/**/*- Engagement.md`. If no workspace, ask the user to list their
leads and active clients.

## Read the state

- **Leads** – for each, note status, fit/verdict (from `/fcxo-qualify` if present),
  last touch, and the next step on record. Each lead sits in its own folder, so the real
  state is right there beside the record: the newest file in `leads/<slug>/communications/`
  dates the last touch, `meetings/` shows whether a call has happened, and anything in
  `leads/<slug>/proposals/` means a number is out and the deal is live. A lead with a
  sent proposal and a month of silence is a different problem from a lead that was never
  called; the folder tells you which, so the review can say so.
- **Active engagements** – status, what's due, and any risk signals (a stalled
  deliverable, a renewal coming up, a quiet client).

## Synthesize

Produce a short, prioritized review:

1. **This week's focus** – the 3-5 actions that matter most, ranked. Each names the
   lead/client and the specific next step. Prefer moves that advance a real opportunity
   or protect revenue over busywork.
2. **At risk** – leads going cold, engagements drifting, renewals to secure. Flag each
   with the recovery action.
3. **Pipeline health** – a quick read: enough live opportunities or time to prospect?
   Concentration risk (too much riding on one client)?
4. **Drop list** – leads with no realistic path; name them so the user can let go.

## Output and save

Return the review in chat, skimmable. If a workspace exists, optionally save a dated
copy to `research/` (e.g. `research/<YYYY-MM-DD> - Pipeline Review.md`) so the user can
compare week over week. This dated review is the week's hub: per the wiki convention in
`practice-workspace.md`, write a `[[wikilink]]` to every client, engagement, and lead it
names (`[[<Client> - Profile]]`, `[[<Client> - <Descriptor> Engagement]]`,
`[[<Lead> - Lead]]`) so the graph connects the week to each item it touches.

## Notes

- Be honest about cold leads and over-concentration – the value is in the hard calls.
- Keep it to one screen. A review the user won't read helps no one.
- This is a read-and-advise skill; it doesn't change lead or engagement files unless
  the user asks.
