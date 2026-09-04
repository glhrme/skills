## What it does

`spec-complexity-routing` scores one spec or ticket on seven dimensions, maps that score to a Fibonacci complexity band, and recommends an implementation model. It also chooses the lowest [reasoning effort](https://www.aihero.dev/ai-coding-dictionary/effort) that fully covers the task.

Model and effort are separate decisions. The complexity band selects the model from the maintained matrix; the task's cognitive shape selects `low`, `medium`, `high`, `max`, or `ultra`.

## When to reach for it

Type `/spec-complexity-routing`, or the agent reaches for it automatically after a spec or ticket is finalized and before implementation starts. Use [to-routed-tickets](https://aihero.dev/skills-to-routed-tickets) when you want spec creation, routing, and ticket publication in one flow.

## The two decisions

| Decision | Evidence it uses |
| --- | --- |
| Model | The seven complexity dimensions, Fibonacci band, overrides, and model benchmark matrix |
| Reasoning effort | Whether the work is mechanical, bounded, deeply analytical, or orchestration across a whole solution |

`high` covers one concentrated hard problem. `max` covers several interacting hard problems or a high cost of missing an edge case. `ultra` is reserved for whole-solution planning, large migrations, and multi-agent orchestration. Touching several files does not by itself require `ultra`.

## Common questions

**Does a high complexity band automatically mean ultra effort?**

No. The band routes the model. Effort follows the kind of thinking required, with minimum safeguards when severe algorithmic, concurrency, or security concerns are present.

**What happens at band 21?**

The unit receives no implementation model. Use `ultra` to decompose and orchestrate the work, then score each resulting unit separately.

**Does this choose the code-review model?**

No. It recommends the model and reasoning effort for implementation of the scored unit.

## It's working if

- The spec or ticket shows all seven scores, the total, band, and any override.
- It names a primary model, fallback, blocked models, and reasoning effort.
- The rationale for the model is distinct from the rationale for effort.
- Band-21 work is decomposed instead of sent to implementation.

## Where it fits

This is the routing step inside [to-routed-tickets](https://aihero.dev/skills-to-routed-tickets), between spec creation and ticket publication. It can also assess an existing spec or ticket on its own. See [ask-matt](https://aihero.dev/skills-ask-matt) for the full flow map.
