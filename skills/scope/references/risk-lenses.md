# Risk lenses

Run every one. The ones most often skipped are at the top.

## 1. The error path
The feature is cheap; everything around it is the project. For each moving part:
- What happens on partial failure — some succeed, some don't?
- Cancellation mid-flight?
- Timeouts?
- Retry — and does retry create duplicates?
- Can a half-finished run resume, or does it strand?

## 2. Works in a demo, dies under load
Feasibility is not viability.
- Rate limits, quotas, throttling
- Latency at realistic volume
- N+1s, unbounded queries, missing indexes
- Cost at production scale

## 3. Prior art — on every surface
Backend, frontend, API, data model, CLI/tooling, generated clients, docs, tests.
- Does a version of this already exist, perhaps half-built?
- Is there a component library or registry to extend rather than duplicate?
- Would a reviewer say "why didn't you use X?"

## 4. Boundaries you can't see
- Closed-source or other-team services
- Vendor APIs, especially `@latest` pinning
- Auth: OAuth scopes, token expiry, existing grants that need reconnection
- What does the *client* code reveal about the service's real API surface?

## 5. State and consistency
- How many systems own a piece of this state?
- Can they disagree? What happens when they do?
- Is creation/deletion ordered so nothing is half-created?

## 6. Migration and backfill
- Existing rows needing migration
- Dual-write or dual-read windows
- Rollback story
- How long does the backfill actually take in production?

## 7. Organizational
**Not in the code. Ask.**
- How many owners approve, and how slow are they?
- Is anyone else in this code right now?
- Does this depend on another team's roadmap?
- How does it land — one enormous PR is not reviewable

## 8. Estimation by invalid analogy
"X took two days, so Y takes two days." Check the comparison is the same *class* of problem. Wiring something already present is not the same as delivering it somewhere it isn't.

## 9. Measurement
- How will anyone know this worked?
- Can the system report success before the user got value?
- Is completion inferred from something that doesn't prove it?
