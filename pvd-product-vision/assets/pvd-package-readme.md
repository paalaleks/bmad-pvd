---
title: How to work on the PVD package
type: readme
audience: any agent or person about to edit __pvd/
---

# Working on `__pvd/`

This package is the **program spine**. It holds only what must stay true across *every* phase. Each
phase PRD is a child of it.

**Read this before adding anything.** The default answer to "should this go in the PVD?" is **no**.
Most content is in practice deferred to child PRDs, and the package's value comes from being short
enough that a PRD author reads all of it and acts without the authoring conversation in the room.
Length destroys that. So does parent-level detail that pre-empts a decision the phase was supposed
to make.

---

## The altitude test

Three filters, in order. An addition must pass all three.

1. **Does it trace to a program invariant (in `pvd.md`) or to a named later-phase outcome?**
   If the justification is only "a future phase could get this wrong" — that is true of almost
   anything, and it is not a reason. Traces to nothing → phase architecture.

2. **Is getting it wrong a retrofit, or merely cleanup?**
   Retrofits cannot be added later without a migration (provenance, erasure, irreversible
   contracts). Enumerated branches, a naming convention, a prompt shape: those are cleanup.
   Cleanup is not spine.

3. **Would reversing it force *multiple* child PRDs to rewrite?**
   One phase would care → offset or deferred decision.

## Where things actually go

| It is… | Home |
| ------ | ---- |
| A promise to the user that holds in every phase | `pvd.md` — an invariant |
| A binding constraint on how any phase's architecture is built | `architecture-principles.md` — an AP |
| Something deliberately **not** decided, with a phase that will own it | `deferred-decisions.md` |
| Phase-scoped vision, scope, sequencing, surfaces, routes | `offsets/phase-NN-*.md` |
| A technology choice, module shape, schema, or contract | that phase's architecture (`_bmad-output/`) |
| A feature, FR, or acceptance criterion | that phase's PRD / epics |

Canonical list of what is explicitly *not* spine: the **"What phase architecture owns"** section at
the foot of `architecture-principles.md` (when present).

Two traps worth naming:

- **A decided feature is not a deferred decision.** Once it has an owner and a shape, it belongs in
  the phase, recorded where the decision was made. Delete the deferred row.
- **Do not fix a phase-owned list in the parent.** Stating *that* there must be a small closed set is
  spine; naming *which* items is the phase's call.

## House shape for a principle

Match the neighbours, at roughly a sibling's length:

```
### AP-NN — <imperative rule, one line>

<the rule, 2–4 lines>

*Forbids:* <the concrete thing someone would otherwise do>

*Why program-level:* <which invariant or phase outcome it protects>
```

No tables. No worked examples. No multi-paragraph rationale. If the argument needs paragraphs, it
belongs in the phase architecture or a `docs/` note.

**Prefer extending an existing principle over adding a numbered one.** Cheaper, and it keeps the
numbering stable for everything that cites it.

## Calibrate against what is already here

The existing entries are the best available guide. Read two or three neighbours before writing.

Typical spine shapes (look for these patterns in *this* package's APs):

- **Rule + defer mechanism** — states the invariant, then hands implementation choices to a deferred
  item or phase architecture. The rule is spine; the mechanism never is.
- **Boundary without product** — keeps "one access path / one contract" and refuses to name a
  vendor or library too early.
- **Conditional principle** — imposes nothing until a later phase condition is true, and says so
  outright.
- **Rare technology exception** — a concrete product on the spine only with an explicit trust,
  compliance, or irreversibility argument. An exception needs that specific argument to earn its place.
- **Big ≠ spine** — a large business decision can still belong in a phase PRD. Size is not altitude.
- **Permanent principle vs phase-local quantity** — protect the separation; leave counts, lists, and
  routes in the offset.

The pattern: the spine states the **rule and its reason**, then names the phase or deferred item
that owns the rest.

**Expect to delete most of what you draft.** A normal net addition is a clause on an existing
principle, or a single row. A new numbered principle is rare, and a new invariant rarer still.

## Changing this package

- Use the **`pvd-product-vision` skill** (`update` for principle/deferred edits, `add-phase`,
  `overview`, `close`). Do not hand-edit as a substitute for the ritual — it carries the conflict
  check and the finalize steps.
- **Log every decision** to the memlog as you go:
  ```bash
  uv run _bmad/scripts/memlog.py append --path __pvd/.memlog.md \
    --type <decision|assumption|direction|gap|note|event> --text "<gist>"
  ```
  When a later decision reverses an earlier entry, append a **superseding** entry rather than editing
  history.
- **Bump `version` and `updated`** in frontmatter and add a one-line changelog entry when those
  fields exist. If a change is withdrawn before commit, revert the bump too.
- A phase architecture that violates an AP is **not making a trade-off** — it triggers a PVD Update.
  Escalate rather than deviate.

## Files

| File | What it is |
| ---- | ---------- |
| `pvd.md` | The spine: purpose, thesis, invariants, phase summary, how child PRDs consume it |
| `architecture-principles.md` | Binding constraints on every phase architecture |
| `deferred-decisions.md` | Intentionally undecided, with owner and the constraint that holds regardless |
| `phases.md` | Phase registry, ownership map, archive paths |
| `offsets/` | Per-phase vision slices — everything phase-local lives here, not on the spine |
| `feasibility-research.md` | Research verdicts (keep / change / defer) against the principles |
| `source-brief.md` | Frozen snapshot of the origin brief (optional); `docs/` is archived per phase, this is not |
| `.memlog.md` | Append-only decision log for the package |
| `README.md` | This file — edit-judgment for the package |

**This package is never archived.** At phase close, `_bmad-output/` and `docs/` become
`__phaseN_*`; `__pvd/` stays and the next phase's PRD is built from it. That is why it must remain
self-sufficient — never cite a path that lives inside `docs/` or `_bmad-output/` as a source of
truth.
