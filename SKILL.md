---
name: overengineering-police
description: Rule on whether a specific added component earns its keep against a named simpler baseline, and name the deletion test for what does not. Use for an explicit over-engineering ruling on a nontrivial target - deciding single-agent vs multi-agent, whether a planner, retry loop, reflection pass, evaluator, memory/RAG layer, guardrail or tracing layer is worth its cost, or whether an abstraction, config surface, gate or schema should exist. Do not use for ordinary code review, small deterministic scripts, or targets with no separately deletable added machinery.
---

# Over-engineering police

> Adapted work, not original research. **The three-receipt rule below is itself borrowed**, along with
> the cost table, the procedure skeleton, the contradictions and the confusion negatives - all from a
> prior evidence campaign on agent-workflow over-engineering. The empirical claims belong to 35
> public sources. Attribution table and bibliography: `references/evidence.md`.

## The one rule

Added machinery is **unproven until shown otherwise**. It earns its keep against one of three receipts:

- **Bottleneck** - it removes a bottleneck someone actually measured.
- **Failure mode** - it contains a named failure that has occurred, or that a stated threat/error model predicts.
- **Frontier** - it moves a *predeclared* outcome- or risk-per-cost frontier against the simplest safe baseline.

The burden of proof sits on the addition, never on the deletion. Count, depth, volume, coverage and
presence are not benefits: more agents, more roles, deeper plans, larger memory, more retries, higher
evaluator agreement, more tracing, more guardrails, richer schema - none is evidence of anything by
itself.

**Scope of the rule.** This is a rebuttable decision heuristic for a specific target, not a universal
architecture law. Its evidence base covers *agent workflows only*. Applying it to ordinary code and to
process machinery is this skill's own design judgment and carries no citation - useful, but do not
present it as an evidenced finding.

## When to run this

At a decision point, on something specific, by someone who did not build it.

- **Decision time, not standing.** Replit reported diminishing or negative returns after stacking a third or fourth reminder *at decision time*. It advocates selective guidance; placement and caching changed tool-use and cost proxies, without a quantified end-quality comparison (`OWE-C031`, `OWE-C032`). Keeping this skill out of an always-on lint is a design judgment, not a result that this account proves.
- **Not the producer.** An evaluator agreeing with its generator is not independent validation. If you designed the target, delegate to the `overengineering-police` subagent.
- **Not on trivia.** If the target has no separately deletable machinery carrying material cost or risk, emit `NOT APPLICABLE - no architecture decision to police` and stop. A 40-line script does not need a ruling.

## Procedure

### 1. State the boundary that could change the decision

Outcome, acceptable error, tool/permission surface, reversibility, time horizon, model + version -
**only the fields that could materially move a verdict here.** Do not demand irrelevant fields.

Then pick one of three, in this order:

- **`NOT APPLICABLE`** - no separately deletable machinery carrying real cost. Stop.
- **`CANNOT RULE`** - neither the outcome nor any credible baseline can be stated. Say what is missing, stop. Reserve this for genuine emptiness.
- **Otherwise, rule.** A missing fact does not block the whole ruling: mark the *affected component* `PROVE` and name the fact that would settle it. Partial boundaries are normal; refusing to rule on a real target is a failure, not rigour.

### 2. Name the baseline that has to lose

Every added component is implicitly beating something simpler. Name it concretely: one direct call, a
deterministic script, one tool-using agent with a bounded loop; the inlined version at the call site;
the step done by hand once with the result written down.

If nobody ever ran the baseline, say so before anything else - that is usually the headline.

**Never require running an unsafe baseline.** For safety, containment or reversibility machinery, the
comparison is against the *simplest safe* control, not against no control.

### 3. Inventory the components as separately deletable units

An agent, a role, a message edge, a planner, a retry policy, an evaluator, a memory store, a
guardrail, a trace sink, an abstraction, a config knob, a gate, a schema field, a ceremony step.

Granularity is the whole trick. "The framework" is not a unit; "the reflection pass" is. Treat
messages, edges and roles as ablatable units (`OWE-C047`, `OWE-C048`).

### 4. Demand a receipt per component

- Which receipt - bottleneck, failure mode, or frontier?
- What evidence, at what scope?
- **What counterfactual would show it is not needed?** If nobody can state one, the component is unfalsifiable and cannot be `KEEP`.

### 4b. Name what already does this job

*Uncited instrument-design check - no evidence base supports this step.*

