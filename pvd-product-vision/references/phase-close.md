# Phase close & archive (soft prior)

User routine for multi-phase programs. Keep it simple — sibling BMad cycles at the same project root, not nested installs.

## When (not after PRD)

Close a phase only after that phase’s **full BMM delivery** is done (PRD → architecture → epics → implementation / stories complete enough to ship the phase outcome).

Do **not** archive after PRD alone — PRD is the start of the phase cycle.

## Archive names

At `{project-root}`:

| Live (active cycle) | After close (phase N) |
| ------------------- | --------------------- |
| `_bmad-output/` | `__phaseN_bmad-output/` |
| `docs/` | `__phaseN_docs/` |

Examples: `__phase1_bmad-output`, `__phase2_docs`. Use the phase number from `phases.md` / offset (`phase-01` → `1`). No brackets in the folder name.

If `docs/` is missing or empty, skip the docs rename (still record that in `phases.md`).

## Preserve the PVD spine

The PVD package normally lives under `_bmad-output/planning-artifacts/pvd/`. Archiving the whole `_bmad-output` would bury it.

**Required before/with archive:**

1. Copy `{planning_artifacts}/pvd/` → a temp hold under `{project-root}` (e.g. `.pvd-hold/`) **or** note you will restore from the archive after reinstall.
2. Rename live folders as above.
3. User runs a **fresh** `npx bmad-method install` (same modules/custom-source as before) so a new `_bmad-output/` (and empty `docs/` if needed) exists.
4. Restore `pvd/` into the new `{planning_artifacts}/pvd/` from `.pvd-hold/` or from `__phaseN_bmad-output/planning-artifacts/pvd/`.
5. Remove `.pvd-hold/` if used.
6. Re-run `pvd-product-vision setup` if team customizes were wiped — or rely on activation to re-copy missing `bmad-prd.toml` / `bmad-code-review.toml`.

## Record in `phases.md`

For the closed phase, set at least:

- `status: archived` (or `done`)
- `archive_bmad_output: __phaseN_bmad-output`
- `archive_docs: __phaseN_docs` (or `none`)
- `closed: {date}`
- Pointer to next phase offset / “next: bmad-prd with …”

Scan for existing `__phase*_bmad-output` / `__phase*_docs` on Overview so the registry stays truthful.

## Next phase

After restore: BMM `bmad-prd` for the **next** phase only — sources = restored `pvd.md` + `build-mode.md` + that phase’s offset (+ optional brief).
