---
name: shipping-checklist
description: Pre-launch checklist for shipping a change — tests including error paths, feature flag and rollout, measurement, monitoring and rollback, security, user and partner-team communication, changelog, docs, cleanup, and what's likely to break later. Verifies what it can from the repo and asks about the rest; never assumes an item is done. Use before merging, launching, enabling or widening a feature flag, or calling something shipped.
user-invocable: true
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - Write
---

# /shipping-checklist

Catches the follow-through that gets skipped when you're rushing to ship: the things a manager otherwise ends up chasing after the fact. Is the flag on? Did users hear about it? Is it in the changelog? Did anyone think about security?

## Operating rules

1. **Never assume an item is done.** Every item ends in exactly one state:
   - **Verified**, with evidence: `file:line`, command output, or a link
   - **Confirmed by user**, meaning they said so
   - **N/A**, with a reason
   - **Open**
2. **Check what you can yourself before asking.** Don't make the user answer something the diff already shows.
3. **Ask everything else in one grouped round,** not one item at a time. This runs at ship time, and a slow checklist gets skipped. Follow up only on answers that are unclear.
4. **Never take launch actions.** Don't flip flags, merge, deploy, or send messages. Check and draft only.
5. **No "Ready" while anything is Open.** "Ready with named risks" is allowed only if the user explicitly accepts each risk by name.

## Step 1 — What's shipping

Find the change: branch, PR, or diff against the base (`git diff`, `git log`). Summarize in two or three lines what changes for users.

→ **Checkpoint:** confirm this is the right scope, and ask what is deliberately *not* in this launch.

## Step 2 — Check what's checkable

Go through the checklist below and verify from the repo wherever possible:
- Tests added or changed in the diff. Run them if the command is obvious; **ask first** if it's slow or has side effects.
- Feature flag references in the diff.
- Changelog touched?
- New endpoints, permissions, secrets, external URLs, or rendered user content.
- Analytics events added or changed.
- Migrations present, and whether they're reversible.
- TODO / FIXME / debug code introduced.

## Step 3 — One round of questions

Show a table of what you verified, with evidence. Then a numbered list of everything still Open, grouped by section.

→ **Checkpoint. Wait.**

## Step 4 — Verdict and drafts

**Verdict:** Ready · Ready with named risks · Not ready (list the blockers).

Offer drafts, and never send them:
- Changelog entry
- Note to any partner product team whose surface this touches
- User-facing announcement, if one is warranted
- Ticket to remove the flag and temp code, with a date

## The checklist

### Correctness
- [ ] Tests cover the new behavior
- [ ] **Tests cover the error paths:** partial failure, timeouts, retries, cancellation, empty or invalid input. The happy path alone doesn't count.
- [ ] Tests were actually run, and pass
- [ ] Tried at realistic volume where it matters. **What happens at 10x?**

### Flag and rollout
- [ ] Behind a flag, with a deliberate default state
- [ ] Who gets it first, what has to be true to widen it, and who decides
- [ ] Kill switch works: turning the flag off really restores the old behavior

### Measurement
- [ ] Events and metrics fire correctly, **checked in real data**, not just read in code
- [ ] Success can't be recorded before the user actually got value
- [ ] For experiments: primary metric, guardrail metrics, and what result means kill it

### Monitoring and rollback
- [ ] What tells you it broke, and who's watching for the first 24–48 hours
- [ ] How to roll back, whether that's been tried, and whether data changes are reversible

### Security and privacy
- [ ] Auth and permissions on anything new
- [ ] Secrets stored properly, and kept out of logs and API responses
- [ ] User-supplied or generated content rendered safely
- [ ] PII: what's collected and where it goes
- [ ] Any new external surface: webhooks, public URLs, third-party calls

### Communication
- [ ] Changelog entry
- [ ] Users notified, if they should be
- [ ] **Partner product teams whose surfaces this touches know it's coming**
- [ ] Support, sales, or docs briefed if customers will ask
- [ ] PM and design have seen the final version

### Handoff and cleanup
- [ ] Docs or runbook for whoever touches this next
- [ ] Ticket to remove the flag and temp code, with a date
- [ ] No stray TODOs or debug code

### What breaks later
- [ ] What's the most likely bug report a month from now, and what would you check first?
- [ ] Is this a fix for one instance of a broader class of problem? If it's the third bug with this shape, say so and name the class.
