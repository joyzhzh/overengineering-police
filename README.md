# overengineering-police

A decision-time ruling tool: does this added machinery earn its keep, and if not, what is the
deletion test?

Built for Claude Code. It reviews a specific design against a simpler baseline and returns
`KEEP`, `PROVE`, or `CUT` for each added component, with the evidence or experiment that would
settle the decision. It produces a ruling; applying changes is a separate step.

## Layout

```
SKILL.md                          the procedure (loaded when invoked)
references/evidence.md            claim map, bibliography, contradictions, ceiling, attribution
agent/overengineering-police.md   the independent-reviewer subagent
```

## Install

Requires Git, a macOS/Linux/WSL shell, and [Claude Code](https://code.claude.com/docs/en/overview)
with [skills](https://code.claude.com/docs/en/skills) and
[custom subagents](https://code.claude.com/docs/en/sub-agents). The reviewer defaults to `opus`;
change `model` in `agent/overengineering-police.md` to `inherit` to use your session's model.

Clone into a location you will keep; the installation links to this checkout:

```bash
git clone https://github.com/joyzhzh/overengineering-police.git
cd overengineering-police
```

Then run from this directory:

```bash
(
  src=$PWD
  skill_dst=$HOME/.claude/skills/overengineering-police
  agent_dst=$HOME/.claude/agents/overengineering-police.md
  mkdir -p "$HOME/.claude/skills" "$HOME/.claude/agents"
  for dst in "$skill_dst" "$agent_dst"; do
    if [ -e "$dst" ] && [ ! -L "$dst" ]; then
      echo "REFUSING: $dst exists and is not a symlink — move it aside first" >&2; exit 1
    fi
  done
  ln -sfn "$src" "$skill_dst" &&
  ln -sfn "$src/agent/overengineering-police.md" "$agent_dst" &&
  ls -ld "$skill_dst" "$agent_dst"
)
```

The guard matters: `ln -sfn` onto an existing *real* directory silently creates the link **inside**
it and exits 0, leaving no `SKILL.md` at the path the loader checks — a mis-install with no error
anywhere. `-n` suppresses symlink dereference; it does nothing for a genuine directory. The `ls -ld`
must print two `->` arrows.

Start a new Claude Code session after installation, then invoke the skill or delegate to the
`overengineering-police` subagent.

To uninstall, move the two symlinks to your system trash, leaving the checkout in place.

## Use

For a design you did not build in the current session:

```text
/overengineering-police Review the planner and retry loop in src/workflow.py against direct tool calling. Our target is at least 95% task success within 10 seconds.
```

For work produced in the current session:

```text
Use the overengineering-police subagent to review the design in docs/proposal.md. Compare it with one tool-using agent and identify the evidence needed to justify each extra component.
```

Supply the relevant files, desired outcome, constraints, and any measurements you have. Missing
evidence normally yields `PROVE` with a proposed test. A ruling includes the boundary, baseline,
ranked component ledger, deletion tests, what earns its keep, and remaining uncertainty.

## Why both a skill and an agent

Not redundancy. The skill is the procedure; the agent is the *independence*.

The evidence base's own confusion negative is "an evaluator agreeing with its generator treated as
independent validation." When the main thread designed the thing under review, invoking the skill
in-thread grades its own work. The subagent has a separate context, no write tools (`Read, Grep,
Glob` only), and a posture prompt that refuses the producer's framing. That is a structural
property, not a stylistic one — which is the only reason it earns a second file.

**What that buys, precisely.** A separate context reduces producer-context contamination. It is an
adversarial second pass, *not* independent empirical validation: it may share model-family biases
with the thread that produced the target. For a load-bearing ruling, use a different engine.

Use the skill directly when reviewing someone else's work. Use the agent when reviewing your own.

## Design constraints it holds itself to

- **Decision-time, not standing.** Deliberately *not* a hook or a lint. Replit reported that stacking
  reminders even at decision time could reduce compliance (`OWE-C031`). This skill chooses
  selective use as a design judgment; that observation does not establish that all standing
  controls are harmful.
- **Ranks, never screens.** Every component appears in the ledger, ordered by unjustified burden.
  Ranking aims attention; it does not cull.
- **Rules, does not rewrite.** The agent has no write tools by construction.
- **Rules on partial boundaries.** `CANNOT RULE` is reserved for targets where neither the outcome
  nor any credible baseline can be stated. Otherwise a missing fact marks the affected component
  `PROVE` and the ruling proceeds — refusing to rule on a real design is a failure, not rigour.
- **Bounded by its own budget.** If the ruling costs more than the machinery it judges, cut the
  ruling.

## Evidence and attribution

The empirical content belongs to 35 public works — papers at arXiv, ACL Anthology, NeurIPS and
OpenReview; Inspect AI documentation; and first-party engineering posts from Anthropic, Microsoft
and Replit. All are listed with locators in
[`references/evidence.md`](references/evidence.md#bibliography).

The *structure* is borrowed too, and named as such — including the skill's central three-receipt
rule, which is not original. That, the nine-row cost table, the procedure skeleton, the
contradictions list and the confusion negatives are all adapted from a prior evidence campaign on
agent-workflow over-engineering (evidence cutoff 2026-08-24). Full attribution table:
[`references/evidence.md`](references/evidence.md#attribution).

Two limits travel with all of it:

1. **Second-hand figures.** Numbers were transcribed from that campaign's claim ledger and have
   not been comprehensively re-verified against primary sources. A publication spot-check
   corrected `OWE-C031`; the remaining figures retain their second-hand status. Open the paper
   before a figure changes a decision.
2. **A bounded ceiling.** The campaign labels itself a high-coverage, effort-bounded map as of
   2026-08-24 — *not saturated*, not a prevalence estimate, not a universal architecture
   recommendation. Cites are evidence that a question is live. They are not laws, and they are not
   grounds to cut a component on their own.

## Sharing

The shareable surface — `SKILL.md`, `references/evidence.md`, `agent/`, this file — excludes
absolute home paths, private repository identifiers, and internal commit or bundle identifiers.
Source-campaign provenance is excluded deliberately; its absence costs traceability, not
grounding, because the public bibliography stands on its own. Public commits use a GitHub noreply address for author and committer metadata.

## License

[MIT](LICENSE). The license covers this repository's skill, agent prompt, and documentation.
Cited third-party works retain their own terms; the bibliography and attribution remain in
[`references/evidence.md`](references/evidence.md#attribution).
