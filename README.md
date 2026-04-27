# lapop-agent-governance

A governance protocol for multi-agent LLM execution, repair, audit, learning, collaboration, and rollback.

**Tagline:** Boundaries for useful agents.

---

## Overview

`lapop-agent-governance` is a framework for coordinating multi-agent and multi-model LLM workflows without losing control of artifacts, evidence, policy, validation, or time.

It is designed for workflows where a human operator coordinates several models or agents at once:

- GPT as supervisor,
- DeepSeek as implementation worker,
- Z.ai / GLM as local repo or tool agent,
- Grok as novelty/adversarial critic,
- Claude as coherence/design critic,
- Gemini as structured documentation worker,
- local scripts as validators,
- and a human as final authority.

The core idea:

> LLM systems do not fail only because they are not smart enough.  
> They fail because they lack governance boundaries.

This repo provides those boundaries.

---

## What This Project Solves

Multi-agent LLM workflows often break down through:

- hallucinated evidence,
- invented test results,
- unowned artifact edits,
- hidden schema drift,
- critic claims treated as truth,
- uncontrolled strategy learning,
- repeated repair loops,
- stale branches,
- merge/rollback oscillation,
- human review overload,
- and false consensus between models.

`lapop-agent-governance` gives the workflow a structured operating system:

```text
task → context → assignment → boundary → policy check → worker output
     → validation → audit → merge/repair/reject → checkpoint
     → optional learning only if clean
````

---

## Governance Stack

The framework is organized into seven layers.

| Layer       | Name                                    | Function                                           |
| ----------- | --------------------------------------- | -------------------------------------------------- |
| `LAPOP-003` | LLM-Agent Prompt Orchestration Protocol | Execution backbone                                 |
| `CDO-003`   | Coordination Discipline Overlay         | Artifact boundaries and dependency notes           |
| `PCL-003`   | Policy Composition Layer                | Policy allow/deny decisions                        |
| `SLL-003`   | Strategy Learning Layer                 | Clean-run strategy learning                        |
| `EIL-003`   | Economic Intelligence Layer             | Cost/value stop-continue decisions                 |
| `CHL-003`   | Collaboration Health Layer              | Trust, disagreement, and collaboration health      |
| `TGL-003`   | Temporal Governance Layer               | Checkpoints, rollback, stale branches, oscillation |

Each layer has a distinct responsibility.

No layer should silently absorb all authority.

---

## Core Principle

```text
Do not trust the model.
Trust the governed process.
```

The framework assumes models are useful but unreliable unless governed.

It does not require models to be perfect.

It requires the process to be:

* artifact-bound,
* evidence-aware,
* policy-aware,
* validation-driven,
* contamination-safe,
* rollback-compatible,
* and auditable.

---

## Repository Structure

```text
lapop-agent-governance/
├── README.md
├── LICENSE
├── CHANGELOG.md
│
├── docs/
│   ├── overview.md
│   ├── architecture.md
│   ├── common-vs-novel.md
│   ├── implementation-roadmap.md
│   └── glossary.md
│
├── specs/
│   ├── LAPOP-003.md
│   ├── CDO-003.md
│   ├── PCL-003.md
│   ├── SLL-003.md
│   ├── EIL-003.md
│   ├── CHL-003.md
│   └── TGL-003.md
│
├── prompts/
│   ├── master-supervisor.md
│   ├── policy-composition-worker.md
│   ├── strategy-learning-worker.md
│   ├── economic-intelligence-worker.md
│   ├── collaboration-health-worker.md
│   ├── temporal-governance-worker.md
│   ├── meta-auditor.md
│   └── multi-model-coordination-v1.md
│
├── schemas/
│   ├── execution-context.schema.yaml
│   ├── dependency-note.schema.yaml
│   ├── execution-record.schema.yaml
│   ├── policy-attestation.schema.yaml
│   ├── strategy-update-patch.schema.yaml
│   └── collaboration-health-report.schema.yaml
│
├── examples/
│   ├── qnvm-agent-cycle.md
│   ├── codebase-repair-cycle.md
│   └── multi-model-worker-session.md
│
└── research/
    ├── novelty-automation-prompting-v0.1.md
    └── design-notes.md
