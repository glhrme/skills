---
name: spec-complexity-routing
description: Score a spec or ticket's implementation complexity on the Fibonacci scale (1-21), recommend an implementation model, and choose its reasoning effort. Use after a unit of work is finalized and before implementation starts.
---

# Spec Complexity Routing

Score implementation complexity on the Fibonacci scale, route to the model tier the score earns, and choose the reasoning effort the task needs. Run on one unit of work at a time: a ticket if `to-tickets` already split the spec, the whole spec otherwise.

## Process

1. **Pick the unit.** If tickets exist, score each ticket separately. A spec's tickets can span wildly different complexity, and scoring the spec as a whole hides the one ticket that actually needs the strongest model. Score the spec directly only when no ticket breakdown exists yet.

2. **Score each dimension**, 0 (none) to 4 (severe), using the rubric in [COMPLEXITY-MODEL-MATRIX.md](./COMPLEXITY-MODEL-MATRIX.md#rubric): algorithmic complexity, concurrency & state consistency, integration surface, data modeling & persistence, security & compliance sensitivity, platform constraints, requirement ambiguity.

3. **Apply the override**: any single dimension at 4 floors the band at 8; two or more dimensions at 4 floors it at 13. This stops one severe axis from hiding behind an otherwise low sum. A payment handler with trivial UI is still a payment handler.

4. **Sum the 7 scores and map to a Fibonacci band** using the bucket table in [COMPLEXITY-MODEL-MATRIX.md](./COMPLEXITY-MODEL-MATRIX.md#bands): 1, 2, 3, 5, 8, 13, or 21.

5. **Band 21 gets no implementation model.** Say so explicitly and hand the unit back for decomposition (split the ticket, or send the spec back through `to-tickets`). Recommend `ultra` for the decomposition and orchestration work, then re-score the pieces before implementation.

6. **Look up the band** in the routing table in [COMPLEXITY-MODEL-MATRIX.md](./COMPLEXITY-MODEL-MATRIX.md#model-routing) for the primary model, its fallback, and any models blocked at that band.

7. **Choose reasoning effort** independently from the model band, using [the reasoning-effort rubric](./COMPLEXITY-MODEL-MATRIX.md#reasoning-effort). Select the lowest effort that fully covers the task. Apply the minimum-effort safeguards from the rubric after making the semantic choice.

8. **Append a "Complexity & Routing" section** to the spec or ticket with: the 7 per-dimension scores, total, resulting band, override, primary model, fallback, blocked models, recommended reasoning effort, a one-line model rationale, and a one-line effort rationale.

## Recalibration

The complexity rubric and bands are stable; the model column is not. When the available model roster changes, re-run the benchmark behind [COMPLEXITY-MODEL-MATRIX.md](./COMPLEXITY-MODEL-MATRIX.md#model-routing) and replace that column. Never guess a new assignment or change the complexity rubric to compensate for a model change. Recalibrate the reasoning-effort rubric separately when observed task outcomes justify it.
