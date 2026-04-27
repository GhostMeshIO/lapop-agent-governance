# Novelty Automation Prompting v0.1

## Purpose

This research note defines an early prompting pattern for generating novelty in a governed multi-agent workflow without letting novelty contaminate validated artifacts.

The goal is not to suppress creative or speculative output.

The goal is to route novelty through safe channels:

- labeled speculation,
- adversarial review,
- benchmark planning,
- dependency notes,
- preserved disagreement,
- and supervisor-controlled merge.

Novelty is useful.

Unbounded novelty is dangerous.

---

## Status

```yaml
document_type: research_note
version: 0.1
status: experimental
owner: research
related_layers:
  - LAPOP-003
  - CDO-003
  - SLL-003
  - EIL-003
  - CHL-003
  - TGL-003
````

---

## Core Problem

Multi-model systems can generate high novelty quickly, but novelty often arrives mixed with:

* unsupported claims,
* invented terminology,
* hidden assumptions,
* speculative equations,
* false confidence,
* vague implementation paths,
* untested abstractions,
* and seductive coherence.

In an unmanaged workflow, a novelty model can inject impressive but unverified ideas directly into core architecture.

That creates contamination.

This file proposes a pattern:

> Generate novelty aggressively, but merge it conservatively.

---

## Novelty Classes

### Class 1 — Naming Novelty

New names, metaphors, labels, slogans, or conceptual handles.

Risk: low to medium.

Example:

```text
Temporal Governance Layer as the stack’s “timekeeper.”
```

Safe handling:

* allow freely in research notes,
* require review before becoming canonical terminology.

---

### Class 2 — Structural Novelty

New modules, layers, subsystems, lifecycle states, or workflow structure.

Risk: medium.

Example:

```text
Add a Governance Debt Report to track unresolved stale obligations.
```

Safe handling:

* route through design note,
* require dependency analysis,
* require schema impact check.

---

### Class 3 — Algorithmic Novelty

New scoring formulas, control loops, ranking logic, or update rules.

Risk: medium to high.

Example:

```text
Expected_Net_Value = Value - Cost - Risk + TrustAdjustment
```

Safe handling:

* require assumptions,
* require inputs/outputs,
* require benchmark plan,
* require failure modes.

---

### Class 4 — Ontological Novelty

New high-level theory, worldview, or conceptual framing.

Risk: high if merged into engineering artifacts without translation.

Example:

```text
Treat collaboration health as an immune system.
```

Safe handling:

* keep in research first,
* translate into operational schema only if useful.

---

### Class 5 — Policy Novelty

New permissions, denials, exceptions, or authority rules.

Risk: high.

Example:

```text
Allow automatic strategy updates after one successful run.
```

Safe handling:

* require PCL review,
* deny by default if safety impact is unclear.

---

### Class 6 — Strategy Learning Novelty

New ways for the system to adapt from prior runs.

Risk: high.

Example:

```text
Increase routing trust for a model after successful repair.
```

Safe handling:

* require clean-run status,
* require benchmark gate,
* require rollback plan,
* require low initial confidence.

---

## Novelty Pipeline

```text
Novelty Prompt
  → Novelty Candidate Output
  → Classification
  → Evidence Check
  → Risk Check
  → Dependency Notes
  → Benchmark Plan
  → Supervisor Decision
  → Research / Backlog / Patch / Reject
```

---

## Novelty Worker Prompt

```md
# Novelty Worker Prompt

## Role

You are a novelty worker inside a governed multi-agent workflow.

Your job is to generate new ideas, edge cases, structures, algorithms, prompts, schemas, or research directions.

You are not a source of truth.

You are not allowed to merge your own output.

You must clearly distinguish:

- grounded claims,
- inferred claims,
- speculative ideas,
- implementation-ready suggestions,
- and unsafe or unvalidated ideas.

## Assigned Topic

<topic>

## Existing Context

