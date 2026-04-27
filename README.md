# LAPOP Agent Governance

**Production-oriented governance protocols for auditable, contamination-safe multi-agent LLM orchestration.**

`lapop-agent-governance` is a public specification stack for coordinating multiple LLM agents across long-running software, research, audit, documentation, and artifact-generation workflows.

It is designed for people building serious multi-agent systems where agents need to:

- work on bounded artifacts,
- exchange structured dependency notes,
- avoid cross-agent contamination,
- preserve reproducibility,
- validate and merge outputs safely,
- learn from clean execution history,
- manage cost and value-of-information decisions,
- detect collaboration pathologies,
- and support rollback across multi-cycle work.

This is not a toy “agent swarm” prompt pack.

It is a governance architecture for disciplined multi-agent execution.

---

## Repository

```text
GhostMeshIO/lapop-agent-governance
````

---

## Status

```yaml
Project_Status: Early_Public_Specification
Version: 0.1
Primary_Mode: Protocol / Architecture / Prompt-Orchestration Stack
Implementation_Status: Reference implementation pending
```

Current focus:

1. Publish the consolidated protocol documents.
2. Normalize shared schemas.
3. Define a minimal reference runner.
4. Build testable examples for multi-agent coding, auditing, and research workflows.
5. Develop benchmark gates for contamination safety, boundary discipline, rollback, and merge quality.

---

## Purpose

Modern multi-agent LLM systems often focus on:

* planner / worker / critic patterns,
* tool use,
* parallel generation,
* self-review,
* and final answer synthesis.

That is not enough for real long-running work.

This project focuses on the missing control-plane machinery:

* Who owns which artifact?
* Who is allowed to edit what?
* How are cross-file changes requested?
* How do agents preserve disagreements instead of forcing bad merges?
* How do we prevent hallucinated critic output from becoming “evidence”?
* How do we rollback a multi-cycle execution?
* How do we learn from previous runs without learning from contaminated runs?
* How do we decide when another tool call, audit, retrieval, or human review is worth the cost?

`lapop-agent-governance` provides a layered answer.

---

## Core Stack

| Layer       | Name                                    | Purpose                                                                                                      |
| ----------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `LAPOP-003` | LLM-Agent Prompt Orchestration Protocol | Runtime execution backbone: task state, validation, audit, merge, repair, provenance, reproducibility        |
| `CDO-003`   | Coordination Discipline Overlay         | Artifact boundaries, dependency notes, lane discipline, ownership transfer, boundary violation handling      |
| `PCL-003`   | Policy Composition Layer                | Deterministic policy composition, conflict resolution, scoped exceptions, policy attestation                 |
| `SLL-003`   | Strategy Learning Layer                 | Contamination-safe learning from outcomes, repair patterns, routing priors, benchmark-gated strategy updates |
| `EIL-003`   | Economic Intelligence Layer             | Value-of-information decisions, tool cost, human review cost, regret, stop/probe logic                       |
| `CHL-003`   | Collaboration Health Layer              | Boundary health, trust graph, disagreement preservation, emergence risk, collaboration metrics               |
| `TGL-003`   | Temporal Governance Layer               | Aging, archival, deadlock prevention, oscillation control, rollback, speculative branch preservation         |

---

## Design Philosophy

The stack follows a few non-negotiable principles.

### 1. Artifact boundaries matter

Agents should not freely mutate the entire workspace.

Each worker receives an assigned artifact, module, file, schema, report, or interface. If another artifact must change, the worker emits a structured dependency note instead of silently editing outside its lane.

### 2. Claims require grounding

Outputs must be based on:

* direct inspection,
* tool results,
* cited sources,
* validated schemas,
* executable tests,
* or explicit uncertainty.

Unsupported claims are treated as grounding failures.

### 3. Contamination must not enter memory

Speculative outputs, critic hallucinations, failed repairs, or contaminated executions must not become authoritative system knowledge.

Execution records carry a `contamination_free` flag. Learning, trust updates, collaboration metrics, and strategy updates ignore contaminated runs.

### 4. Disagreement can be valuable

Not all disagreement should be merged away.

Some disagreements should be preserved as first-class artifacts with non-merge reasons, evidence, intervention prompts, and revalidation cycles.

### 5. Learning must be reversible

The system may learn better routing, repair patterns, validation thresholds, or escalation triggers, but only through explicit, benchmark-gated, reversible strategy patches.

No hidden self-modification.

### 6. Reproducibility is a first-class requirement

Every significant execution should be replayable or explainably non-replayable.

Checkpoints, hashes, policy attestations, strategy ledger heads, tool versions, model versions, retrieval snapshots, and validation results should be preserved.

---

## What Makes This Different

Most multi-agent systems already include common patterns like:

* planner / worker / critic / supervisor roles,
* task decomposition,
* validation,
* retry logic,
* routing,
* memory,
* and final merge.

This project adds stricter governance primitives:

* artifact boundaries with forbidden actions,
* structured dependency notes with lifecycle states,
* anti-contamination barriers,
* policy attestation records,
* deterministic policy precedence,
* strategy memory ledger,
* benchmark regression gates,
* value-of-information decisioning,
* disagreement preservation artifacts,
* temporal rollback candidates,
* oscillation detection,
* trust decay,
* boundary-health metrics,
* speculative branch preservation,
* and constitutional review cycles.

---

## Example Use Cases

### Multi-agent code repair

Assign agents to specific files or modules. Each agent may only modify its owned artifact. Cross-module changes are requested through dependency notes. Supervisor validates, audits, merges, and records provenance.

### Large research synthesis

Separate agents handle literature review, equation extraction, critique, falsifiability analysis, and implementation blueprinting. Disagreements are preserved instead of flattened.

### Long-running architecture design

Policy, strategy, economic, collaboration, and temporal governance layers evolve across cycles while keeping rollback and contamination barriers intact.

### Agent benchmark laboratory

Run multiple model workers on the same task, compare boundary discipline, hallucination rate, cost, repair success, and merge quality.

### Human-in-the-loop governance

Human review is modeled as scarce and costly. The system decides when escalation is worth it rather than treating human attention as free.

---

## Recommended Repository Layout

```text
lapop-agent-governance/
├── README.md
├── LICENSE
├── CHANGELOG.md
├── docs/
│   ├── overview.md
│   ├── architecture.md
│   ├── common-vs-novel.md
│   ├── implementation-roadmap.md
│   └── glossary.md
├── specs/
│   ├── LAPOP-003.md
│   ├── CDO-003.md
│   ├── PCL-003.md
│   ├── SLL-003.md
│   ├── EIL-003.md
│   ├── CHL-003.md
│   └── TGL-003.md
├── prompts/
│   ├── master-supervisor.md
│   ├── policy-composition-worker.md
│   ├── strategy-learning-worker.md
│   ├── economic-intelligence-worker.md
│   ├── collaboration-health-worker.md
│   ├── temporal-governance-worker.md
│   ├── meta-auditor.md
│   └── multi-model-coordination-v1.md
├── schemas/
│   ├── execution-context.schema.yaml
│   ├── execution-record.schema.yaml
│   ├── dependency-note.schema.yaml
│   ├── artifact-boundary.schema.yaml
│   ├── policy-attestation.schema.yaml
│   ├── strategy-update-patch.schema.yaml
│   ├── collaboration-health-report.schema.yaml
│   └── rollback-candidate.schema.yaml
├── examples/
│   ├── qnvm-agent-cycle.md
│   ├── codebase-repair-cycle.md
│   ├── research-audit-cycle.md
│   └── multi-model-worker-session.md
├── research/
│   ├── novelty-automation-prompting-v0.1.md
│   └── design-notes.md
└── roadmap/
    ├── v0.1-public-spec.md
    ├── v0.2-schema-normalization.md
    └── v0.3-reference-runner.md
