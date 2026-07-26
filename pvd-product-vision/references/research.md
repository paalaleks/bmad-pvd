# Research Feasibility

**Outcome:** Evidence that early PVD principles and coherence choices are *durable* under tech and architecture fit — so the team can control what locks before Finalize.

**When:** After Forge + Carve (and on Update when principles change). Do **not** run before those steps — the candidate lock set must exist so research is validation, not open-ended topic fishing.

**Engine:** Run via the installed BMM skill `{project-root}/.agents/skills/bmad-technical-research/SKILL.md` (or the IDE mirror under `.claude/skills/`). That workflow owns web search, steps, citations, and the full technical report. This file owns PVD altitude, the brief you hand it, and how verdicts land back on the spine.

## Why after Forge/Carve

Decisions are drafted first; Research is sent off against them. Early research on a thin vision wastes cycles or locks tech before principles and phase boundaries exist.

## How to run

1. **Extract the candidate lock set** from the drafted package (principles, stack *rules*, ownership/collision boundaries, contracts, build-mode constraints) — not every vision sentence. Drop or rewrite claims that are still implementation scaffolding.
2. **Normalize wording** with the glossaries below before naming claims or search terms. Wrong wording fails the control gate.
3. **Hand off to `bmad-technical-research`** with topic/goals already set — skip its interactive "What do you want to research?" discovery:
   - `research_type` = `technical`
   - `research_topic` = short title derived from the program + "PVD principle durability" (e.g. `autocompete-pvd-architecture-durability`)
   - `research_goals` = validate keep/change/defer for the lock-set claims; stay at principle altitude; do not produce monorepo scaffolding
   - Seed context: paths under `{doc_workspace}/` (`architecture-principles.md`, `build-mode.md`, `phases.md`, offsets summary) plus the bullet list of claims/search terms
4. Let that skill produce its normal report under `{planning_artifacts}/research/` (ephemeral with the BMad cycle — fine).
5. **Write `{doc_workspace}/feasibility-research.md`** (durable, under `__pvd/`): each canonical claim → search terms → evidence pointers (cite the technical report + primary sources) → **keep** | **change** | **defer** → why. Group by theme.
6. Surface **change** and **defer** before Finalize; apply agreed updates to spine/deferred list; log via memlog.
7. Thin evidence → defer, do not guess a scaffold. Do not invent parent architecture under the banner of research.

**Altitude:** The bar is "will this principle stand?", not "have we designed the codebase?" If a finding would force over-specifying setup at PVD altitude, prefer **defer** to phase architecture.

## Wording is the research instrument

Architecture, platform engineering, and AI/ML each have their own dictionary. Vague paraphrases yield low-signal blog posts; precise terms yield ADRs, vendor docs, and pattern literature.

Before searching or writing a claim:

1. **Translate** into canonical vocabulary (glossaries below). Prefer practitioner and documentation terms — for AI, prefer AI-native terms over "the smart part" / "ChatGPT does X".
2. **Name the claim** that way in `feasibility-research.md` and in the brief to technical research.
3. **Search** with those terms (+ official product/version names). Disambiguate product "pipeline" vs CI/CD vs ML training pipeline.
4. **Quote sources** in their wording so the user can re-find them.
5. If you cannot map a fuzzy idea to a known term, **ask once** or **defer** — do not research under a wrong label.

## Glossary — software architecture

Translation aid, not a mandatory taxonomy. Smallest accurate term.

| Prefer saying | Over | What you are testing |
| ------------- | ---- | -------------------- |
| Bounded context / context map | "separate areas of the app" | Ownership of language and models across phases |
| Anti-corruption layer | "adapter so they don't mess each other up" | Protecting a phase from another's model |
| Published language / shared kernel | "common types" | What is intentionally shared |
| Deployment unit / bounded deployable | "the service" / "the app" | What ships independently |
| Contract / API boundary / ACL | "the interface" | Cross-phase coupling rules |
| Event-driven vs request/response | "how they talk" | Integration style at principle level |
| Consistency model (strong / eventual) | "sync vs async data" | Multi-phase data coherence |
| Idempotency; exactly-once vs at-least-once | "don't double-run" | Cross-phase job/message safety |
| Observability (logs, metrics, traces) | "monitoring" | Cross-phase ops coherence |
| Environment parity; config/secrets ownership | "dev/stage/prod" | Who owns config across phases |
| ADR (Architecture Decision Record) | "we decided" | How principles are recorded for later phases |

## Glossary — AI / ML (when the program includes AI)

| Prefer saying | Over | What you are testing |
| ------------- | ---- | -------------------- |
| RAG; retrieval corpus ownership | "it looks stuff up" | Who owns knowledge vs generation |
| Tool use / function calling / MCP | "plugins" | Agent capability boundaries across phases |
| Agent vs workflow vs single completion | "the AI" | Orchestration ownership |
| Eval harness; offline vs online eval | "we tested the prompts" | How quality is proven across phases |
| Guardrails; input/output filtering; policy layer | "safety" (alone) | Cross-phase safety controls ownership |
| Model gateway / router; provider abstraction | "which model" | Swap-out without multi-phase rewrite |
| Prompt/version registry; skill or agent definition as artifact | "the prompts in the code" | How prompt/agent assets version across phases |
| YAGNI; premature optimization / premature hardening | "make it solid now" | Whether early phases over-build |
| Production readiness / hardening phase | "polish later" (vague) | Dedicated late PRD for security, backfills, ops |