```

---

## Quick Start

### 1. Choose a supervisor

Use `prompts/master-supervisor.md` as the controlling prompt for the model coordinating the workflow.

The supervisor is responsible for:

* understanding the task,
* assigning workers,
* enforcing boundaries,
* reviewing output,
* validating evidence,
* detecting contamination,
* deciding merge/repair/reject,
* and writing the execution summary.

---

### 2. Create an execution context

Use `schemas/execution-context.schema.yaml`.

The `ExecutionContext` records:

* project,
* session,
* cycle,
* run,
* artifact,
* agent role,
* policy state,
* strategy state,
* temporal position,
* contamination status,
* and permissions.

Minimal manual example:

```yaml
project_id: lapop-agent-governance
session_id: session-001
cycle_id: cycle-001-intake
run_id: run-001
artifact_id: prompt.master-supervisor
execution_mode: manual_paste
side_effect_class: advisory
risk_level: low
contamination_state:
  contamination_free: true
  contamination_status: clean
  learning_eligible: false
  trust_update_allowed: false
  memory_write_allowed: false
```

---

### 3. Assign a worker

Use the worker prompts in `prompts/`.

Example worker assignment:

```md
# Worker Assignment Packet

## Worker Model

DeepSeek

## Role

implementation_worker

## Assigned Artifact

schemas/execution-record.schema.yaml

## Objective

Draft the canonical Execution_Record schema.

## Forbidden Actions

- Do not modify unassigned artifacts.
- Do not invent test results.
- Do not claim file inspection unless files are provided.
- Do not change public interfaces without dependency notes.

## Required Output

1. Complete artifact or patch.
2. Validation summary.
3. Dependency notes, or `None`.
4. Known risks.
```

---

### 4. Require worker output contracts

Every worker should return:

````md
# Worker Output

## Assigned Artifact

<artifact>

## Work Completed

<summary>

## Artifact / Patch

<content or patch>

## Validation Summary

```yaml
syntax_valid: true | false | not_applicable
schema_valid: true | false | not_applicable
tests_run: []
boundary_respected: true | false
evidence_grounded: true | false
contamination_detected: true | false
````

## Dependency Notes

<notes or None>

## Known Risks / Unresolved Issues

* <risk>

````

---

### 5. Validate, audit, then merge

Use:

- `prompts/meta-auditor.md`,
- `schemas/execution-record.schema.yaml`,
- `schemas/dependency-note.schema.yaml`,
- and `schemas/collaboration-health-report.schema.yaml`.

Do not merge simply because an output looks good.

Merge only if:

```yaml
validation_passed: true
boundary_respected: true
policy_allowed: true
blocking_dependencies_resolved: true
contamination_free: true
rollback_available_for_risky_changes: true
````

---

## Minimal Manual Workflow

This project is manual-first.

You can use it before any automation exists:

```text
1. Human states objective.
2. GPT supervisor creates worker packet.
3. Human pastes packet into worker model.
4. Worker returns output contract.
5. Human pastes worker output back to supervisor.
6. Supervisor validates, audits, and merges or rejects.
7. Supervisor records execution summary.
8. Repeat.
```

This lets one person coordinate several LLMs without letting the workflow become chaos.

---

## Core Concepts

## ExecutionContext

A shared context object passed across layers.

It binds together:

* task identity,
* artifact identity,
* agent role,
* policy state,
* strategy state,
* checkpoint state,
* temporal position,
* contamination status,
* and permissions.

File:

```text
schemas/execution-context.schema.yaml
```

---

## Dependency Note

A structured request for a cross-boundary change.

If a worker sees something that must change outside its assigned artifact, it must emit a dependency note instead of editing the unowned artifact.

File:

```text
schemas/dependency-note.schema.yaml
```

Example:

```md
# Dependency Note

Source Artifact:
schemas/execution-record.schema.yaml

Target Artifact:
schemas/strategy-update-patch.schema.yaml

Priority:
high

Blocking:
true

Dependency Type:
schema

## Summary

Strategy rollback fields must align with execution record fields.

## Required Change

Ensure both schemas use compatible `strategy_ledger_head` references.

