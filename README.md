# Product Vision Document (PVD) — BMad module

Before-anything BMad module: turn a product vision into an overarching PVD, build mode, and phase offsets so focused child BMM PRDs stay collision-safe and do not over-harden early.

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

Then run **`pvd-product-vision`** once. The BMad installer places the skill and help entries; it does **not** copy `_bmad/custom/bmad-prd.toml`. On activation the skill installs that override if missing (so BMM PRD can load PVD facts and honor build mode). Use `setup` / `configure` only to reconfigure.

## Layout

```
.
├── .claude-plugin/marketplace.json
├── README.md
├── LICENSE
└── pvd-product-vision/
    ├── SKILL.md
    ├── assets/          # outlines, module.yaml, module-help.csv, bmad-prd.toml
    ├── references/      # research.md
    └── scripts/         # merge-config.py, merge-help-csv.py
```

## After install

1. `pvd-product-vision` — Create with a product brief  
2. Per phase: `bmad-prd` using `pvd.md` + `build-mode.md` + phase offset  

## License

MIT