```

---

## Minimal Workflow

A basic LAPOP/CDO-style multi-agent cycle looks like this:

```text
1. User submits task.
2. Supervisor creates ExecutionContext.
3. Policy layer compiles effective rules.
4. Task is decomposed into artifact-bound work units.
5. Agents receive assignments and artifact boundaries.
6. Agents produce outputs and dependency notes.
7. Validators check syntax, schemas, evidence, boundaries, and completeness.
8. Auditors detect hallucination, drift, contamination, or unsupported claims.
9. Merger combines valid outputs or preserves disagreement.
10. Execution record is sealed.
11. Strategy layer learns only if contamination_free = true.
12. Temporal layer manages checkpoints, rollback, aging, and oscillation.
```

---

## Core Concepts

### ExecutionContext

Shared context object passed through the stack.

```yaml
ExecutionContext:
  project_id: string
  session_id: string
  cycle_id: string
  task_node_id: optional[UUID]
  artifact_id: optional[string]
  agent_class: optional[string]
  agent_assignment_id: optional[string]
  timestamp: timestamp
  checkpoint_hash: optional[string]
  policy_attestation_id: optional[UUID]
  strategy_ledger_head: optional[string]
```

### Artifact Boundary

Defines what an agent may and may not do.

```yaml
Artifact_Boundary:
  artifact_id: string
  artifact_type: enum [file, module, schema, dataset, interface, config, diagram, report]
  owner_agent_class: string
  owner_assignment_id: optional[string]
  allowed_actions:
    - read
    - annotate
    - propose_change
    - modify_content
    - validate
    - approve_for_merge
  forbidden_actions:
    - modify_unowned
    - delete
    - rename
    - change_interface
    - change_schema
    - publish_without_validation
