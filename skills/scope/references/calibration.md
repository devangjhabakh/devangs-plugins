# Personal calibration

Measured, not guessed. Update after each project — the value compounds.

> Private working notes. Delete this file if the repo ever stops being private.

## The alarm

> **"This is just a quick tweak."**

Measured across five projects at two companies: every project expected to be easy missed by **5–8x** or shipped on time and then leaked defects for weeks. Every project expected to be hard went well.

The day-one sense of difficulty is a reliable predictor, **inverted**. "Easy" does not mean easy — it means not yet looked at. Treat the feeling as an alarm, never as information.

## The rule

> **Failure comes from committing to time without doing the pre-work.**

Not from being slow, and not from bad estimates. Two successes both did the pre-work or had no deadline; three failures all had a number — stated, imposed, or private — with no homework behind it.

## Measured multipliers

| Condition | Error |
|---|---|
| Cold estimate, unfamiliar code, happy-path reasoning | **5–8x low** |
| After context transfer, consultation, option evaluation | **~1.0x** |
| After ~2h structured scoping in an unfamiliar repo | **~1.3x** |
| Cold, immediately after being burned by an under-estimate | **2–4x high** |

The cold number is unreliable in **both** directions. The answer was never "be more conservative" — it is that the cold number carries no information and shouldn't be spoken.

## Cost of scoping

~2 hours in a large unfamiliar repo; ~16 minutes once oriented and using agents well. Under 1% of project duration, for roughly a 6x accuracy gain.

> Weeks-sized work → buy 30–120 minutes.
> Days-sized work → buy 30 minutes.

## The ask

> *"My estimates get a lot more accurate after half an hour of digging. Want the thirty-minute version, or the guess?"*

Not a confession — a tradeoff, stated as fact, with the decision handed back. Nobody refuses thirty minutes.

## Categories systematically missed

1. **The error path.** The feature is cheap; partial failure, cancellation and timeouts are the project.
2. **Works in a demo, dies under load.** Feasibility checked, viability not.
3. **Prior art on the surface that wasn't interesting.** Thorough backend recon, zero frontend recon.
4. **Organizational cost.** Review latency, how it lands, who else is in the code — consistently priced at zero.
5. **The defect tail.** Shipping on time while deferring the cost into production, where it arrives weeks later as bugs and never gets connected back to the estimate.

## Running log

| Date | Project | Cold | After scoping | Actual | Notes |
|---|---|---|---|---|---|
