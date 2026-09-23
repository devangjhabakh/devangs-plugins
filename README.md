# scoping

A Claude Code plugin with one skill: **`/scope`**.

It walks a piece of engineering work from an ambiguous ask to a scoping doc someone can actually plan against — boundary, prior art, risks, probes, estimate, kill criteria, re-forecast trigger.

## Why it's built this way

It is **deliberately not autonomous**. It runs in short phases and stops at a checkpoint after each one. Unattended exploration produces confident, wrong scoping docs, which are worse than no doc — they get planned against.

Two rules do most of the work:

- **Never guess at anything organizational.** Who owns a service, how slow review is, whether code is dead — none of that is in the repo. It asks.
- **Challenge "easy."** Estimates given before exploration run 5–8x low. If you call something a quick tweak, it says so.

## Install

```
/plugin marketplace add <user>/scoping-plugin
/plugin install scoping
```

Or point Claude Code at a local checkout.

## Use

```
/scope
```

Then answer the questions. It will interrupt often. That's the design.

Invoke it again in **re-forecast mode** when work passes its trigger — three sentences: what you got wrong, the new number, what changed your mind. Then stop. No softener on the end.

## Layout

```
.claude-plugin/plugin.json
skills/scope/
  SKILL.md
  references/
    risk-lenses.md      # the lenses, most-skipped first
    doc-template.md     # output shape
    calibration.md      # personal measured multipliers — private
```
