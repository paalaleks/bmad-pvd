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

Then run **`pvd-product-vision`** with `setup` / `configure` once (or on first use if `pvd` is not in `_bmad/config.yaml`). Setup can also install team customizes: `_bmad/custom/bmad-prd.toml` and `_bmad/custom/bmad-code-review.toml`.

## Layout

```
.
├── .claude-plugin/marketplace.json
├── README.md
├── LICENSE
└── pvd-product-vision/
    ├── SKILL.md
    ├── assets/          # outlines, module.yaml, module-help.csv, bmad-prd.toml, bmad-code-review.toml
    ├── references/      # research.md
    └── scripts/         # merge-config.py, merge-help-csv.py
```

## After install

1. `pvd-product-vision` — Create with a product brief  
2. Per phase: `bmad-prd` using `pvd.md` + phase offset  
3. When that phase’s **build** is done (not after PRD): `pvd-product-vision` **Close** — archives `_bmad-output` → `__phaseN_bmad-output` and `docs` → `__phaseN_docs`; `{project-root}/__pvd/` stays; then fresh BMad install for the next phase  

## License

MIT
