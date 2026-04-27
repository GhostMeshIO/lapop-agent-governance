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
