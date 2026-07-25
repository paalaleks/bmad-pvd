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

**Important (BMad installer behavior):** `npx bmad-method install` only *stages* the module — skill → `.agents/skills/` (and IDE mirrors), stub → `_bmad/pvd/`. It does **not** write `_bmad/custom/*.toml`. Those are [skill customizations](https://bmad-builder-docs.bmad-method.org/explanation/module-configuration/) (`_bmad/custom/{skill}.toml`), installed by the skill setup step — same pattern as official modules that say “run the setup skill after install” (e.g. BMad Loop).

Finish setup:

```text
/pvd-product-vision setup
```

Setup can install (default Yes, never overwrite without asking):
- `_bmad/custom/bmad-prd.toml` — child PRDs load PVD facts / build mode
- `_bmad/custom/bmad-code-review.toml` — autonomous review + push-to-`main` on approve

Or run **`pvd-product-vision`** once (Create). Activation copies either file into `_bmad/custom/` if missing. Use `setup` / `configure` again only to reconfigure.

## Layout

```
.
├── .claude-plugin/marketplace.json
├── README.md
├── LICENSE
└── pvd-product-vision/
    ├── SKILL.md
    ├── assets/          # outlines, module.yaml, module-help.csv, bmad-prd.toml, bmad-code-review.toml
    ├── references/      # research.md, phase-close.md
    └── scripts/         # merge-config.py, merge-help-csv.py
```

## After install

1. `pvd-product-vision` — Create with a product brief  
2. Per phase: `bmad-prd` using `pvd.md` + `build-mode.md` + phase offset  
3. When that phase’s **build** is done (not after PRD): `pvd-product-vision` **Close** — archives `_bmad-output` → `__phaseN_bmad-output` and `docs` → `__phaseN_docs`, preserves/restores the PVD package, then you run a fresh BMad install for the next phase  

## License

MIT
