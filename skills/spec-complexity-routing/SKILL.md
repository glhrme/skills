---
name: spec-complexity-routing
description: Score a spec or ticket's implementation complexity on the Fibonacci scale (1-21) and recommend which model tier should implement it, using a routing matrix keyed by complexity band. Use right after a spec or ticket is finalized, before implementation starts — for mobile, web, or backend work alike.
---

# Spec Complexity Routing

Score implementation complexity on the Fibonacci scale, then route to the model tier the score earns. Runs on one unit of work at a time: a ticket if `to-tickets` already split the spec, the whole spec otherwise.

## Process

1. **Pick the unit.** If tickets exist, score each ticket separately — a spec's tickets can span wildly different complexity, and scoring the spec as a whole hides the one ticket that actually needs the strongest model. Score the spec directly only when no ticket breakdown exists yet.

2. **Score each dimension**, 0 (none) to 4 (severe), using the rubric in [COMPLEXITY-MODEL-MATRIX.md](./COMPLEXITY-MODEL-MATRIX.md#rubric): algorithmic complexity, concurrency & state consistency, integration surface, data modeling & persistence, security & compliance sensitivity, platform constraints, requirement ambiguity.

3. **Apply the override**: any single dimension at 4 floors the band at 8; two or more dimensions at 4 floors it at 13. This stops one severe axis from hiding behind an otherwise low sum — a payment handler with trivial UI is still a payment handler.

4. **Sum the 7 scores and map to a Fibonacci band** using the bucket table in [COMPLEXITY-MODEL-MATRIX.md](./COMPLEXITY-MODEL-MATRIX.md#bands): 1, 2, 3, 5, 8, 13, or 21.

5. **Band 21 gets no model.** Say so explicitly and hand the unit back for decomposition (split the ticket, or send the spec back through `to-tickets`) — nothing is implemented at this band; re-score the pieces instead.

6. **Look up the band** in the routing table in [COMPLEXITY-MODEL-MATRIX.md](./COMPLEXITY-MODEL-MATRIX.md#routing) for the primary model, its fallback, and any models blocked at that band.

7. **Append a "Complexity & Routing" section** to the spec or ticket with: the 7 per-dimension scores, the resulting band, the override if it fired, and the model recommendation with a one-line reason.

## Recalibration

The rubric and bands are stable; the model column is not. When the available model roster changes, re-run the benchmark behind [COMPLEXITY-MODEL-MATRIX.md](./COMPLEXITY-MODEL-MATRIX.md#routing) and replace that column — never guess a new assignment, and never touch the rubric or bands to compensate for a model change.