```

### Dependency Note

Used when one agent needs another artifact changed.

```yaml
Dependency_Note:
  note_id: UUID
  target_agent_class: string
  target_assignment_id: optional[string]
  target_artifact: string
  summary: string
  required_change: string
  justification: string
  blocking: boolean
  priority: enum [low, medium, high, blocking]
  dependency_type: enum [schema, interface, logic, validation, performance]
  lifecycle_state: enum [open, acknowledged, deferred, resolved, superseded, rejected]
```

### Execution Record

The sealed trace of a run.

```yaml
Execution_Record:
  execution_id: UUID
  context: ExecutionContext
  task_graph: list[TaskNode]
  agent_assignments: list[AgentAssignment]
  artifacts_produced: list[ArtifactRef]
  validation_reports: list[ValidationReport]
  audit_reports: list[AuditReport]
  merge_log: optional[MergeLog]
  dependency_notes: list[Dependency_Note]
  checkpoints: list[Checkpoint]
  contamination_free: boolean
  failure_events: list[FailureEvent]
  created_at: timestamp
  completed_at: optional[timestamp]
```

---

## Layer Summaries

### LAPOP-003

The orchestration backbone.

Responsibilities:

* task typing,
* decomposition,
* routing,
* execution,
* validation,
* audit,
* merge,
* repair,
* memory writeback,
* reproducibility,
* side-effect control.

### CDO-003

The coordination discipline overlay.

Responsibilities:

* artifact ownership,
* allowed and forbidden actions,
* dependency notes,
* boundary violations,
* note lifecycle,
* ownership transfer,
* semantic integrity,
* supervisor-mediated consolidation.

### PCL-003

The policy arbiter.

Responsibilities:

* ingest policy fragments,
* compile deterministic precedence,
* resolve conflicts,
* build scoped policy maps,
* manage exception windows,
* produce policy attestations,
* expose `is_action_allowed`.

### SLL-003

The safe strategy learner.

Responsibilities:

* collect clean execution outcomes,
* quarantine contaminated runs,
* learn routing priors,
* mine repair patterns,
* tune validation predictors,
* track merge regret,
* commit benchmark-gated strategy patches,
* support rollback through a strategy memory ledger.

### EIL-003

The value-of-information layer.

Responsibilities:

* decide whether further computation is worth it,
* rank next actions,
* estimate information gain,
* model tool trust,
* model human review cost,
* track economic regret,
* stop when marginal value is too low.

### CHL-003

The collaboration immune system.

Responsibilities:

* measure boundary health,
* track note density and cross-boundary churn,
* maintain decaying trust graphs,
* preserve important disagreement,
* detect harmful coordination loops,
* expose collaboration health reports,
* emit intervention signals.

### TGL-003

The temporal control layer.

Responsibilities:

* age unresolved items,
* archive stale branches,
* reopen archived items,
* detect deadlock,
* detect oscillation,
* preserve speculative branches,
* coordinate rollback,
* maintain governance debt reports,
* schedule constitutional review.

---

## Prompt Packs

The repository includes prompt templates for:

* master supervisor,
* layer-specific workers,
* meta-auditor,
* code generation workers,
* analysis-only workers,
* multi-model coordination cycles.

These are intended for running separate model sessions while preserving shared invariants.

Example roles:

```text
GPT Supervisor:
  Integration, validation, synchronization, correction.

Implementation Worker:
  Owns one assigned artifact or file.

Analysis Worker:
  Produces critique, shortcomings, or recommendations only.

Meta-Auditor:
  Finds contradictions, missing schemas, contamination paths, and reproducibility failures.
```

---

## Multi-Model Coordination Pattern

The stack can coordinate different LLMs by assigning each model a strict role.

Example:

```text
DeepSeek:
  heavy code generation or repair

GLM / Z.ai Agent:
  file editing, tool execution, local testing

Grok:
  alternative critique, adversarial framing, novelty exploration

Gemini:
  structured worker sessions or module-specific generation

GPT Supervisor:
  architecture, validation, merge discipline, prompt synthesis, audit
```

Each worker receives:

* assigned artifact,
* allowed actions,
* forbidden actions,
* output contract,
* dependency-note rules,
* validation requirements,
* and rollback expectations.

---

## Anti-Contamination Rules

The system treats contamination as a first-class failure mode.

Examples of contamination:

* hallucinated evidence,
* unsupported claims,
* generated speculation cited as source,
* critic output treated as authority,
* failed repair prompt becoming canonical,
* merged artifact overwriting original evidence,
* unverified external artifact entering memory.

Contaminated runs:

* do not update trust graphs,
* do not update strategy learning,
* do not enter validated memory,
* do not influence collaboration health scores,
* require quarantine or supervisor review.

---

## Governance Layers as Control Plane

The project separates execution from governance.

```text
Data Plane:
  agent execution,
  tool calls,
  artifact generation,
  validation,
  audit,
  merge.