Components get justified one at a time and never against each other, which is how quality machinery
stacks. For each unit, name every *other* thing in the supplied target that appears to hold the same
job - another component, a human gate, a test suite, a type system, a tool's own error handling.
Record control surfaces you cannot see in `RESIDUAL`.

**Do not collapse components by label.** A critic, a judge, a policy layer and a human reviewer may
address genuinely different failure modes; detection, policy, authorization and human control are
distinct jobs even when they all look like "quality". Overlap has to be shown, not assumed.

Where target-specific evidence does show real overlap, compare against the cheapest sufficient unit;
each additional unit then needs a receipt for its distinct *marginal* catch at acceptable cost, not
for the job itself.

### 5. Charge the full cost

Charge every row that applies. Do not instantiate irrelevant ones - and do not stop at tokens.

| Cost | Charge | Do not accept |
|---|---|---|
| Outcome | Task success, calibrated quality, usable grounded answer | Longer traces or more tool calls as quality |
| Compute | Realized in/out/cache tokens, calls, retries, dollars, one-time setup | Nominal caps instead of actual use |
| Latency | Wall time, sequential depth, TTFT, blocked time | Latency inferred from component count |
| Coordination | Agents, roles, messages, edges, handoffs, duplicated work, context re-encoding | A persona label counted as an agent |
| Reliability | Failure class, variance, unusable outputs, retry tail, recovery time | Retries erasing first failures; means without tails |
| Memory/state | Retrieval precision, conflict/update handling, abstention, staleness, provenance | Retrieval volume or store presence as value |
| Security | Benign utility, utility under attack, attack success, false positives, permission surface | Guardrail documentation as deployed containment |
| Maintenance | Component/config count, change amplification, debug time, removal path | Inference spend only, ignoring operator burden |
| Human control | Approval, override, drop/rollback decision, residual owner | Evaluator agreement as independent validation |

### 6. Rule, and bind the ruling

Default status is **unproven** - which is not the same as `CUT`.

- **KEEP** - target-specific evidence clears a predeclared outcome- or risk-per-cost threshold against the simplest safe baseline; costs charged, falsifying counterfactual recorded.
- **PROVE** - plausible but unverified. Name the safest decisive test and when the answer expires. **An unrun baseline lands here, not in `CUT`.**
- **CUT** - a safe comparison shows deletion stays within tolerance, or no credible mechanism connects the component to the claimed benefit, or charged cost exceeds demonstrated benefit.

Bind each verdict **only to the factors that could flip it** - task, model/version, scale, threat
model, price - and state what change would void it. Controls can go stale: one first-party account
describes a context-reset control that helped one model and became dead weight on the next
(`OWE-C034`). That is an illustration, not a universal expiry law.

### 7. Name the deletion test

For every `CUT` and `PROVE`: what gets removed, what replaces it, the **performance tolerance declared
in advance**, and how you will read the result.

Do not minimize blindly. In one study over-pruning lost up to 20.8%, which is why the tolerance is
declared before the cut, not after (`OWE-C048`).

## Output

Short. Ranked, never screened - ordered by unjustified burden (cost carried × weakness of receipt).
Nothing is dropped for scoring low; ranking aims attention, it does not cull. Every *materially
distinct* component appears; repeated units may be grouped only when they share the same job, receipt,
cost profile and deletion test - record the group's cardinality.

When only an opaque bundle count is supplied ("60 policy rules"), record the bundle once with its
cardinality, mark every unnamed unit `PROVE`, and require itemization before any of them can reach
`KEEP` or `CUT`. A shared count is not evidence of a shared job.

```
BOUNDARY   only the fields that could move a verdict here
BASELINE   the simpler thing that must lose - and whether it was ever run

LEDGER     (ranked by unjustified burden)
  component | claimed job | known overlap / marginal catch | receipt | cost charged | VERDICT | binding

DELETION TESTS   what to remove, tolerance declared up front, how to read it
EARNED           what survives, and why
RESIDUAL         what stays unknown, and who owns that risk
```

## Smells

Each smell is a *prompt to demand a receipt*, never an automatic `CUT`. Bounded claim map, scope notes
and primary-source locators: `references/evidence.md` - consult the linked source before a number
changes a decision.

### Agent and workflow structure

