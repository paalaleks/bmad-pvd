# Research Feasibility

**Outcome:** Evidence that early PVD principles and coherence choices are *durable* under tech and architecture fit — so the team can control what locks before finalize.

**Consumer:** The same Create/Update run (and later readers of `feasibility-research.md`). Verdicts must be actionable as keep / change / defer on spine items.

**Scope:** Tech and architecture fit only. Not market research, not team-skills inventory, not a license to write concrete monorepo scaffolding into the parent.

**Altitude:** Research serves systematic principles and coherence across later build specs. If a finding would force over-specifying setup at PVD altitude, prefer **defer** to phase architecture over locking concrete detail. The bar is "will this principle stand?", not "have we designed the codebase?"

## Wording is the research instrument

Architecture, platform engineering, and AI/ML each have their own dictionary. Vague paraphrases yield low-signal blog posts; precise terms yield ADRs, vendor docs, and pattern literature.

Before searching or writing a claim:

1. **Translate** into canonical vocabulary (glossaries below). Prefer practitioner and documentation terms — for AI, prefer AI-native terms over "the smart part" / "ChatGPT does X".
2. **Name the claim** that way in `feasibility-research.md`.
3. **Search** with those terms (+ official product/version names). Disambiguate product "pipeline" vs CI/CD vs ML training pipeline.
4. **Quote sources** in their wording so the user can re-find them.
5. If you cannot map a fuzzy idea to a known term, **ask once** or **defer** — do not research under a wrong label.

Wrong wording fails the control gate.

## How to run

1. Extract the candidate lock set (principles, stack *rules*, ownership/collision boundaries, contracts) — not every vision sentence.
2. Drop or rewrite claims that are still implementation scaffolding.
3. Gather current evidence per claim (web subagents, official docs, ADRs, user constraints). Prefer primary/current sources — AI stack moves fast. Queries must use normalized terms.
4. Write `{doc_workspace}/feasibility-research.md`: canonical claim → search terms → evidence → **keep** | **change** | **defer** → why. Group by theme.
5. Surface **change** and **defer** before Finalize; apply agreed updates to spine/deferred list; log via memlog.
6. Do not invent parent architecture under the banner of research. Thin evidence → defer, do not guess a scaffold.

## Glossary — software architecture

Translation aid, not a mandatory taxonomy. Smallest accurate term.

| Prefer saying | Over | What you are testing |
| ------------- | ---- | -------------------- |
| Bounded context / context map | "separate areas of the app" | Ownership of language and models across phases |
| Anti-corruption layer | "adapter so they don't mess each other up" | Protecting a phase from another's model |
| Published language / shared kernel | "common types package" | How shared meaning is owned and versioned |
| Contract (OpenAPI, AsyncAPI, proto, events) | "the API" (alone) | Stability surface between deployables |
| Deployment unit / bounded deployable | "service" or "app" (ambiguous) | What ships independently — collision unit |
| Module boundary / package boundary | "folder" | Ownership for parallel PRDs |
| Monorepo vs polyrepo; workspace | "one big repo" | VCS topology principle |
| Shared library vs service extract | "reuse code" | Coupling and release cadence |
| Database per service vs shared database | "one DB or many" | Schema ownership and migration rights |
| Synchronous vs async integration; choreography vs orchestration | "they talk to each other" | Runtime coupling |
| Strangler fig / branch by abstraction | "migrate later" | Evolution without big-bang rewrite |
| Idempotency, exactly-once vs at-least-once | "reliable messages" | Messaging durability claims |
| AuthN / AuthZ; tenancy model | "login and permissions" | Cross-app identity principles |
| Observability (logs, metrics, traces) | "monitoring" | Cross-phase ops coherence |
| CI/CD topology; promotion path | "the pipeline" (ambiguous with product pipeline) | How artifacts move to prod |
| Environment parity; config/secrets ownership | "dev/stage/prod" | Who owns config across phases |
| ADR (Architecture Decision Record) | "we decided" | How principles are recorded for later phases |

## Glossary — AI / ML systems

| Prefer saying | Over | What you are testing |
| ------------- | ---- | -------------------- |
| Foundation model / LLM; model provider vs self-host | "ChatGPT" or "the AI" (alone) | Where inference runs and who owns the model lifecycle |
| Inference vs training / fine-tuning / continued pretraining | "train the model" (when you mean prompt or RAG) | Whether the spine claims training cost/ops it does not need |
| Prompt engineering vs system prompt vs structured output / tool schema | "tell the AI what to do" | Interface contract to the model |
| RAG (retrieval-augmented generation); corpus; chunking; embeddings; vector index | "AI looks up our docs" | Retrieval architecture and ownership of the knowledge base |
| Grounding / citation / attribution | "it should be accurate" | How answers bind to retrieved evidence |
| Tool calling / function calling; MCP (Model Context Protocol) | "plugins" or "the AI can use stuff" | How the model reaches external systems |
| Agent vs workflow vs single-shot completion | "autonomous AI" (vague) | Control loop, state, and human gates |
| Multi-agent / orchestrator–worker | "several AIs talking" | Coordination pattern and failure boundaries |
| Memory (short-term context vs long-term store); session vs user memory | "it remembers" | What persists, where, and who owns PII |
| Context window; context management / compaction | "enough room for the prompt" | Limits that force architecture choices |
| Guardrails; input/output filtering; policy layer | "safety" (alone) | Cross-phase safety controls ownership |
| Eval / evaluation harness; offline vs online eval; golden set | "we will test the AI" | How quality is measured before lock-in claims |
| Human-in-the-loop (HITL); approval gates | "someone checks" | Where humans sit in the control loop |
| Model gateway / router; fallback model | "switch models if needed" | Provider abstraction and resilience |
| Batch vs online / real-time inference | "run AI on all the data" | Latency, cost, and deployment shape |
| Feature store; model registry; experiment tracking | "ML stuff" | Classical ML platform boundaries (when applicable) |
| Prompt/version registry; skill or agent definition as artifact | "the prompts in the code" | How prompt/agent assets version across phases |
| AI observability (prompt traces, token/cost metrics, latency) | "monitoring" (alone) | Ops coherence for LLM paths |
| Data residency / training-data policy; PII redaction for model I/O | "privacy with AI" | Compliance constraints on spine |
| Determinism vs sampling; temperature; seed | "make it consistent" | Whether reproducibility is a real requirement |

## Glossary — build mode / premature complexity

| Prefer saying | Over | What you are testing |
| ------------- | ---- | -------------------- |
| YAGNI; premature optimization / premature hardening | "make it solid now" | Whether early phases over-build |
| Production readiness / hardening phase | "polish later" (vague) | Dedicated late PRD for security, backfills, ops |
| Backfill / data migration (production-scale) | "fix the old data" | Jobs that only matter once real data/users exist |
| Minimum viable AuthN vs security program | "add security" | Dev-sufficient identity vs full hardening scope |
| Build mode (`early-development` vs `production-readiness`) | "be careful about scope" | Explicit rules child PRDs must inherit |

Keep product-domain terms (e.g. "ingestion pipeline", "compete report") **and** map their architectural/AI role so search hits both literatures.

## Degradation

If research tools are unavailable, state the gap, use user-supplied constraints and training knowledge with explicit `[ASSUMPTION]` tags, and still produce verdicts in canonical wording — never skip the control gate silently.
