# Evidence base for the over-engineering police

Every `OWE-Cnnn` cite in `SKILL.md` resolves here, and every row here resolves to a numbered public
source in the [Bibliography](#bibliography).

## Read this before quoting any number

**Chain of custody.** These figures were transcribed from a structured evidence ledger produced by a
prior research campaign that inspected the primary sources. They have **not** been independently
re-verified against the papers by this skill. Treat every number as second-hand until you open the
source in the bibliography. Where a figure would change a real decision, go read the paper.

**Publication spot-check (2026-09-12).** `OWE-C031` was checked against source `[30]` and corrected:
the third/fourth-reminder observation concerns stacked decision-time reminders. This was not a
comprehensive re-verification of the ledger.

**Ceiling.** The claim label carried by the source of these figures is *high-coverage, effort-bounded map as of
2026-08-24; **not saturated**; working plateau **not** established*. It is explicitly **not** a
systematic review, **not** a prevalence estimate, **not** runtime validation of any software, and
**not** a universal architecture recommendation. One planned survey/bibliography frame was not completed. English-language
public sources only.

Consequence for this skill: every cite below is a **bounded observation at a stated scope**. Cite it
as evidence that a question is live and must be asked. Never as a law, never as a default, never as
proof that a component should be cut. Syntheses in this file are derived from the cited primary-source
observations; they are not independent observations.

**Coverage of this file.** 55 claims cited, drawn from 35 distinct works. The source ledger holds 63
claims across 41 works; the 8 uncited claims and 6 uncited works were not needed by `SKILL.md` and are
not reproduced here. Nothing was dropped for disagreeing.

## Claim map

Column 3 gives the boundary that must travel with the number, and `[n]` points at the bibliography.

### Single agent vs. multi-agent, at matched budget

| Cite | Observation | Scope / limit |
|---|---|---|
| `OWE-C001` | Under homogeneous-workflow assumptions one LLM can simulate separate-agent control flow while reusing shared KV state; six-benchmark tables report comparable or slightly higher scores for single execution | Assumes homogeneous model, visible shared state, deterministic tools/routing. Author study of its own method `[1]` |
| `OWE-C002` | Most paired single-execution variants use fewer token-cost dollars (HumanEval 0.020 vs 0.026; GSM8K 0.387 vs 0.623; MATH 0.677 vs 0.819) | Study-specific models and prices; idealized cache cost; not total cost of ownership `[1]` |
| `OWE-C003` | Across FRAMES and 4-hop MuSiQue, four backbones, most requested thinking budgets: the single call is best or statistically overlapping the best multi-agent variant, usually using less actual thinking | Requested caps are not actual-token equality `[2]` |
| `OWE-C004` | **Counter-condition:** several multi-agent variants win at the 100-token regime; Gemini 2.5 Pro sometimes favours debate or sequential structure; ensemble can be strongest at high FRAMES budgets | Keeps `OWE-C003` conditional — do not cite C003 without C004 `[2]` |
| `OWE-C005` | With loader, model, tools, evaluator, answer contract, usage accounting and logging aligned: single anchor 74.12%; EvoAgent 75.56%; five other multi-agent wrappers 62.83–71.56%, generally using more tokens | Substrate authored by the evaluators `[3]` |
| `OWE-C006` | Latency is **not** monotone in agent count — several weaker multi-agent wrappers were faster than the stronger single anchor | Measure latency; never infer it from structure `[3]` |
| `OWE-C007` | Reported multi-agent gains shrink substantially under Gemini 2.0 Flash; MAS/SAS prefill-token ratios span 1.20×–220.22×, decode 2.10×–21.64× | Model-bound `[4]` |
| `OWE-C008` | **Positive case:** a verify-then-escalate cascade beat always-single and always-multi while spending fewer normalized tokens than always-multi | Conditional on cheap verification, a large cost gap, tolerable failed-attempt overhead `[4]` |

### Scaling, topology, coordination tax

| Cite | Observation | Scope / limit |
|---|---|---|
| `OWE-C009` | Agent-count effects are non-monotone and model/task bound; small models degrade with added agents; several eight-agent rows fall far below one-agent accuracy | `[5]` |
| `OWE-C010` | Fixed-context padding weakens a pure long-context explanation; profile deletions change **sign** by model and task rather than yielding one necessary role set | No universal role set exists `[5]` |
| `OWE-C011` | Across 260 configurations, 5 architectures, 6 benchmarks, 9 models at ~4,800 reasoning tokens/trial: multi-agent effects range **+57% to +80.8%** (Finance-Agent) down to **−39% to −70%** (PlanCraft) | Task-conditioned, both directions. Cite both halves `[6]` |
| `OWE-C012` | Coordination overhead rises 58% (Independent) to **263–515%** (communicating); success per 1k tokens falls 67.7 (single) to 13.6 (Hybrid); an orchestrator reduces some error classes while Hybrid adds coordination failures | Architectures trade failure families `[6]` |
| `OWE-C013` | Decomposability, tool intensity, single-agent baseline strength and error containment are stronger selection variables than agent count | Cross-validated fits R² 0.373 / 0.413 — modest `[6]` |
| `OWE-C047` | Some star/tree topologies *reduce* performance; AgentPrune reports comparable results at $5.6 vs $43.7 with 28.1–72.8% token reduction; one five-agent optimization itself cost $234.76 and 17M tokens | The optimizer's own cost is real and often uncharged `[7]` |
| `OWE-C048` | **Counter-condition:** oversimplification can lose up to 20.8% — ablate against a **predeclared tolerance**, do not minimize blindly | The single most important brake in this file `[7]` |
| `OWE-C050` | Accountability placement changes outcomes only when the delivery path reaches the accountable agent; the winning placement **flips across model families** | `[8]` |
| `OWE-C052` | **Counter-condition:** directed-acyclic agent networks support larger teams; irregular/small-world topologies beat regular ones; logistic scaling curve | Growing-compute regime, unlike `[6]`'s matched-compute regime `[9]` |
| `OWE-C039` | A failure taxonomy identifies 14 failure modes across system design, inter-agent misalignment, and verification/termination | Architectures trade one failure family for another `[10]` |
| `OWE-C040` | A specialized three-role intervention gave only a small non-significant GPT-4 gain (p=0.4) but significant GPT-4o gains (p=0.03) | Role value is model-bound `[10]` |

### Planning, reflection, retries, evaluators

| Cite | Observation | Scope / limit |
|---|---|---|
| `OWE-C014` | Repeated sampling up to 40 raises scores on several static benchmarks — but the mechanism is self-ensemble / test-time compute, **not** coordination; tokens grow proportionally | The canonical "more agents" title that is not about coordination `[11]` |
| `OWE-C016` | Across five datasets at 1–20-query budgets, CoT self-consistency often beats multi-agent debate and Reflexion **at matched budgets**; debate can plateau after ~6 queries and added compute can *reduce* performance | `[12]` |
| `OWE-C017` | Evaluator quality matters: weakened Tree-of-Thought evaluation lowers results; oracle-stopped Reflexion must not be compared with deployable non-oracle policies | Evaluator and stopping variants used in this study; no prevalence claim `[12]` |
| `OWE-C018` | **Counter-condition:** at comparable modeled compute, tuned debate and mixture-of-agents are +1.3 and +2.7 points over self-consistency on MMLU-Pro — but **below ~3× CoT, self-consistency is the viable option** | Directly contradicts `OWE-C016` above a threshold `[13]` |
| `OWE-C019` | Self-refinement stays below CoT; debate peaks around four agents / two rounds; weaker proposers can *reduce* mixture-of-agents accuracy versus deleting them | Local optima, then harm `[13]` |
| `OWE-C020` | Reflexion raises HotPotQA and FEVER accuracy at ~5× ReAct tokens **under optimistic ground-truth stopping**. The same study reports a cheaper positive: TCAR earned a GSM8K gain by reusing one trajectory | The stopping rule is doing much of the work `[14]` |
| `OWE-C021` | The most expensive failures exhaust seven trials and approach **197,386 tokens**; rare unique solves coexist with a severe heavy-cost tail | Report the tail, not the mean `[14]` |
| `OWE-C022` | Simple warming/retry baselines match or nearly match the best complex agents on test-equipped HumanEval at far lower cost; one tree-search agent is over **50×** costlier than warming | Historical prices; requires tests to exist `[15]` |
| `OWE-C041` | Intrinsic self-correction without external feedback often failed and sometimes **degraded** performance; external feedback is preserved as the positive boundary | Studied reasoning tasks, not all tasks `[16]` |
| `OWE-C042` | Across 165 traces at identical caps, unusable final answers were 22/53, 33/86 and 12/26 by level while mean tokens rose 8,152 → 16,389 | Waste = repeated actions, step caps, tool failures, missing evidence `[17]` |
| `OWE-C062` | **Positive case:** a lightweight budget tracker added to ReAct improved reported accuracy for Gemini 2.5 Pro/Flash and Claude Sonnet 4 under the same tool budgets | `[18]` |
| `OWE-C063` | **Positive case:** budget tracker at tool budget 10 reported comparable accuracy to ReAct at budget 100 with 40.4% fewer search calls, 19.9% fewer browse calls, 31.3% lower unified cost. The full framework *additionally* adds planning/verification — price the light tracker and the full framework **separately** | Search tasks, provider prices, three-run averages, limited tuning; trajectory-context controls confound pure tracker attribution `[18]` |
| `OWE-C024` | Maintainers report removing a Markdown package for lack of use and Math/Wait plugins for lost relevance, consolidating common code, and **announcing discontinuation of planner packages** in favour of function calling | First-party maintainer self-report `[19]` |
| `OWE-C025` | **Limit:** that page supplies no measured before/after — the rationale and implementation are evidence; the benefit is unverified | Cite C024 only with C025 `[19]` |
| `OWE-C027` | One date/weather example: ~1,380 tokens with a stepwise planner vs ~240 with automatic function calling, for materially the same answer | A single worked example, not a benchmark `[20]` |

### Memory, RAG, context

| Cite | Observation | Scope / limit |
|---|---|---|
| `OWE-C049` | A memory benchmark separates accurate retrieval, conflict resolution, test-time learning and long-range understanding under incremental interactions — making memory value falsifiable **by function**, not by retrieval volume | The test to demand before keeping a memory layer `[21]` |
| `OWE-C054` | 500 curated questions over extraction, multi-session, temporal, knowledge-update and abstention; evaluated systems drop ~30% accuracy under sustained interaction; indexing/retrieval/reading choices matter | `[22]` |
| `OWE-C053` | Context is a finite attention budget; indiscriminate preload creates pollution and stale indexes; rigid prompts are brittle; prefer smallest high-signal context with just-in-time retrieval or compaction | First-party engineering guidance, not a controlled study `[23]` |
| `OWE-C034` | A context-reset control useful for one model became **dead weight** for a later model | One first-party transition; illustrates why model-bound controls may need revalidation. Does not establish a universal expiry rule `[24]` |
| `OWE-C035` | **Positive case:** separating durable session log, stateless harness, sandbox/tools and credential vault; lazy sandbox provisioning reportedly cut p50 time-to-first-token ~60% and p95 >90% | First-party self-report `[24]` |

> *Useful Memories Become Faulty When Continuously Updated by LLMs* was identified but never fully
> inspected, and is deliberately not used as evidence here. Treat continuous memory consolidation
> as an open question, not a settled one.

### Security, guardrails, governance

| Cite | Observation | Scope / limit |
|---|---|---|
| `OWE-C043` | 97 user tasks and 629 security cases; evaluated defenses lose roughly **15–20% utility under attack**, and injection detection introduces false positives that reduce *benign* utility | Makes attack-time and benign utility both part of the measured frontier `[25]` |
| `OWE-C044` | Preselected tool filtering drops attack success to 7.5% but **fails** when tools cannot be planned ahead or when benign and malicious goals need the same tool — 17% of suite cases | The cheap control has a named blind spot `[25]` |
| `OWE-C045` | A deterministic two-firewall tool interface reports strong utility/security across four suites **and** identifies bugs, weak attacks and flawed success metrics in earlier benchmarks | Abstract-bounded inspection `[26]` |
| `OWE-C046` | In a small reproduction, mean attack success falls 25.8% → 4.2% — but a handcrafted adaptive attack stays at 2.6%; static benchmarks can **overstate** defense strength | Small reproduction; abstract-bounded `[27]` |
| `OWE-C028` | Implemented filesystem/database checkpoints, a separately stored append-only Git remote, dev/prod database separation, agent access limited to development, discardable isolated forks | First-party `[28]` |
| `OWE-C029` | The vendor states its agent **deleted application-database data** and rollback restored it with no data loss — and that the agent did not know the rollback facility existed | The clearest documented reversibility case here. A single self-reported incident; no measured incident-rate reduction, and production remained reachable during recovery `[29]` |
| `OWE-C030` | Reported shipped mitigations: default dev/prod separation, denial of dev-agent changes to production, documentation-lookup prompting, proactive rollback surfacing | Self-reported, not audited prevention rates `[29]` |
| `OWE-C031` | Stacking a third or fourth reminder **at decision time** produced diminishing or negative returns in early experiments. The proposed system selects relevant, ephemeral guidance and prompts consultation of another model for stuck/high-risk trajectories | First-party account; supports selectivity, not a general finding against standing controls. Corrected in the publication spot-check `[30]` |
| `OWE-C032` | Moving a parallel-tool instruction to the trace bottom reportedly produced ~15% more tools per loop; keeping the core prompt cacheable reportedly cut cost **90%** versus dynamic system-prompt modification | Placement and cacheability are real levers `[30]` |
| `OWE-C033` | **Positive case:** separate production-derived benchmarks, A/B tests and clustered failure trajectories; the loop recommends ship / iterate / drop while **engineers retain launch approval** | Human gate with an explicit drop path `[31]` |
| `OWE-C036` | Evaluation logs expose outcome/status, errors, tokens, timing, retries, message/turn counts, run configuration and reduction fields — and instruct consumers to check successful status before analyzing results | A measurement surface, not a validation `[32]` |
| `OWE-C037` | Version 0.3.206 added deduplication and zstd compression for long-horizon log growth, including documented O(N²) repeated-message storage | Observability has its own scaling cost `[32]` |

### Where multi-agent structure earned it

| Cite | Observation | Scope / limit |
|---|---|---|
| `OWE-C055` | An orchestrator-worker research system reportedly outperformed a single strong agent by **90.2%** on an internal evaluation — while **tokens explained ~80% of performance variance** | Read both halves. Internal evaluation, first-party `[33]` |
| `OWE-C056` | Same account: roughly **15×** chat token use; fit is breadth-first parallel research; explicitly a **poor fit** for shared-context or tightly dependent tasks; commonly 3–5 subagents; up to 90% time reduction on suitable work | The sharpest earned-complexity boundary in the set `[33]` |
| `OWE-C026` | Guidance recommends the smallest sufficient solution, notes agentic systems trade latency and cost for performance, warns frameworks can obscure prompts and debugging, and reserves parallel structure for decomposable or independent-perspective tasks | First-party guidance, not a controlled study `[34]` |
| `OWE-C038` | Default multi-agent debate did not reliably beat self-consistency or multi-path ensembling on the evaluated tasks; tuned multi-persona variants sometimes did | Abstract-bounded `[35]` |

## Contradictions to keep visible

Do not average these away. If a ruling depends on one side, say which side and why.

1. **Fixed vs. growing compute** — matched-total-token work finds broad overhead and task-conditioned harm (`OWE-C011`–`OWE-C013`, `[6]`); large-DAG work reports positive scaling as compute grows (`OWE-C052`, `[9]`).
2. **Debate/mixture vs. self-consistency** — self-consistency often wins at matched budgets (`OWE-C016`, `[12]`); tuned debate/mixture wins beyond a compute threshold (`OWE-C018`, `[13]`).
3. **Reflection vs. simple retry** — reflection buys unique hard-case solves at ~5× tokens with oracle stopping (`OWE-C020`, `OWE-C021`, `[14]`); simple warming/retry is Pareto-competitive where tests exist (`OWE-C022`, `[15]`).
4. **Simplify vs. isolate** — stacked reminders can interfere and model-bound controls can become stale (`OWE-C031`, `[30]`; `OWE-C034`, `[24]`); durable state, sandbox, credential, rollback and approval boundaries earn their interfaces under long-horizon or high-risk conditions (`OWE-C035`, `[24]`; `OWE-C029`, `[29]`; `OWE-C033`, `[31]`).
5. **Simple security vs. adaptive threats** — deterministic filtering can outperform elaborate in-band controls in bounded suites (`OWE-C045`, `[26]`); dynamic shared tools, false positives, benchmark flaws and adaptive white-box attacks preserve open risk (`OWE-C044`, `[25]`; `OWE-C046`, `[27]`).

## Confusion negatives

Reject these as arguments, from anyone, including yourself. *(List adapted from the source campaign's
own "confusion negatives" — see [Attribution](#attribution).)*

- More tokens or longer traces treated as better reasoning.
- More agents treated as more capability without matched task, model, tool and budget controls.
- A role label or prompt persona treated as an independent agent or an architecture change.
- Self-consistency sampling, model ensembles and multi-agent coordination silently conflated.
- Better results from a stronger model or a larger budget attributed to orchestration.
- A framework feature list treated as evidence that every layer should be enabled.
- Repository modularity, stars or code volume treated as performance or maintainability.
- An evaluator agreeing with its generator treated as independent validation.

## Bibliography

All public. Verify any figure here against the source before it changes a decision.

**Controlled and matched comparisons**

1. *Rethinking the Value of Multi-Agent Workflow: A Strong Single Agent Baseline* — arXiv:2601.12307 — https://arxiv.org/abs/2601.12307
2. *Single-Agent LLMs Outperform Multi-Agent Systems on Multi-Hop Reasoning Under Equal Thinking Token Budgets* — arXiv:2604.02460 — https://arxiv.org/abs/2604.02460
3. *Do More Agents Help? Controlled and Protocol-Aligned Evaluation of LLM Agent Workflows* — arXiv:2606.05670 — https://arxiv.org/abs/2606.05670
4. *Single-agent or Multi-agent Systems? Why Not Both?* — arXiv:2505.18286 — https://arxiv.org/abs/2505.18286
5. *Scaling Behavior of Single LLM-Driven Multi-Agent Systems* — arXiv:2606.00655 — https://arxiv.org/abs/2606.00655
6. *Towards a Science of Scaling Agent Systems* — arXiv:2512.08296 — https://arxiv.org/abs/2512.08296 (author-institution summary: https://research.google/blog/towards-a-science-of-scaling-agent-systems-when-and-why-agent-systems-work/)
7. *Cut the Crap: An Economical Communication Pipeline for LLM-based Multi-Agent Systems* (AgentPrune) — arXiv:2410.02506 — https://arxiv.org/abs/2410.02506
8. *Organizational Science of Multi-Agent LLM Systems* — arXiv:2607.25446 — https://arxiv.org/abs/2607.25446
9. *Scaling Large Language Model-based Multi-Agent Collaboration* (MacNet) — arXiv:2406.07155 — https://arxiv.org/abs/2406.07155
10. *Why Do Multi-Agent LLM Systems Fail?* (MAST) — arXiv:2503.13657 — https://arxiv.org/abs/2503.13657
11. *More Agents Is All You Need* — arXiv:2402.05120 — https://arxiv.org/abs/2402.05120
12. *Reasoning in Token Economies: Budget-Aware Evaluation of LLM Reasoning Strategies* — EMNLP 2024 — https://aclanthology.org/2024.emnlp-main.1112/
13. *Multi-Agent Reasoning Improves Compute Efficiency: Pareto-Optimal Test-Time Scaling* — ACL SRW 2026 — https://aclanthology.org/2026.acl-srw.1/
14. *What Moves the Pareto Frontier in Tool-Using Agents? A Compute-Aware Study of ReAct Variants* — ACL SRW 2026 — https://aclanthology.org/2026.acl-srw.82/
15. *AI Agents That Matter* — arXiv:2407.01502 — https://arxiv.org/abs/2407.01502
16. *Large Language Models Cannot Self-Correct Reasoning Yet* — arXiv:2310.01798 — https://arxiv.org/abs/2310.01798
17. *Early Diagnosis of Wasted Computation in Multi-Agent Systems* — arXiv:2606.01365 — https://arxiv.org/abs/2606.01365
18. *Budget-Aware Tool Use Enables Effective Agent Scaling* (BATS) — arXiv:2511.17006 — https://arxiv.org/abs/2511.17006 (code: https://github.com/google-research/budget-aware-agent)
35. *Should We Be Going MAD? A Look at Multi-Agent Debate Strategies for LLMs* — arXiv:2311.17371 — https://arxiv.org/abs/2311.17371

**Benchmarks and evaluation surfaces**

21. *MemoryAgentBench: Evaluating Memory in LLM Agents via Incremental Multi-Turn Interactions* — OpenReview — https://openreview.net/pdf/6c45207081ddfd908751b53640571f1dd7880008.pdf
22. *LongMemEval: Benchmarking Chat Assistants on Long-Term Interactive Memory* — arXiv:2410.10813 — https://arxiv.org/abs/2410.10813
25. *AgentDojo: A Dynamic Environment to Evaluate Prompt Injection Attacks and Defenses for LLM Agents* — NeurIPS 2024 Datasets & Benchmarks — https://proceedings.neurips.cc/paper_files/paper/2024/file/97091a5177d8dc64b1da8bf3e1f6fb54-Paper-Datasets_and_Benchmarks_Track.pdf
26. *Indirect Prompt Injections: Are Firewalls All You Need, or Stronger Benchmarks?* — arXiv:2510.05244 — https://arxiv.org/abs/2510.05244
27. *Adaptive Evaluation of Out-of-Band Agent Defenses* — arXiv:2606.26479 — https://arxiv.org/abs/2606.26479
32. *Inspect AI — evaluation logs* — https://inspect.aisi.org.uk/eval-logs.html (repository: https://github.com/UKGovernmentBEIS/inspect_ai)

**First-party production and engineering accounts** *(vendor self-reports — mechanisms are evidence, benefits are claims)*

19. Microsoft — *Semantic Kernel: Package Previews, Graduations & Deprecations* — https://devblogs.microsoft.com/agent-framework/semantic-kernel-package-previews-graduations-deprecations/
20. Microsoft — *Planning with Semantic Kernel using Automatic Function Calling* — https://devblogs.microsoft.com/agent-framework/planning-with-semantic-kernel-using-automatic-function-calling/
23. Anthropic — *Effective context engineering for AI agents* — https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
24. Anthropic — *Scaling Managed Agents: Decoupling the brain from the hands* — https://www.anthropic.com/engineering/managed-agents
28. Replit — *Inside Replit's Snapshot Engine: The Tech Making AI Agents Safe* — https://replit.com/blog/inside-replits-snapshot-engine
29. Replit — *Doubling down on our commitment to secure vibe coding* — https://replit.com/blog/doubling-down-on-our-commitment-to-secure-vibe-coding
30. Replit — *Decision-Time Guidance: Keeping Replit Agent Reliable* — https://replit.com/blog/decision-time-guidance
31. Replit — *Closing the loop: Evaluating and improving Replit Agent at scale* — https://replit.com/blog/evaluating-and-improving-agent-at-scale
33. Anthropic — *How we built our multi-agent research system* — https://www.anthropic.com/engineering/multi-agent-research-system
34. Anthropic — *Building effective agents* — https://www.anthropic.com/engineering/building-effective-agents

## Attribution

This skill is not original research, and its structure is not original either. Both are owed to
sources that should be named.

**Third-party sources.** Every empirical claim belongs to the 35 works in the bibliography above.
None of those figures were produced here. Vendor engineering posts are self-reports; their described
mechanisms are evidence, their claimed benefits are not independently verified.

**Borrowed structure.** The following parts of this skill are adapted, not invented, from a prior
private evidence campaign on agent-workflow over-engineering (evidence cutoff 2026-08-24) that
inspected those 35 works and produced the claim ledger this file draws on:

| Part of this skill | Adapted from that campaign's |
|---|---|
| **The three-receipt rule and the burden-of-proof default** in `SKILL.md` | Its core finding: machinery "earns its burden only when it changes a measured task bottleneck, contains a material failure mode, or moves a predeclared outcome-cost frontier", and "retain each additional component only against a matched counterfactual" |
| "Count, depth, volume, coverage and presence are not benefits" | Its "Agent count, role names, planner depth, memory volume, retries, evaluator agreement, tracing coverage, and guardrail presence are not benefits by themselves" |
| The nine-row cost table in `SKILL.md` step 5 | "Complexity scorecard" table, including its dimension names and its do-not-accept column |
| The procedure in `SKILL.md` steps 1–7 | "Architecture decision protocol" nine-step list, compressed and made operational |
| "Contradictions to keep visible" above | Its section of the same name |
| "Confusion negatives" above | Its section of the same name |
| The claim map's observations | Direct paraphrase of its `OWE-Cnnn` claim objects |
| The claim ceiling language | Its frozen claim label, quoted |

The `OWE-Cnnn` identifiers are that campaign's internal claim keys, retained as traceability handles.
They are meaningful only against its ledger; the bibliography is what a reader outside it should use.

Original to this skill: the `KEEP`/`PROVE`/`CUT`/`NOT APPLICABLE`/`CANNOT RULE` operational
verdict set with bindings and expiry, the overlap step, the smell tables, the producer/reviewer
separation, and the skill's own self-limits. The central rule is **not** among them.

Precise source provenance is deliberately excluded from the shareable surface. Its absence costs
traceability, not grounding: the bibliography above stands on its own.
