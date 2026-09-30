# devangs-plugins

Personal Claude Code plugin. One plugin, many skills — new ones go in `skills/`.

## Skills

### `/scope`

Walks a piece of engineering work from an ambiguous ask to a scoping doc someone can actually plan against: boundary, prior art, risks, probes, estimate, kill criteria, re-forecast trigger.

It is **deliberately not autonomous**. It runs in short phases and stops at a checkpoint after each one. Unattended exploration produces confident, wrong scoping docs, which are worse than no doc — because they get planned against.

Two rules do most of the work:

- **Never guess at anything organizational.** Who owns a service, how slow review is, whether code is dead, who else is in there right now — none of that is in the repo. It asks.
- **Challenge "easy."** Estimates given before exploration run 5–8x low. If you call something a quick tweak, it says so.

Invoke it again in **re-forecast mode** when work passes its trigger: three sentences — what you got wrong, the new number, what changed your mind. Then stop, with no softener on the end.

### `/shipping-checklist`

Pre-launch follow-through: the things that get skipped when you're rushing to ship. Tests including error paths, feature flag and rollout, measurement, monitoring and rollback, security, user and partner-team communication, changelog, docs, cleanup, and what's likely to break a month from now.

It checks what it can from the diff itself, asks about everything else in one grouped round, and won't call anything **Ready** while an item is still open. It never flips flags, merges, deploys, or sends messages — it checks and drafts.

## Install

```bash
claude plugin marketplace add devangjhabakh/devangs-plugins
```

Or point Claude Code at a local checkout.

## Layout

```
.claude-plugin/plugin.json
skills/
  scope/
    SKILL.md
    references/
      risk-lenses.md      # the lenses, most-skipped first
      doc-template.md     # output shape
      calibration.md      # personal measured multipliers — private
  shipping-checklist/
    SKILL.md
```

## Adding a skill

Create `skills/<name>/SKILL.md` with frontmatter (`name`, `description`, `user-invocable`, `allowed-tools`). Supporting files go in `skills/<name>/references/`. Nothing else needs changing.