| Smell | Cheaper move to try first | Cites |
|---|---|---|
| Several agents/roles over one homogeneous model | One call carrying the same control flow | `OWE-C001` `OWE-C002` `OWE-C003` `OWE-C004` |
| Debate / committee / mixture for accuracy | Self-consistency at the *same* budget | `OWE-C016` `OWE-C038` `OWE-C018` |
| Reflection or self-critique with no external signal | External test, tool, environment state, or human | `OWE-C041` `OWE-C019` |
| Retry loop with no budget and no tail reported | Plain warm retry; report the worst case, not the mean | `OWE-C021` `OWE-C022` |
| Planner layer in front of function calling | Run direct function calling as the baseline. One vendor example reported ~1,380 vs ~240 tokens, but supplied no controlled before/after | `OWE-C024` `OWE-C025` `OWE-C027` |
| Memory / RAG layer added "for context" | No-memory context-only baseline; test memory *by function* | `OWE-C049` `OWE-C054` `OWE-C053` |
| More agents assumed to scale | Non-monotone: communicating designs carried 263-515% coordination overhead in one matched-compute study | `OWE-C011` `OWE-C012` `OWE-C009` |
| More roles assumed to help | Role value flips sign by model and task | `OWE-C040` `OWE-C010` `OWE-C050` |
| Guardrail layer credited by its existence | Charge benign utility loss and false positives too | `OWE-C043` `OWE-C044` |
| Tracing/observability added for completeness | Name the stop/drop/rollback decision it changes | `OWE-C036` `OWE-C042` |
| Stacked prompt reminders | Select only relevant guidance at the decision; timing alone is insufficient | `OWE-C031` `OWE-C032` |
| Benchmark win credited to orchestration | Check whether a stronger model or bigger budget explains it | `OWE-C014` `OWE-C055` |

### Adjacent domains — uncited design judgment

Nothing below is supported by this skill's evidence base, which covers agent workflows only. These are
recognition cues, not findings. Weigh them accordingly.

**Code.** An abstraction with one call site (inline it; extract at the third, or at a named second
consumer). A config knob for a value that never varies. Error handling for a state that cannot occur.
A wrapper that only renames. "Flexibility" nobody asked for. A layer added so code "reads better" -
readability is a claim about a reader, so name them.

**Process machinery.** A validator, schema or ontology built before the artifact has a user. A gate
whose failure has never changed an outcome. A ledger field nobody has queried. A handoff doc
duplicating a handoff doc. A reviewer who is the producer. Setup expanding until it displaces the
work.

Governance can be earned three ways: a documented incident, a credible threat/error model with
material consequence, or target-specific evidence that the control catches or contains the failure at
acceptable cost. Ask which one, and for the first, ask for the failure by name and date.

## Positive boundary cases

These are documented cases where added structure paid, **not** category-level receipts. Do not cut
them on aesthetic grounds; still require a target-specific failure model and net-benefit test.

- **Breadth-first parallel search** over genuinely independent subtasks - large reported win at roughly 15× chat tokens; explicitly a *poor* fit for tightly dependent or shared-context work (`OWE-C055`, `OWE-C056`).
- **Verify-then-escalate cascade** - cheap verifiable path first, escalate on failure; conditional on cheap verification and a large cost gap (`OWE-C008`).
- **A lightweight budget tracker** - in that study's search-agent implementations and price mapping, comparable reported accuracy at tool budget 10 vs 100 at lower unified cost. The *light* tracker, priced separately from the full planning framework (`OWE-C062`, `OWE-C063`).
- **Rollback and environment separation** following a real destructive incident (`OWE-C028`, `OWE-C029`, `OWE-C030`).
- **Durable state / sandbox / credential separation** on long horizons (`OWE-C035`).
- **A human launch gate** on consequential loops, with a drop/rollback path and a named residual owner (`OWE-C033`).
- **Sparse-but-not-minimal topology** pruned against a declared tolerance (`OWE-C047`, `OWE-C048`).

## Police the police

- Decision-time tool. Not a hook, not a gate, not a standing instruction.
- It ranks; it does not screen. Nothing is dropped from the report for scoring low.
- It rules; it does not rewrite. Deletion tests go back to the producer, who decides.
- It rules on real targets. `CANNOT RULE` is for genuine emptiness, not for a missing field it could have marked `PROVE`.
- Its own budget: for a small target, one screen. If the ruling costs more than the machinery it judges, cut the ruling.
- **The evidence is bounded.** A high-coverage, effort-bounded map as of 2026-08-24, explicitly not saturated, not a prevalence estimate, not a universal architecture recommendation. Each `OWE-C` cite is one study at one scope - evidence that a question is live, never a law, and never grounds on its own to cut a component.
- **The figures are second-hand.** Transcribed from a claim ledger, not re-verified against the papers here. Before a number changes a decision, open the primary source.
