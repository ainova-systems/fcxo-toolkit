---
name: fcxo-qualify
description: "Qualify a lead for a fractional executive: assess fit, urgency, and budget, recommend an engagement shape, and give a go/no-go. Use when the user wants to qualify a lead or decide whether to pursue a client."
argument-hint: "<lead slug, profile text, or URL>"
allowed-tools: Read, Write, Edit, Glob, WebFetch, WebSearch
---

# fcxo-qualify – fit, urgency, budget, and a clear verdict

Decide whether a lead is worth the user's time and, if so, what to propose. The user's
time is the scarce resource – protect it with an honest go/no-go.

Read `${CLAUDE_PLUGIN_ROOT}/references/practice-workspace.md`. Pull the user's role
from `me/*- Profile.md` and their offers/tiers from `me/*- Offers.md` if present; otherwise
ask for a one-line summary of what they sell and at what price band.

Score through the role's lens: the same lead can be a strong fit for one role and weak
for another. A CMO weighs growth and brand pain, a CTO weighs scaling and delivery pain,
a CFO weighs financial control. Judge fit against what this role actually solves.

## Input

`$ARGUMENTS`: an existing `leads/*- Lead.md`, pasted profile text, or a URL. If empty,
ask for the lead. Read any existing lead file or research the target briefly to fill gaps.

## Qualify on four axes

Score each plainly (strong / mixed / weak) with a one-line reason:

1. **Fit** – does the user's role and offer actually solve a problem they have?
2. **Urgency** – is there a trigger making this live now, or is it "someday"?
3. **Budget signal** – stage, size, and spend patterns that suggest they can pay the
   user's rate. Be realistic; a small budget framed as a big problem is a no.
4. **Access** – can the user reach the decision-maker, or are they several layers down?

## Recommend

- **Engagement shape** – map the lead to one of the user's offers/tiers (or the
  nearest sensible shape if offers aren't recorded). State the entry point and the
  realistic path to a paid engagement.
- **Go / no-go** – a clear verdict. If no-go, say why in one line and stop; do not
  create a pipeline file for a lead with no realistic paid path.
- **Next step** – the single most useful action (`/fcxo-outreach`, a specific question
  to confirm budget, a referral path, or drop it).

## Save

For a **go**: save or update `leads/<Lead> - Lead.md` with the verdict and recommended
shape, frontmatter `type: lead`, `status: new`, `updated: <date>`. Per the wiki
convention in `practice-workspace.md`, the lead file links the referrer when known
(`[[<Client> - Profile]]` or the person who referred it) and `[[<Owner> - Offers]]` for
the recommended engagement shape. For a **no-go**: report in chat, no file.

## Notes

- Honesty over optimism. A fast no-go is more valuable than a hopeful maybe.
- Keep the verdict skimmable – the user should grasp it in ten seconds.