<context>

## Required Output

Return:

1. Novel ideas.
2. Why each idea might matter.
3. What evidence supports it, if any.
4. What assumptions it depends on.
5. What could go wrong.
6. What artifact it would affect.
7. Whether it needs a dependency note.
8. Whether it needs benchmark validation.
9. Whether it should be research-only, backlog, candidate patch, or rejected.

## Forbidden Behavior

- Do not claim novelty is truth.
- Do not claim tests were run.
- Do not claim file inspection unless files were provided.
- Do not silently rewrite architecture.
- Do not erase uncertainty.
- Do not use mystical, metaphorical, or poetic framing as engineering proof.
```

---

## Novelty Candidate Schema

```yaml
NoveltyCandidate:
  candidate_id: string
  title: string
  novelty_class:
    - naming
    - structural
    - algorithmic
    - ontological
    - policy
    - strategy_learning
    - other
  summary: string
  proposed_artifact_targets:
    - string
  supporting_evidence:
    - string
  assumptions:
    - string
  risk_level: low | medium | high | critical
  implementation_readiness:
    - research_only
    - backlog
    - candidate_patch
    - ready_for_implementation
    - reject
  dependency_note_required: true | false
  benchmark_required: true | false
  policy_review_required: true | false
  rollback_required: true | false
  contamination_risk: low | medium | high | critical
```

---

## Novelty Classification Prompt

```md
# Novelty Classification Prompt

Classify the following idea.

## Idea

<idea>

## Classify By

1. Novelty class.
2. Risk level.
3. Artifact targets.
4. Whether it is research-only or implementation-ready.
5. Required validation.
6. Required dependency notes.
7. Required policy review.
8. Contamination risk.

## Output YAML

Return a `NoveltyCandidate` object.
```

---

## Novelty Acceptance Rules

A novelty candidate may move to backlog if:

```yaml
backlog_eligible:
  - risk_level is low or medium
  - assumptions are stated
  - artifact targets are known
  - no unsupported factual claim is required
```

A novelty candidate may move to candidate patch if:

```yaml
candidate_patch_eligible:
  - artifact target is known
  - implementation path is clear
  - validation plan exists
  - dependency notes are complete
  - policy review is not blocking
  - rollback is possible
```

A novelty candidate must stay research-only if:

```yaml
research_only:
  - assumptions are untested
  - terms are not operationalized
  - no benchmark exists
  - impact is architectural
  - claim is metaphorical or philosophical
```

A novelty candidate must be rejected if:

```yaml
reject:
  - it requires fabricated evidence
  - it weakens contamination gates
  - it bypasses policy
  - it encourages unsafe external action
  - it breaks rollback
  - it silently mutates unowned artifacts
```

---

## Examples

### Example 1 — Useful Structural Novelty

```yaml
candidate_id: novelty-001
title: Governance Debt Report
novelty_class: structural
summary: >
  Add a report that counts stale notes, unresolved disagreements, expired exceptions,
  old rollback candidates, and oscillation records.
proposed_artifact_targets:
  - specs/TGL-003.md
  - schemas/execution-record.schema.yaml
supporting_evidence:
  - "Long-running workflows accumulate unresolved state."
assumptions:
  - "Stale governance state can be measured."
risk_level: medium
implementation_readiness: candidate_patch
dependency_note_required: true
benchmark_required: true
policy_review_required: false
rollback_required: true
contamination_risk: low
```

Supervisor result:

```yaml
decision: accept_as_candidate_patch
reason: "Operational, bounded, and testable."
```

---

### Example 2 — Speculative Ontological Novelty

```yaml
candidate_id: novelty-002
title: Semantic Immune System
novelty_class: ontological
summary: >
  Treat collaboration health as an immune system that detects contamination,
  boundary violations, and trust decay.
proposed_artifact_targets:
  - docs/architecture.md
  - specs/CHL-003.md
