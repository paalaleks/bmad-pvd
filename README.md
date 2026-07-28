# Product Vision Document (PVD) — BMad module

Before-anything BMad module: turn a product vision into an overarching PVD and phase offsets so focused child BMM PRDs stay collision-safe.

## Install

With BMad Method (include official BMM + this module):

```bash
npx bmad-method install \
  --directory . \
  --modules bmm \
  --custom-source https://github.com/paalaleks/bmad-pvd \
  --tools cursor \
  --yes
```

Or point at this folder locally while developing:

```bash
npx bmad-method install \
  --directory ~/my-project \
  --modules bmm \
  --custom-source /path/to/this/repo \
  --tools cursor \
  --yes
```

**Important:** `npx bmad-method install` only *stages* the skill. Run **`pvd-product-vision setup`** (or first Create) to install team customizes into `_bmad/custom/` if missing:

- `bmad-prd.toml` — child PRDs load PVD facts
- `bmad-code-review.toml` — autonomous review + push-to-`main` on approve
- `bmad-ux.toml` — HTML mock HITL before epics (or skip with rationale)
- `bmad-create-epics-and-stories.toml` / `bmad-create-story.toml` — UI-heavy stories cite mocks or skip rationales

## Layout

```
.
├── .claude-plugin/marketplace.json
├── README.md
├── LICENSE
└── pvd-product-vision/
    ├── SKILL.md
    ├── assets/          # outlines, module.yaml, team customize tomls
    ├── references/      # research.md, phase-close.md, phase-add.md
    └── scripts/
```

## After install

1. `pvd-product-vision` — Create with a product brief (`__pvd/`)  
2. Per phase: `bmad-ux` (mock HITL) → architecture → epics → stories → `bmad-prd` / build  
3. When that phase’s **build** is done (not after PRD): `pvd-product-vision` **Close** — archives `_bmad-output` / `docs` to `__phaseN_*`; `__pvd/` stays; fresh BMad install for the next phase  
4. Vision grew / new stage: `pvd-product-vision` **Add Phase** — reconcile PVD vs codebase first, then carve the new offset  

## License

MIT