## Validation Requirement

Validate that TGL rollback can restore the strategy ledger state.
```

---

## Execution Record

A sealed trace of a governed execution.

It records:

* context,
* task request,
* task graph,
* assignments,
* artifacts,
* validation,
* audit,
* dependency notes,
* policy refs,
* strategy refs,
* economics,
* collaboration refs,
* temporal refs,
* checkpoints,
* side effects,
* failures,
* contamination,
* reproducibility,
* and final decision.

File:

```text
schemas/execution-record.schema.yaml
```

---

## Policy Attestation

A proof object emitted by `PCL-003`.

It records:

* policy query,
* action descriptor,
* effective policy hash,
* matched rules,
* conflicts,
* exceptions,
* final allowance result,
* constraints,
* and rollback support.

File:

```text
schemas/policy-attestation.schema.yaml
```

---

## Strategy Update Patch

A reversible, benchmark-gated strategy update candidate.

Strategy updates are not allowed directly from vibes.

They require:

* clean run,
* validation,
* audit,
* policy check,
* benchmark gate,
* rollback plan,
* and low initial confidence when sample size is small.

File:

```text
schemas/strategy-update-patch.schema.yaml
```

---

## Collaboration Health Report

A structured report emitted by `CHL-003`.

It tracks:

* boundary health,
* dependency-note health,
* trust signals,
* disagreement,
* merge health,
* contamination,
* human review burden,
* emergence risk,
* and intervention recommendations.

File:

```text
schemas/collaboration-health-report.schema.yaml
```

---

## Contamination Model

Contamination means unverified or invalid material enters a trusted channel.

Examples:

* a model invents a test result,
* a critic hallucination becomes a repair requirement,
* a stale branch is merged as current,
* a failed validation is hidden,
* generated text is treated as evidence,
* a model claims it inspected files it did not inspect,
* an unowned artifact is modified silently.

Contamination blocks:

```yaml
learning_allowed: false
trust_update_allowed: false
memory_write_allowed: false
validated_merge: false
```

Core rule:

> Generated text is not evidence for itself.

---

## Strategy Learning Model

SLL is intentionally conservative.

A clean execution may create a candidate strategy patch.

It does not automatically change global behavior.

Correct flow:

```text
clean execution
  → validated outcome
  → candidate strategy patch
  → policy check
  → benchmark gate
  → ledger commit
  → rollback support
```

A single successful run should usually produce only a low-confidence, scoped candidate.

---

## Economic Intelligence Model

More work is not always better.

EIL ranks candidate next actions by expected net value:

```text
Expected Net Value =
  expected quality gain
+ expected uncertainty reduction
+ expected risk reduction
+ expected rework avoidance
- expected cost
- expected latency
- expected contamination risk
- expected coordination burden
```

Possible recommendations:

* stop,
* continue,
* retrieve more,
* run validation,
* run audit,
* repair again,
* request human review,
* preserve disagreement,
* rollback,
* archive,
* defer.

Sometimes the correct answer is:

```text
Stop. Current result is good enough.
```

---

## Temporal Governance Model

TGL protects long-running work from time-based corruption.

It tracks:

* checkpoints,
* rollback candidates,
* stale branches,
* stale validation,
* expired policy exceptions,
* oscillation,
* deadlock,
* governance debt,
* policy state,
* and strategy state.

Rollback must account for:

```yaml
artifact_state: required
policy_attestation_id: required_if_policy_sensitive
strategy_ledger_head: required_if_strategy_sensitive
contamination_free: required
post_rollback_validation: required
```

---

## Multi-Model Coordination

The framework supports role-separated multi-model sessions.

Example roles:

| Model      | Suggested Role                     |
| ---------- | ---------------------------------- |
| GPT        | Supervisor / merger / meta-auditor |
| DeepSeek   | Heavy implementation worker        |
| Z.ai / GLM | Local file and tool worker         |
| Grok       | Novelty/adversarial critic         |
| Claude     | Coherence/design critic            |
| Gemini     | Structured documentation worker    |

Rule:

> No model is the whole system.

Each model is a worker inside a governed process.

---

## Example Workflows

### QNVM Agent Cycle

File:

```text
examples/qnvm-agent-cycle.md
```

Shows how to govern a simulation repair cycle where metrics reveal excessive collapse, death, resurrection, and churn.

---

### Codebase Repair Cycle

File:

```text
examples/codebase-repair-cycle.md
```

Shows how to patch a concrete Python runtime error while preserving traceback evidence, boundary discipline, validation, audit, and optional strategy learning.

---

### Multi-Model Worker Session

File:

```text
examples/multi-model-worker-session.md
```

Shows how one human can coordinate GPT, DeepSeek, Z.ai, Grok, Claude, Gemini, and other models without cross-contaminating outputs.

---

## Research Notes

### Novelty Automation Prompting

File:

```text
research/novelty-automation-prompting-v0.1.md
```

Explores how to generate novelty aggressively while merging it conservatively.

Key rule:

```text
Novelty is fuel.
Governance is the engine.
Validation is the road.
```

---

### Design Notes

File:

```text
research/design-notes.md
```

Explains the design rationale behind the stack, the seven-layer split, contamination gates, manual-first operation, and future automation path.

---

## Current Status

```yaml
status: draft_framework
version: 0.1.0
maturity: experimental
recommended_use:
  - manual multi-model coordination
  - repo planning
  - prompt governance
  - schema design
  - code repair cycles
  - audit workflows