supporting_evidence:
  - "Metaphor aligns with CHL function."
assumptions:
  - "The metaphor improves understanding without adding false claims."
risk_level: medium
implementation_readiness: research_only
dependency_note_required: false
benchmark_required: false
policy_review_required: false
rollback_required: false
contamination_risk: medium
```

Supervisor result:

```yaml
decision: keep_as_research_or_docs_metaphor
reason: "Useful framing, not engineering proof."
```

---

### Example 3 — Unsafe Strategy Novelty

```yaml
candidate_id: novelty-003
title: Automatic Global Trust Increase
novelty_class: strategy_learning
summary: >
  Increase a model’s trust globally after one successful repair.
proposed_artifact_targets:
  - specs/SLL-003.md
  - specs/CHL-003.md
supporting_evidence:
  - "One repair succeeded."
assumptions:
  - "One success generalizes globally."
risk_level: high
implementation_readiness: reject
dependency_note_required: false
benchmark_required: true
policy_review_required: true
rollback_required: true
contamination_risk: high
```

Supervisor result:

```yaml
decision: reject
reason: "Overgeneralizes from one run and risks trust pollution."
```

---

## Novelty-to-Implementation Prompt

Use this only after a novelty candidate is accepted as a candidate patch.

```md
# Novelty-to-Implementation Prompt

## Role

You are a bounded implementation worker.

## Accepted Novelty Candidate

<candidate>

## Assigned Artifact

<artifact>

## Required Implementation

<specific implementation steps>

## Forbidden Changes

- Do not expand scope.
- Do not modify unowned artifacts.
- Do not weaken validation.
- Do not bypass policy.
- Do not claim benchmarks pass unless run.

## Dependency Notes

Emit dependency notes for cross-artifact impacts.

## Validation

<validation plan>

## Output Contract

Return:

1. Patch or complete artifact.
2. Validation summary.
3. Dependency notes.
4. Known risks.
```

---

## Novelty Scoring

Optional scoring model:

```text
NoveltyUtility =
  0.25 * usefulness
+ 0.20 * testability
+ 0.20 * implementation_clarity
+ 0.15 * compatibility
+ 0.10 * reversibility
+ 0.10 * compression_value
- 0.25 * contamination_risk
- 0.15 * policy_risk
- 0.10 * complexity_cost
```

Interpretation:

```yaml
score_interpretation:
  below_0: reject
  0_to_0.25: research_only
  0.25_to_0.50: backlog
  0.50_to_0.75: candidate_patch
  above_0.75: implementation_ready_after_review
```

---

## Anti-Patterns

### Anti-Pattern 1 — Novelty Laundering

A speculative idea is placed directly into a spec as if it were validated.

Fix:

```text
Move to research note or mark as proposed.
```

### Anti-Pattern 2 — Critic Authority Drift

A critic says something dramatic and the supervisor treats it as proof.

Fix:

```text
Require artifact evidence or mark as speculative.
```

### Anti-Pattern 3 — Metaphor as Mechanism

A poetic frame becomes an algorithm without operational definition.

Fix:

```text
Translate metaphor into inputs, outputs, states, and tests.
```

### Anti-Pattern 4 — One-Run Generalization

A single successful repair becomes a global rule.

Fix:

```text
Create low-confidence candidate patch with benchmark gate.
```

### Anti-Pattern 5 — Novelty Flood

Too many new ideas enter the cycle and block implementation.

Fix:

```text
Use EIL to rank expected net value and defer low-value novelty.
```

---

## Final Rule

Novelty is fuel.

Governance is the engine.

Validation is the road.

Without governance, novelty burns the project down.

With governance, novelty becomes architecture.

````

---

## `research/design-notes.md`

```md id="1g5ws5"
# Design Notes

## Purpose

This file records high-level design reasoning for `lapop-agent-governance`.

It explains why the framework is structured as a governed multi-layer stack and what problems each layer is meant to prevent.

