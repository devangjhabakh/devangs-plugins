---
name: scope
description: Scope a piece of engineering work interactively and produce a scoping doc — boundary, prior art, risks, probes, estimate, kill criteria. Stops and asks at every checkpoint rather than assuming. Use before committing to any timeline, when handed an ambiguous or underspecified ask, when someone asks "how long will this take?", or when an estimate already given needs revising.
user-invocable: true
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - Write
  - Edit
  - WebFetch
---

# /scope — interactive scoping

Take a piece of work from an ambiguous ask to a scoping doc someone can plan against.

**This skill is deliberately not autonomous.** It runs in short phases, each ending at a checkpoint where it stops and waits. Long unattended exploration produces confident, wrong scoping docs — the failure this exists to prevent.

## Operating rules

These override any instinct to be efficient.

1. **Stop at every checkpoint.** Present findings, ask the questions, then wait for a reply. Never chain two phases in one turn. Never say "I'll assume X and continue."
2. **Never guess at anything organizational.** Who owns a service, how slow review is, what another team is shipping, whether code is dead, what the deadline really means — none of this is in the repo. Ask.
3. **Surface every assumption as it's made.** Maintain a running ledger. A material assumption blocks progress until the user resolves it. Interrupting ten times beats one confident wrong doc.
4. **Challenge "easy."** If the user calls the work simple, quick, a tweak, or just wiring — say so once, plainly, and ask what makes them confident. Estimates given before exploration run 5–8x low. This is the single highest-yield intervention in the skill.
5. **Never produce the final number alone.** The estimate is co-produced. Propose a decomposition; the user supplies effort per slice.
6. **Distinguish what you verified from what you were told.** Cite `file:line` for anything read. Mark everything else as reported.

## Phase 0 — The ask and the boundary

**No exploration yet. Do not open the codebase.**

Ask:
- What exactly was asked, in their words? Who asked?
- **Has a date or estimate already been given? By whom, and had they done the work?** A deadline from someone who hasn't done the work is information about when they want it, not how long it takes.
- **What does "done" mean?** First shippable increment, or the full vision? Two people estimating different boundaries disagree forever and are both right.
- What is explicitly *not* in this?
- What metric or outcome does this move?

Then capture their **cold gut number** and label it clearly as unreliable. It is not an estimate. It is a baseline for comparison at Phase 7, and it exists only to measure what exploration was worth.

→ **CHECKPOINT. Wait.**

## Phase 1 — Recon plan

Propose the surfaces to examine — backend, frontend, API, data model, migrations, tooling/CLI, infra, tests, docs, generated clients, feature flags. Name what will be searched and why.

→ **CHECKPOINT. Ask what's missing, wrong, or irrelevant here. Wait.**

## Phase 2 — Prior art sweep

Search **every** surface from Phase 1, not just the interesting ones. Thorough backend recon paired with no frontend recon is a common and expensive failure.

For each surface report: what already exists that this work could extend, reuse, or accidentally duplicate. Cite `file:line`.

The question being answered is *"is this greenfield or is this finishing something?"* — and the answer is usually the second one.

→ **CHECKPOINT.** Ask:
- Which of this did you already know about?
- What did I miss?
- Is any of it dead, deprecated, or about to be replaced?
- Is anyone working in here right now?

**Wait.**

## Phase 3 — Assumption ledger

List every assumption made so far and every material unknown. For each: what it would change if wrong.

→ **CHECKPOINT. The user resolves each one or converts it to a risk. Nothing proceeds with an unresolved material assumption. Wait.**

## Phase 4 — Organizational map

**Ask; do not infer.** None of this is in the code:

- Who owns each system this touches? Who approves?
- How long does review actually take here? How many reviewers?
- Is anyone else working in this code, or about to be?
- Does anything depend on another team's roadmap?
- Are there systems in scope you can't see — closed-source services, other repos, vendor APIs?
- Who is the single person responsible for this landing?

→ **CHECKPOINT. Wait.**

## Phase 5 — Risks

Propose a ranked risk register using the lenses in `references/risk-lenses.md`. Each risk gets: **how you find out early** / **what it costs if it bites late**. A risk with no early-detection signal isn't managed, only named.

→ **CHECKPOINT.** Ask: which are real, which are theatre, what's missing, and *what have you seen break in this system before?* Their scar tissue beats the agent's inference. **Wait.**

## Phase 6 — Probes

For the top three risks, propose the cheapest check that could be done in the first two days. Every probe answers **two** questions:
- Does it work?
- **What happens at 10x?** ← the one that gets skipped

Feasibility is not viability. "Yes, it installs" and "yes, it installs 500 times without hitting a rate limit" are different findings.

→ **CHECKPOINT. Get approval before running anything. Wait.**

## Phase 7 — Estimate

Co-produced, never handed down.

1. Decompose into vertical slices that could each land separately.
2. **Ask the user for effort per slice.** Don't supply the numbers.
3. Apply any multiplier from `references/calibration.md`.
4. Produce a **range**, and name the single biggest driver of the spread.
5. State **focused-work or calendar** explicitly. People plan against calendar; review and coordination live in the gap.
6. **Show the delta against the Phase 0 cold number.** Say what exploration was worth. This is how calibration data accumulates.

→ **CHECKPOINT. Wait.**

## Phase 8 — The doc and the sentence

Write the doc using `references/doc-template.md`.

Then produce **the sentence** — the actual words to say to whoever asked. It contains the range, the boundary, the spread driver, and the re-forecast trigger. Nothing else.

### Completeness gate

Refuse to finish if any of these is missing:
- [ ] "Done" is defined
- [ ] Prior art checked on every surface
- [ ] Organizational risks present, not only technical ones
- [ ] Every risk has an early-detection signal
- [ ] Estimate is a range with a named spread driver
- [ ] Focused-work vs calendar stated
- [ ] Kill criteria written
- [ ] Re-forecast trigger set as a number

## Re-forecast mode

Invoke separately when work exceeds its trigger. **This is the most important thing the skill does**, and the part most likely to be skipped.

Help write exactly three things:
1. What I got wrong
2. The new number
3. What changed my mind

**Then stop.** Do not add reassurance. Do not append "but I think I can wrap this up in a few days."

That clause is what destroys trust — not the delay. A revised number is information. A revised number followed by a softener is a promise that expires, and a sequence of expiring promises is how an estimate stops carrying meaning. If the user drafts one, say so and cut it.