Control Plane:
  policy composition,
  routing,
  economics,
  strategy learning,
  temporal governance,
  collaboration health,
  rollback,
  contamination control.
```

This prevents worker agents from silently changing the rules of their own execution.

---

## Roadmap

### v0.1 — Public Specification Release

* Publish consolidated docs.
* Establish repo structure.
* Add README and license.
* Include initial prompt packs.
* Include layer specs.

### v0.2 — Schema Normalization

* Normalize shared schemas.
* Remove duplicated schema definitions.
* Define canonical `ExecutionContext`.
* Define canonical `Execution_Record`.
* Define canonical `Dependency_Note`.
* Define canonical `HumanReviewOutcome`.
* Define canonical rollback and policy attestation schemas.

### v0.3 — Reference Runner

* Build a minimal local runner.
* Support task decomposition.
* Assign artifact boundaries.
* Validate dependency notes.
* Emit execution records.
* Run simple supervisor merge.

### v0.4 — Policy and Temporal MVP

* Implement policy query stub.
* Implement exception windows.
* Implement checkpointing.
* Implement basic rollback.
* Implement aging and archive logic.

### v0.5 — Collaboration Health MVP

* Boundary health metrics.
* Note density.
* Cross-boundary churn.
* Simple trust graph.
* Disagreement preservation record.

### v0.6 — Strategy Learning MVP

* Outcome collection.
* Quarantine gate.
* Strategy update patch schema.
* Benchmark regression gate stub.
* Reversible strategy ledger.

### v0.7 — Economic Intelligence MVP

* Value-of-information decision record.
* Tool trust calibration.
* Human review cost model.
* Stop/probe decision logic.

### v1.0 — Integrated Reference Platform

* End-to-end execution cycle.
* Multi-agent worker support.
* Policy attestation.
* Strategy ledger.
* Collaboration health report.
* Temporal rollback.
* Reproducibility profile.
* Benchmark examples.

---

## Example: Dependency Note

```text
DEPENDENCY NOTE FOR Strategy Learning Layer:
The Execution_Record schema must expose contamination_free, validation_outcome,
repair_strategy_used, merge_method, human_review_outcome, and benchmark_result
so SLL can learn from clean runs only.
```

---

## Example: Worker Output Contract

```text
=== ARTIFACT UPDATED: <artifact_id> ===

<complete artifact content>

=== VALIDATION SUMMARY ===

- syntax_valid: true
- schema_valid: true
- boundary_respected: true
- contamination_free: true
- tests_passed: true

=== DEPENDENCY NOTES ===

None
```

---

## Contributing

This project welcomes:

* protocol critiques,
* schema improvements,
* implementation prototypes,
* benchmark ideas,
* examples of multi-agent coordination failures,
* integrations with local agent tooling,
* documentation improvements.

Contribution guidelines will be added as the project stabilizes.

Until then:

1. Keep changes scoped.
2. Preserve artifact boundaries.
3. Do not remove validation logic without justification.
4. Include rationale for any schema change.
5. Avoid vague “agent intelligence” claims.
6. Prefer measurable contracts, explicit state transitions, and reproducible examples.

---

## Non-Goals

This project does not claim to provide:

* artificial general intelligence,
* model consciousness,
* automatic truth,
* unrestricted self-modification,
* fully autonomous production deployment,
* or a replacement for human judgment.

It is a governance and orchestration protocol stack.

---

## Security and Safety

This stack is designed to reduce risks such as:

* hallucinated artifacts,
* ungrounded claims,
* unsafe merges,
* runaway agent loops,
* stale branches,
* hidden policy conflicts,
* poisoned learning,
* unbounded tool use,
* and loss of reproducibility.

Security review is still required before using any reference implementation in production.

---

## License

Recommended license:

```text
Apache-2.0
```

Apache-2.0 is preferred because this project may eventually include reference implementations, interfaces, schemas, and reusable protocol components.

MIT is also acceptable if maximum simplicity is preferred.

---

## Acknowledgment

This project began as an attempt to understand, formalize, and improve the emerging workflow of multi-model LLM collaboration.

It is released publicly in the spirit of open tooling, shared governance, and disciplined experimentation.

The goal is simple:

> Make multi-agent systems less magical, less fragile, less contaminated, and more accountable.

---

## Short Description

```text
Production-oriented governance protocols for auditable, contamination-safe multi-agent LLM orchestration.
```

---

## Keywords

```text
multi-agent
LLM
agent orchestration
prompt orchestration
governance
artifact boundaries
dependency notes
anti-contamination
strategy learning
policy composition
value of information
collaboration health
temporal governance
rollback
AI agents
LLM agents
```