These notes are not canonical policy.

They are design rationale.

---

## Project Summary

`lapop-agent-governance` is a framework for coordinating multi-agent, multi-model LLM work without losing control of artifacts, evidence, policy, or time.

It is designed for practical workflows where a human operator may coordinate:

- one supervisor model,
- several worker models,
- tool-using local agents,
- critics,
- auditors,
- and manual file edits.

The framework assumes that LLMs are useful but unreliable unless governed.

---

## Core Design Claim

The core design claim is:

> Multi-agent LLM systems do not primarily fail because they lack intelligence.  
> They fail because they lack governance boundaries.

Common failure modes include:

- unowned artifact edits,
- hallucinated evidence,
- invented test results,
- critic claims treated as truth,
- hidden schema drift,
- uncontrolled strategy learning,
- excessive retries,
- stale branches,
- merge/rollback oscillation,
- human review overload,
- and false consensus.

The framework addresses these with explicit layers.

---

## Layer Rationale

## LAPOP-003 — Execution Backbone

LAPOP exists because a multi-agent system needs a traceable execution lifecycle.

Without LAPOP:

- tasks blur,
- assignments blur,
- artifacts blur,
- validation is forgotten,
- and final outputs cannot be replayed or audited.

LAPOP provides:

- task intake,
- decomposition,
- assignment,
- validation,
- audit,
- merge,
- repair,
- escalation,
- and execution records.

Design note:

```text
LAPOP is the skeleton.
````

---

## CDO-003 — Coordination Discipline Overlay

CDO exists because agent systems easily violate artifact boundaries.

Without CDO:

* workers edit unassigned files,
* schema changes propagate silently,
* interface contracts drift,
* dependency notes are skipped,
* and ownership becomes meaningless.

CDO provides:

* artifact boundaries,
* dependency notes,
* ownership discipline,
* semantic integrity checks,
* and cross-boundary change control.

Design note:

```text
CDO is the connective tissue.
```

---

## PCL-003 — Policy Composition Layer

PCL exists because “should we do this?” must be separated from “can we do this?”

Without PCL:

* economics overrides safety,
* workers perform unauthorized actions,
* exceptions become permanent,
* destructive actions happen casually,
* external writes occur without approval.

PCL provides:

* allow/deny decisions,
* policy precedence,
* exception handling,
* policy attestation,
* and action constraints.

Design note:

```text
PCL is the legal system.
```

---

## SLL-003 — Strategy Learning Layer

SLL exists because useful systems should improve, but unsafe learning poisons the future.

Without SLL:

* contaminated runs become training signals,
* one success becomes a global rule,
* hallucinated critique changes strategy,
* model trust becomes vibes-based,
* and bad repairs become default behavior.

SLL provides:

* clean-run learning gates,
* quarantine records,
* candidate strategy patches,
* benchmark gates,
* low-confidence updates,
* and rollback-compatible strategy ledger state.

Design note:

```text
SLL is disciplined adaptation.
```

---

## EIL-003 — Economic Intelligence Layer

EIL exists because agent systems often do too much.

Without EIL:

* models keep retrying,
* audits pile up,
* human review is overused,
* tools are called without value,
* and the system expands beyond the task.

EIL provides:

* value-of-information estimation,
* cost/risk scoring,
* stop/continue decisions,
* human attention budgeting,
* repair economics,
* audit economics,
* and regret tracking.

Design note:

```text
EIL is the cost-value brain.
```

---

## CHL-003 — Collaboration Health Layer

CHL exists because multi-agent systems can look busy while becoming unhealthy.

Without CHL:

* boundary violations normalize,
* trust gets polluted,
* disagreement gets flattened,
* note ping-pong grows,
* repair loops repeat,
* and collaboration overhead becomes the work.

CHL provides:

* boundary health,
* dependency health,
* trust signals,
* disagreement preservation,
* merge health,
* human burden monitoring,
* emergence risk detection,
* and intervention recommendations.

Design note:

```text
CHL is the immune system.
```

---

## TGL-003 — Temporal Governance Layer

TGL exists because time corrupts unmanaged workflows.

Without TGL:

* stale branches return as if fresh,
* old validation is trusted,
* policy assumptions expire unnoticed,
* strategy state cannot roll back,
* oscillation repeats,
* and governance debt accumulates invisibly.

TGL provides:

* checkpoints,
* rollback candidates,
* stale branch handling,
* oscillation detection,
* deadlock detection,
* governance debt reports,
* policy/strategy restoration,
* and temporal review.

Design note:

```text
TGL is the timekeeper.
```

---

## Why Seven Layers?

The stack is intentionally separated.

A single “supervisor” can do everything in a small task, but as complexity grows, responsibilities need separation.

| Concern                     | Layer |
| --------------------------- | ----- |
| What work is being done?    | LAPOP |
| Who owns what?              | CDO   |
| Is this allowed?            | PCL   |
| Can we learn from this?     | SLL   |
| Is another action worth it? | EIL   |
| Is collaboration healthy?   | CHL   |
| What changes over time?     | TGL   |

This separation prevents authority collapse.

No layer should silently absorb all powers.

---

## Core Invariants

The framework depends on a few hard invariants:

```yaml
core_invariants:
  - Generated text is not evidence for itself.
  - Critique is not source truth.
  - Unowned artifact changes require dependency notes.
  - PCL deny is final unless valid higher authority changes policy.
  - SLL cannot learn from contaminated runs.
  - CHL cannot update trust from contaminated runs.
  - EIL cannot override policy.
  - TGL rollback must restore policy and strategy state when required.
  - External writes require explicit approval.
  - Destructive actions deny by default.
