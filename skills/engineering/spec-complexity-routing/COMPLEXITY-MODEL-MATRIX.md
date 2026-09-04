# Complexity, Model & Reasoning Effort Routing

Reference for `spec-complexity-routing`. Read through the pointers in [SKILL.md](./SKILL.md).

## Rubric

7 dimensions, each scored 0–4. Applies uniformly to mobile, web, and backend work. Read "component" as screen/view (mobile, web) or handler/service (backend).

### Algorithmic complexity
- 0: no non-trivial logic (getter/setter, direct CRUD passthrough)
- 1: simple conditionals, basic input validation
- 2: a known, well-documented algorithm (sort, search, pagination, standard state machine)
- 3: a custom algorithm or a combination of several (pathfinding, parsing, fuzzy matching, cache invalidation, diffing)
- 4: novel algorithm design, critical performance optimization, specialized data structures

### Concurrency & state consistency
- 0: synchronous, no shared state
- 1: local state, single request or single component
- 2: coordinated async operations (`Promise.all`, debounce/throttle, a simple queue)
- 3: shared state across actors with real race-condition risk (cache, message queue, WebSocket, multi-tab/multi-device sync)
- 4: distributed consistency, multi-service transactions, critical locking or idempotency guarantees

### Integration surface
- 0: one isolated file or component, no external dependency
- 1: one module, no new external dependency
- 2: 2-3 internal modules, or one well-documented external API
- 3: multiple internal services, or an external API with an unstable/undocumented contract
- 4: cross-repo change, infrastructure migration, breaking change to a public contract

### Data modeling & persistence
- 0: no persisted data change
- 1: new optional field, no migration needed
- 2: new entity, or a simple reversible migration
- 3: migration with backfill, relationship change, production data at stake
- 4: irreversible or large-scale migration, real risk of data loss or corruption

### Security & compliance sensitivity
- 0: no sensitive data involved
- 1: internal, unregulated data only
- 2: standard auth/authz already solved by the framework
- 3: PII, a new granular access-control rule, encryption at rest/in transit
- 4: payment flow, regulated data (LGPD/HIPAA/PCI), or a new attack surface

### Platform constraints
- 0: no constraint beyond the basics
- 1: one already-supported target (one modern browser, one OS version)
- 2: multiple targets with small divergence (evergreen browsers; iOS+Android through a shared layer)
- 3: real fragmentation (offline-first, native permissions, mandatory accessibility, legacy OS/browser support)
- 4: specific hardware/sensor access, app-store certification constraints, infrastructure-scale throughput/latency requirement

### Requirement ambiguity
- 0: acceptance criteria fully closed
- 1: minor details to clarify, non-blocking
- 2: 1-2 open product/design decisions
- 3: several open decisions, needs research or a spike before implementation
- 4: exploratory requirement, "discover the problem by building"

## Bands

Sum the 7 dimension scores (range 0–28), then map with this table:

| Sum | Band | Label |
|---|---|---|
| 0–2 | 1 | Trivial |
| 3–5 | 2 | Simple |
| 6–8 | 3 | Low-moderate |
| 9–13 | 5 | Moderate |
| 14–18 | 8 | High |
| 19–23 | 13 | Very high |
| 24–28 | 21 | Epic |

**Override**: any single dimension scored 4 floors the band at 8, regardless of sum. Two or more dimensions at 4 floors it at 13.

Band 21 is a decomposition signal, not an implementation band. Split the unit before routing any implementation model to it, then re-score the pieces.

## Reasoning effort

Choose reasoning effort from the task's cognitive shape, independently from its implementation model. Use the lowest level that covers the work. The number of files alone never earns `ultra`.

| Effort | Choose when | Typical work |
|---|---|---|
| `low` | The work is mechanical, repetitive, or a direct syntax transformation with no logical puzzle. | Known boilerplate, simple CRUD or SQL, straightforward language translation, documentation, comments, Swagger descriptions. |
| `medium` | The work needs bounded business context and a small amount of validation before delivery. | Isolated functions with internal logic, pagination or custom sorting, unit tests over several paths, local readability refactors. |
| `high` | One hard reasoning problem needs hypothesis testing or careful edge-case analysis. | Complex debugging, advanced algorithms, race-condition analysis, security review, or performance diagnosis. |
| `max` | Several hard reasoning concerns interact, or missing an edge case has a high cost. | Intermittent concurrency failures across components, security-sensitive algorithms with state, or performance work requiring several competing hypotheses. |
| `ultra` | The task itself is to plan, architect, or orchestrate an entire solution across many dependent steps or agents. | Project inception, system-wide architecture, large migrations, or multi-agent coordination across parallel workstreams. |

Apply these minimum-effort safeguards after the semantic choice:

- Any score of 4 in algorithmic complexity, concurrency and state consistency, or security and compliance sensitivity requires at least `high`.
- Two or more dimensions scored 4 require at least `max`.
- Band 21 requires `ultra` for decomposition and orchestration, while still receiving no implementation model.

Use `high` for one concentrated hard problem and `max` when several hard problems interact. Use `ultra` only when orchestration or whole-solution design is the work being performed. A normal implementation ticket can touch multiple files and remain `medium`, `high`, or `max`.

## Model routing

Default assignments, calibrated against the 8-model benchmark in `/Users/gude01/workspace/personal/tetris` (see `relatorios/relatorio-avaliacao-tecnica.pdf` for the full evidence: spec compliance, clean code, performance, resilience, design scores per model). Re-derive this table, but not the rubric or bands above, whenever the model roster changes.

| Band | Primary | Fallback | Blocked | Why |
|---|---|---|---|---|
| 1 | gpt-5.6-luna | gpt-5.6-terra | None | closed, deterministic scope; cheapest option that's still correct |
| 2 | gpt-5.6-terra | k3 | None | proven correct on closed/deterministic logic (bounded state, clear rules) |
| 3 | k3 | gpt-5.6-terra | gpt-5.6-luna | fast, clean bulk generation; luna's logic reliability drops past trivial scope |
| 5 | k3 | qwen3.8 | gpt-5.6-luna, deepseek-flash | still k3's zone, but the fallback needs to hold up once shared state is involved |
| 8 | gpt-5.6-sol | glm-5.3 | gpt-5.6-luna, deepseek-flash | this is where simulated-state heuristics broke for most benchmarked models; only the top resilience tier is proven correct here |
| 13 | gpt-5.6-sol | glm-5.3 → qwen3.8 | gpt-5.6-luna, deepseek-flash, muse | requires resilience ≥7.0 in the benchmark; low scorers carry silent logic bugs, not just missing features |
| 21 | None (do not implement) | None | all | decompose first; re-score the resulting units before assigning any model |

Adapt the primary and fallback columns to whatever models are actually live in your router config. The rubric and bands transfer as-is to any roster; these columns are expected to go stale.