not_yet_ready_for:
  - unsupervised autonomous execution
  - high-stakes deployment
  - automatic external writes
  - automatic strategy updates without validation
```

---

## Roadmap

### v0.1 — Manual Governance Pack

* README
* specs
* prompts
* schemas
* examples
* research notes
* execution-record-lite template
* dependency-note template

### v0.2 — Validation Utilities

Planned:

```text
validators/
├── yaml_parse.py
├── schema_validate.py
├── execution_record_lint.py
└── dependency_note_lint.py
```

### v0.3 — Local Runner

Planned:

```text
cli/
└── lapop_runner.py
```

Possible commands:

```bash
lapop init
lapop new-cycle
lapop validate schemas/
lapop create-execution-record
lapop audit-cycle
```

### v0.4 — Strategy Ledger

Planned:

```text
ledger/
├── strategy-ledger.jsonl
├── policy-attestations.jsonl
└── checkpoints/
```

### v0.5 — Review Dashboard

Possible local dashboard for:

* execution records,
* dependency notes,
* validation status,
* collaboration health,
* stale branches,
* rollback candidates,
* strategy patches.

---

## Safety and Governance Notes

This project is intended for governed coordination.

It should not be used to justify:

* unsupervised external actions,
* hidden automation,
* unreviewed publication,
* automatic deletion,
* automatic credential changes,
* high-stakes decisions without human approval,
* or strategy learning from contaminated runs.

External writes require explicit approval.

Destructive actions deny by default.

---

## Philosophy

This project is not trying to make LLMs flawless.

It assumes they are flawed.

The goal is to create a process where flawed models can still produce useful, auditable, reversible work.

```text
Evidence wins over confidence.
Boundaries win over speed.
Policy wins over economics.
Validation wins over novelty.
Rollback wins over irreversible cleverness.
```

---

## License

See `LICENSE`.

Recommended default:

```text
MIT License
```

---

## Changelog

See `CHANGELOG.md`.

---

## Contributing

This project is early.

Useful contributions include:

* schema validation fixes,
* example execution records,
* repair-cycle examples,
* prompt improvements,
* failure-mode reports,
* CLI validation scripts,
* documentation cleanup,
* benchmark-gate designs,
* and meta-audit reports.

When contributing, preserve the core invariants:

```yaml
core_invariants:
  - generated_text_is_not_evidence_for_itself
  - unowned_changes_require_dependency_notes
  - contaminated_runs_cannot_update_strategy
  - contaminated_runs_cannot_update_trust
  - policy_denial_cannot_be_overridden_by_economics
  - rollback_requires_policy_and_strategy_state_when_relevant
  - external_writes_require_explicit_approval
  - destructive_actions_deny_by_default
```

---

## Final Line

```text
Do not trust the model.
Trust the governed process.
```

```
```