```

---

## Why Contamination Matters

Contamination means unverified or invalid material enters a trusted channel.

Examples:

* a model invents a test result,
* a critic hallucination becomes a repair requirement,
* an uninspected file is described as inspected,
* a stale branch is merged as current,
* a failed validation is hidden,
* a policy-denied action is represented as completed.

The danger is not one bad output.

The danger is that the system starts learning from or trusting the bad output.

That is why contamination blocks:

```yaml
contamination_blocks:
  - strategy_learning
  - trust_update
  - memory_write
  - validated_merge
```

---

## Why Dependency Notes Matter

Dependency notes are the safe alternative to unauthorized edits.

When an agent sees a needed change outside its boundary, it should not silently modify another artifact.

It should emit a dependency note.

This preserves:

* ownership,
* intent,
* review,
* merge safety,
* downstream awareness,
* and auditability.

Dependency notes are small governance objects with large protective value.

---

## Why Strategy Learning Is Conservative

The system should improve, but only from clean outcomes.

A single success can suggest a candidate strategy.

It should not create a global rule.

The correct flow is:

```text
Clean outcome
  → Strategy outcome record
  → Candidate patch
  → Policy check
  → Benchmark gate
  → Ledger commit
  → Rollback support
```

This keeps adaptation useful but reversible.

---

## Why Economics Is Included

LLM workflows can expand endlessly.

Given enough models, every artifact can be:

* audited again,
* rewritten again,
* criticized again,
* benchmarked again,
* documented again.

That is not always value.

EIL exists to ask:

> Is the next action worth it?

Sometimes the best action is stop.

Sometimes the best action is preserve disagreement.

Sometimes the best action is a targeted test instead of a broad rewrite.

---

## Why Collaboration Health Is Separate From Strategy Learning

Trust and strategy are related but not identical.

A worker may succeed once without becoming globally trusted.

A critic may find useful issues while also hallucinating others.

A model may be strong at prose and weak at code.

CHL tracks collaboration signals.

SLL proposes strategy changes.

Keeping them separate prevents trust from becoming unreviewed strategy.

---

## Why Temporal Governance Is Necessary

Multi-session work creates old state.

Old state becomes dangerous when it reappears without context.

Examples:

* a stale branch contains an old schema name,
* a prior prompt assumes a field that was renamed,
* an old model output conflicts with current policy,
* a rollback target lacks strategy ledger state,
* a disagreement was preserved but never reviewed.

TGL makes time explicit.

---

## Manual-First Design

The framework is designed to work manually before automation exists.

A human can use it by:

1. asking a supervisor model for assignment packets,
2. pasting packets into worker models,
3. collecting worker outputs,
4. feeding outputs back to the supervisor,
5. applying accepted artifacts manually,
6. recording an execution record lite.

This is important because many real multi-model workflows start as human-directed coordination.

Automation can come later.

---

## Automation Path

The likely automation path is:

```text
Manual packets
  → Repo templates
  → YAML schemas
  → Local validation scripts
  → CLI runner
  → Agent orchestration
  → Dashboard
  → Policy/strategy ledger
