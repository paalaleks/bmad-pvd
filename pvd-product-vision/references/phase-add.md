# Add Phase (soft prior)

Use when the program vision grows after phase 1+ and a **new** phase was not in the original Carve — or when an existing planned phase must be reshaped against codebase reality.

Do **not** use this for simply starting the *next already-carved* phase after Close — that is Close → reinstall → `bmad-prd` with the next offset.

## Why reconcile first

Adding a phase against a stale spine widens the gap between `__pvd/` and the repo. Reconcile PVD ↔ reality before carving.

## Ritual

### 1. Reconcile (required)

Compare `{doc_workspace}/` (`pvd.md`, `architecture-principles.md`, `phases.md`, offsets, `deferred-decisions.md`) to:

- Current codebase (ownership, contracts, deployables)
- `{project-root}/docs/` if present
- Latest `__phase*_bmad-output/` / `__phase*_docs/` archives
- Live `_bmad-output/` if a phase is in-flight

Optional engine (when installed): `{project-root}/.agents/skills/bmad-document-project/SKILL.md` or `bmad-generate-project-context` — use as a reality scan, then distill findings back here. Do not dump a full brownfield dump into the PVD spine.

Write a short gap list for the user (ownership broken, missing handoffs, principles vs code, archive vs registry). For each gap: **fix spine** | **defer** | **accept debt** — log via memlog.

### 2. Apply agreed spine fixes

Update `pvd.md` / `architecture-principles.md` / `deferred-decisions.md` / existing offsets only as agreed. Collision rule still holds: two phases must not both own the same source tree or deployable.

### 3. Carve the new phase

- Pick next free `NN` in `offsets/phase-NN-<slug>.md`
- Add registry row in `phases.md`: `status: planned`, ownership boundaries, composition rule (`pvd.md` + this offset), empty archive fields
- Confirm no collision with archived or in-flight phases
- Update `pvd.md` strategic-phases summary if needed

### 4. Research (conditional)

Re-run Research (`references/research.md` → `bmad-technical-research`) **only if** principles, ownership, or cross-phase contracts changed. Purely additive phase vision with unchanged spine → skip Research; note that in memlog.

### 5. Finalize delta

- Memlog audit for this Add Phase run
- Set touched docs `updated: {date}` (package may already be `final`)
- Append memlog event: new phase key + reconcile summary
- Handoff: if a prior phase is still in-build, finish it (Close) before starting this one; else `bmad-prd` with `pvd.md` + the new offset (+ optional brief). Remind UX-before-epics if the phase is UI-heavy.

## Phrases that route here

"add phase", "new phase", "we need another stage", "vision grew", "insert phase", "update phases for reality"
