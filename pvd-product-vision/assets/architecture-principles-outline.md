# Architecture principles outline (soft prior)

Lock principles and coherence rules — not concrete folder trees or framework wiring unless multi-phase impact is clear. Prefer **architecture dictionary** terms (bounded context, deployment unit, contract, published language, etc.) so Research and later phase docs share searchable language.

Heavy *implementation* detail (concrete scaffolding, full security programs, ops runbooks) → phase architecture or `deferred-decisions.md` — not the spine unless multi-phase impact is clear.

Judgment menu (pick what must stay coherent across phases; defer the rest):

- Design system (shared language vs per-app)
- Canonical data model / bounded contexts (context map; anti-corruption where needed)
- Testing strategy (program-level quality gates only)
- Git / branching (how parallel phases stay collision-safe)
- Workspace topology (monorepo vs polyrepo; module/package boundaries — not full scaffold)
- Deployment topology (deployment units; coupling vs independent deploy)
- Shared library vs extract-as-service; ownership of shared kernels
- Integration contracts (OpenAPI / AsyncAPI / events; versioning posture)
- AuthN / AuthZ and tenancy model (if cross-phase)
- Database ownership (database-per-service vs shared; who migrates)
- CI/CD topology and promotion paths (disambiguate from product "pipeline")
- Environment parity; config and secrets ownership
- Observability (logs, metrics, traces) standards
- Tooling inheritance (only where shared across deployment units)
- ADR expectation (how decisions are recorded)
- Phase handoff contracts (what a prior phase must publish)
- Local DX principles (whole program vs one phase)
- Dependency and security *baseline* (principles only)
- AI: model provider vs self-host; inference vs fine-tune
- AI: RAG vs tools/MCP; agent vs workflow; memory ownership
- AI: guardrails; eval harness; model gateway; AI observability
- AI: prompt/skill artifacts as versioned assets (vocabulary: see `references/research.md`)

Always maintain an explicit deferred list for what phase architecture owns.
