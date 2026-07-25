# Phase close & archive (soft prior)

User routine for multi-phase programs. Keep it simple — same project root, archive the BMad cycle folders, reinstall. The PVD spine is **not** inside those folders.

## When (not after PRD)

Close a phase only after that phase’s **full BMM delivery** is done (PRD → architecture → epics → implementation / stories complete enough to ship the phase outcome).

Do **not** archive after PRD alone — PRD is the start of the phase cycle.

## What moves vs what stays

| Path | On Close |
| ---- | -------- |
| `_bmad-output/` | Rename → `__phaseN_bmad-output/` |
| `docs/` | Rename → `__phaseN_docs/` (skip if missing/empty) |
| `{project-root}/pvd/` | **Stays.** Program spine — never archive with the phase |

Examples: `__phase1_bmad-output`, `__phase2_docs`. Phase number from `phases.md` / offset (`phase-01` → `1`). No brackets in the folder name.

## Ritual

1. Confirm phase N build is done.
2. Rename `_bmad-output` → `__phaseN_bmad-output` (and `docs` → `__phaseN_docs` when present).
3. Update `{project-root}/pvd/phases.md` (status, archive paths, date, next offset).
4. User runs a **fresh** `npx bmad-method install` (same modules/custom-source as before) so a new empty `_bmad-output/` (and `docs/` if needed) exists.
5. If team customizes were wiped, re-run `pvd-product-vision setup` — or rely on activation to re-copy missing `bmad-prd.toml` / `bmad-code-review.toml`.
6. Next: BMM `bmad-prd` for the next phase — sources still `{project-root}/pvd/` (`pvd.md` + `build-mode.md` + offset).

No hold/restore of PVD. No nested installs.

## Record in `phases.md`

For the closed phase, set at least:

- `status: archived` (or `done`)
- `archive_bmad_output: __phaseN_bmad-output`
- `archive_docs: __phaseN_docs` (or `none`)
- `closed: {date}`
- Pointer to next phase offset / “next: bmad-prd with …”

Scan for existing `__phase*_bmad-output` / `__phase*_docs` on Overview so the registry stays truthful.
