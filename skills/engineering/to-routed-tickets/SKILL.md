---
name: to-routed-tickets
description: Compose to-spec, spec-complexity-routing, and to-tickets to turn a discussed feature into a spec and tickets, with complexity and a recommended model in every ticket. Use when the user wants this combined planning and publication flow.
---

# To Routed Tickets

Run the existing skills in sequence, keeping their instructions and routing matrix as the sources of truth. This skill adds composition and a per-ticket routing step before publication. Its output is a spec and routed tickets; model selection is a recommendation for later implementation.

## Load the dependencies

Read these skills before starting, using user-supplied paths when provided. Otherwise resolve them from the available skill catalog, repository skills, or the personal skills directory. The personal copies used by this workflow are:

- `~/.agents/skills/to-spec/SKILL.md`
- `~/.agents/skills/spec-complexity-routing/SKILL.md`
- `~/.agents/skills/to-tickets/SKILL.md`

Expand `~` to the user's home directory. Read `COMPLEXITY-MODEL-MATRIX.md` relative to the resolved routing skill, following its reference. If a dependency is missing, identify it and stop before publishing incomplete artifacts.

Compose the skills by reading and following them in the current session. Use a native skill invocation only when supported, passing the phase boundary and pre-publication requirements below. A skill marked user-invoked can still supply its instructions through its file; do not depend on an unavailable Skill tool or change invocation settings.

Keep the original skill files, routing matrix, and agent configurations unchanged. Do not create or dispatch agents, implement tickets, or switch the current session's model. Ticket metadata refers only to models.

## 1. Produce the spec

Follow `to-spec` with the conversation and repository context. Preserve its test-seam check and the user's existing approvals. Complete its spec and publication workflow using the configured tracker. Retain the resulting spec body and identifier for the next phase.

On a resumed run, inspect and reuse the spec and tickets already produced for this request instead of creating duplicates.

## 2. Route the spec

Run `spec-complexity-routing` on the completed spec when it has no ticket breakdown yet. Persist its `Complexity & Routing` section with the spec before starting the ticket phase. If tickets already exist, follow the routing skill's preference for evaluating the individual tickets instead.

A spec in band 21 proceeds to decomposition by `to-tickets`, with no model assigned to the spec. The spec's score is planning context, never a default copied into every child ticket.

## 3. Draft, route, and publish tickets

Follow `to-tickets` with this additional instruction supplied before it starts: draft the slices, route every draft individually, then present the breakdown and publish only the final routed bodies.

After its drafting step and before its approval/publication steps:

1. Run `spec-complexity-routing` separately on each draft ticket. Use the current matrix's dimensions, arithmetic, overrides, recommendations, fallbacks, and blocked models without reproducing or inventing a separate matrix here.
2. For a band-21 draft, return to decomposition and score the resulting pieces. Keep it out of the publishable implementation set until every resulting ticket has a permitted implementation band and model recommendation. If a meaningful split requires an unresolved user decision, present the concrete drafts and ask that question.
3. Append the routing result to each ticket body using the required section below. A link to the spec or a label alone does not satisfy this requirement.
4. Include complexity and the recommended model alongside each ticket in the breakdown shown by `to-tickets`. Preserve its approval step and existing authorization. After a merge, split, or scope change, re-score affected drafts and update their bodies before publication.

Resume `to-tickets` publication with the approved bodies, acceptance criteria, dependencies, and configured tracker conventions intact. Its parent-issue protection continues to apply during this phase; spec routing was completed in phase 2.

## Required ticket section

Append one `## Complexity & Routing` section to every ticket, whether a local file or a tracker issue. Include:

- **Dimension scores:** all seven named dimensions with their individual scores and a brief evidence-based rationale.
- **Total:** the sum of the dimension scores.
- **Complexity:** the final Fibonacci band after overrides.
- **Override:** which rule applied, or `None`.
- **Recommended model:** the exact primary model identifier from the resolved matrix.
- **Fallback:** the matrix's fallback identifiers, preserving their order, or `None`.
- **Blocked models:** the restrictions for this band, or `None`.
- **Routing rationale:** a one-line explanation tied to this ticket's work.

Use the ticket's language for field labels while retaining exact model identifiers. Report recommendations as such, without claiming the models are available or have been launched. A runtime availability mismatch must be reported separately; it does not authorize rewriting the matrix or substituting an unlisted model.

## Completion

Read back every published ticket and confirm its body contains the complete routing section for its final scope, with a permitted band and model, as well as its acceptance criteria and correct blocking references. Repair missing metadata in tickets created by this run before reporting completion. For an uncertain publication result, inspect the tracker for the existing ticket before retrying creation.

Return the spec link or path and a compact table of ticket links or paths, complexity, recommended model, and fallback. Report any unpublished or unverified tickets explicitly.
