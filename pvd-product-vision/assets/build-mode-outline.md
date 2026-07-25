# Build mode outline (soft prior)

Program-level rules that every child PRD and architecture run must honor. Not a feature list.

## Modes (pick and name explicitly)

| Mode | Intent |
| ---- | ------ |
| `early-development` | Build from scratch; no real users yet. Optimize for learning speed and reversible decisions. |
| `production-readiness` | After cross-phase testing checks out. Hardening, security program, backfills, user-scale ops. |

Default pattern: delivery phases run under `early-development`; a **last phase** (or last PRD) is `production-readiness`.

## Child PRD composition rule

Sources for a delivery-phase PRD: `pvd.md` + `build-mode.md` + phase offset (+ optional brief).  
Any functional requirement (FR) that violates active build mode → move to readiness phase offset / deferred list, or mark out of scope with a pointer — do not silently implement in an early PRD.

## Early-development — allow

- Thin vertical slices that prove the phase outcome
- Minimal AuthN sufficient to develop (e.g. local/dev identity) if needed for the slice — postpone ≠ “no AuthN ever”
- Schema/migrations for the feature in hand — not historical backfill of imaginary production data
- Observability enough to debug locally/staging
- Tests that lock the behavior you are building now

## Early-development — postpone (route to production-readiness PRD)

Beyond the minimum needed to develop safely, postpone:

- Premature hardening and over-architecture (“enterprise” complexity without users)
- Large security programs (full threat model implementation, audit logging suites, compliance packs)
- Database backfill jobs and data-repair routines aimed at production history
- User-lifecycle ops that only pay off with real users (lifecycle emails at scale, retention jobs, complex entitlement migrations)
- Multi-region, SRE runbooks, chaos engineering, aggressive rate limiting — unless a phase outcome truly requires them to exist at all

## Production-readiness phase — owns

- Security and compliance writes deferred earlier
- Hardening of contracts, deployables, and operational envelopes
- Backfills and migration of real/accumulated data
- Production ops, on-call, and user-scale routines
- Re-check of build-mode exceptions granted during early phases
