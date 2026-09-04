## What it does

`to-routed-tickets` turns an already-discussed feature into a published spec and a set of tracer-bullet tickets. Every ticket records its complexity, recommended implementation model, fallback, and [reasoning effort](https://www.aihero.dev/ai-coding-dictionary/effort).

You invoke the combined flow once. It runs `to-spec`, applies `spec-complexity-routing`, drafts tickets, routes each ticket separately, and then completes the review and publication steps from `to-tickets`.

## When to reach for it

Type `/to-routed-tickets`, or the agent reaches for it automatically when a discussed feature needs both a spec and implementation tickets with routing metadata. Use [to-spec](https://aihero.dev/skills-to-spec), [spec-complexity-routing](https://aihero.dev/skills-spec-complexity-routing), or [to-tickets](https://aihero.dev/skills-to-tickets) directly when you only need that one stage.

## Prerequisites

The repository needs an issue tracker configured by [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills). The discussion must already contain the product and implementation decisions that `to-spec` will synthesize.

## One entry, three stages

`to-routed-tickets` is the entry point. Its internal sequence is `to-spec` → `spec-complexity-routing` → `to-tickets`. Routing runs again for each ticket draft because tickets from one spec can require different models and efforts.

## Common questions

**Do I run to-spec first?**

No. Invoke `to-routed-tickets` with the discussion. The skill creates the spec as its first stage.

**Does it create or launch agents?**

No. The ticket metadata recommends an implementation model and reasoning effort for later use.

**Does it choose the code-review model?**

No. Its routing metadata applies to ticket implementation.

**What happens if a ticket scores 21?**

That draft is decomposed and the resulting tickets are scored again before publication.

## It's working if

- One invocation produces the spec and approved tickets.
- Each final ticket has its own complexity, model, fallback, and reasoning effort.
- Changed or split ticket drafts are scored again.
- No band-21 implementation ticket is published.

## Where it fits

This is the combined planning step between [grill-with-docs](https://aihero.dev/skills-grill-with-docs) and [implement](https://aihero.dev/skills-implement). See [ask-matt](https://aihero.dev/skills-ask-matt) for the complete route and its branches.
