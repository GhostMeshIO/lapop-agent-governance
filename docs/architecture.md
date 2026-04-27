# Architecture

## High-Level Architecture

`lapop-agent-governance` is built as a layered control-plane architecture for multi-agent LLM orchestration.

The system separates execution from governance.

```text
User Task
   ↓
ExecutionContext
   ↓
Policy Composition
   ↓
Task Decomposition
   ↓
Agent Assignment + Artifact Boundaries
   ↓
Worker Execution
   ↓
Validation + Audit
   ↓
Merge / Preserve Disagreement / Repair
   ↓
Execution Record
   ↓
Strategy Learning + Collaboration Health + Temporal Governance
````

## Control Plane vs Data Plane

### Data Plane

The data plane performs work.

It includes:

* task execution,
* tool calls,
* artifact generation,
* validation,
* audit,
* merge,
* repair,
* and final output production.

### Control Plane

The control plane governs work.

It includes:

* policy composition,
* artifact boundary enforcement,
* value-of-information decisions,
* collaboration health monitoring,
* strategy learning,
* temporal rollback,
* contamination control,
* and reproducibility.

This separation prevents worker agents from silently changing the rules of their own execution.

## Layer Stack

### LAPOP-003 — Execution Backbone

The LLM-Agent Prompt Orchestration Protocol coordinates the lifecycle of a task.

Responsibilities:

* parse task intent,
* create execution state,
* decompose work,
* route agents,
* validate artifacts,
* audit outputs,
* merge results,
* repair failures,
* write memory,
* preserve provenance,
* support reproducibility.

LAPOP is the main execution stack.

### CDO-003 — Coordination Discipline Overlay

The Coordination Discipline Overlay enforces lane discipline.

Responsibilities:

* assign artifact ownership,
* define allowed and forbidden actions,
* enforce artifact boundaries,
* require dependency notes for cross-boundary changes,
* detect boundary violations,
* preserve semantic integrity,
* manage ownership transfer,
* prevent architectural drift.

CDO keeps worker agents from trampling each other.

### PCL-003 — Policy Composition Layer

The Policy Composition Layer converts multiple policy sources into a single effective policy set.

Responsibilities:

* ingest policy fragments,
* order policies deterministically,
* resolve conflicts,
* build scoped policy maps,
* manage exception windows,
* record policy attestations,
* expose `is_action_allowed`.

PCL is the deterministic policy arbiter.

### SLL-003 — Strategy Learning Layer

The Strategy Learning Layer improves how the system works over time.

Responsibilities:

* collect clean execution outcomes,
* quarantine contaminated runs,
* learn routing priors,
* mine repair patterns,
* adjust validation thresholds,
* track merge regret,
* evaluate patches against benchmarks,
* commit reversible strategy updates.

SLL learns strategy, not artifact content.

### EIL-003 — Economic Intelligence Layer

The Economic Intelligence Layer decides whether further work is worth the cost.

Responsibilities:

* estimate value of information,
* compare expected gain against cost and latency,
* model tool trust,
* model human review cost,
* track economic regret,
* rank next actions,
* decide when to stop.

EIL prevents endless low-value computation.

### CHL-003 — Collaboration Health Layer

The Collaboration Health Layer measures structural collaboration quality.

Responsibilities:

* track boundary health,
* measure note density,
* detect cross-boundary churn,
* maintain trust graphs,
* preserve important disagreement,
* detect emergent coordination risk,
* emit intervention signals.

CHL is the collaboration immune system.

### TGL-003 — Temporal Governance Layer

The Temporal Governance Layer manages time, aging, rollback, and long-running cycles.

Responsibilities:

* assign TTLs,
* archive stale items,
* reopen archived items,
* detect deadlocks,
* detect oscillation,
* preserve speculative branches,
* coordinate rollback,
* schedule constitutional review,
* report governance debt.

TGL keeps long-running work from rotting.

## Core Data Objects

### ExecutionContext

Shared execution identity passed across layers.

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

Defines what an agent can and cannot do.

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

Structured request for cross-boundary changes.

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

The sealed record of a run.

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

## Execution Flow

### 1. Task Intake

The user submits a task.

The system creates an `ExecutionContext`.

### 2. Policy Compilation

PCL compiles the effective policy set for the current context.

If the action is disallowed, execution is blocked or escalated.

### 3. Task Decomposition

LAPOP decomposes the task into nodes.

Each node receives:

* artifact target,
* agent class,
* constraints,
* side-effect level,
* validation requirements.

### 4. Agent Assignment

CDO assigns artifact boundaries.

Each agent receives:

* owned artifact,
* allowed actions,
* forbidden actions,
* output contract,
* dependency note rules.

### 5. Execution

Workers produce artifacts or notes.

Workers do not edit outside their boundary.

### 6. Validation

Outputs are checked for:

* syntax validity,
* schema validity,
* completeness,
* boundary compliance,
* evidence grounding,
* interface stability,
* semantic integrity.

### 7. Audit

Critic or audit layers inspect for:

* hallucination,
* unsupported claims,
* contamination,
* architectural drift,
* unsafe side effects,
* hidden contradictions.

### 8. Merge or Preserve Disagreement

Valid outputs may be merged.

If disagreement is useful or unresolved, CHL may preserve it as a first-class disagreement artifact.

### 9. Repair

Invalid outputs may trigger repair.

Repairs must cite original failures, not hallucinated critique.

### 10. Record Sealing

The execution record is sealed with validation and audit results.

### 11. Learning and Governance

If `contamination_free = true`, SLL may learn from the run.

CHL updates collaboration health.

TGL updates temporal state, stale items, rollback candidates, and oscillation status.

## Cross-Layer Interface Matrix

| Producer  | Consumer                 | Interface                                         |
| --------- | ------------------------ | ------------------------------------------------- |
| LAPOP     | CDO                      | Agent assignment map, task DAG, artifacts         |
| CDO       | LAPOP                    | Boundary validation, dependency notes             |
| PCL       | LAPOP/CDO/EIL            | `is_action_allowed`                               |
| LAPOP     | SLL                      | Clean execution outcomes                          |
| LAPOP     | CHL                      | Execution records, validation reports, merge logs |
| LAPOP/CDO | TGL                      | artifact lineage, checkpoints, dependency notes   |
| TGL       | CHL                      | oscillation status                                |
| CHL       | Router/Merger/Supervisor | intervention signals                              |
| SLL       | Router/Repair/Validation | strategy patches                                  |
| EIL       | Supervisor/Router        | value-of-information decisions                    |

## Architectural Invariants

1. Workers do not mutate unowned artifacts.
2. Cross-boundary changes require dependency notes.
3. Policy conflicts resolve deterministically or block by default.
4. Contaminated runs do not update strategy or trust.
5. Learning is explicit, reversible, and benchmark-gated.
6. Human review is scarce, not free.
7. Disagreement can be preserved.
8. Rollback must preserve lineage.
9. Speculative branches cannot silently contaminate mainline.
10. Governance layers do not directly rewrite artifacts unless explicitly authorized.

## Minimal MVP Architecture

The first implementation should be intentionally small:

```text
1. ExecutionContext
2. AgentAssignment
3. ArtifactBoundary
4. DependencyNote
5. ValidationReport
6. ExecutionRecord
7. Supervisor merge/reject
8. Policy query stub
9. Simple checkpoint
10. Basic rollback snapshot
```

Everything else can be layered in after the base cycle is stable.