```

The framework should not assume full automation at the start.

---

## Naming Notes

Current repository name:

```text
lapop-agent-governance
```

Possible subtitle:

```text
A governance protocol for multi-agent LLM execution, repair, audit, and rollback.
```

Possible tagline:

```text
Boundaries for useful agents.
```

---

## Design Tradeoffs

### Strictness vs Speed

The framework adds overhead.

That overhead is justified when:

* multiple models are involved,
* artifacts persist,
* code or schemas change,
* validation matters,
* rollback matters,
* or learning/trust may be updated.

For trivial tasks, use lighter forms.

---

### Manual Flexibility vs Formal Schemas

The framework uses schemas, but manual workflows may use simplified records.

This is intentional.

Strict machine validation can come later.

The early goal is disciplined structure, not bureaucracy.

---

### Novelty vs Grounding

The framework should allow strange, creative, cutting-edge ideas.

But novelty must be routed through research, backlog, candidate patch, or dependency note.

Do not merge novelty directly into canonical architecture without validation.

---

### Multi-Model Diversity vs Consistency

Different models have different strengths.

The framework benefits from diversity but controls it through:

* role assignment,
* artifact boundaries,
* output contracts,
* validation summaries,
* supervisor merge,
* and meta-audit.

---

## Open Design Questions

1. Should schemas be strict JSON Schema, OpenAPI-compatible, or YAML-native?
2. Should `ExecutionContext` be embedded or referenced in every record?
3. Should dependency notes be stored as files, issues, or database records?
4. Should strategy ledger be Git-based, SQLite-based, or JSONL-based?
5. How should semantic change magnitude be computed?
6. How much validation is required for documentation-only artifacts?
7. How should human review approvals be represented?
8. Should trust be tracked per model, per role, per artifact type, or all three?
9. How should stale branch divergence be measured?
10. Should the framework include a local CLI runner in v0.1?

---

## Near-Term Priorities

```yaml
near_term_priorities:
  - finish README
  - finish specs
  - finish prompts
  - finish schemas
  - add examples
  - add research notes
  - add basic YAML validation script
  - add issue templates
  - add execution-record-lite template
  - add roadmap for CLI runner
```

---

## Future Work

Possible future modules:

* `cli/lapop-runner.py`
* `validators/schema_check.py`
* `templates/execution-record-lite.yaml`
* `templates/dependency-note.yaml`
* `templates/worker-assignment.md`
* `templates/supervisor-merge-packet.md`
* `dashboards/local-review-ui.md`
* `benchmarks/strategy-patch-gates.md`
* `tests/schema-validation/`

---

## Final Design Principle

The framework is not trying to make LLMs perfect.

It assumes imperfection.

It builds a process where imperfect models can still produce useful, auditable, reversible work.

That is the point:

```text
Do not trust the model.
Trust the governed process.
```

