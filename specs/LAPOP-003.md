# LAPOP-003

## LLM-Agent Prompt Orchestration Protocol

```yaml
Spec_Name: LAPOP
Version: 003
Status: Production_Candidate
Layer_Type: Execution_Backbone
Project: lapop-agent-governance
````

---

## 1. Purpose

LAPOP-003 defines the runtime execution backbone for auditable, reproducible, contamination-safe multi-agent LLM orchestration.

It governs how a user task becomes:

1. a structured execution context,
2. a decomposed task graph,
3. a set of artifact-bound agent assignments,
4. validated outputs,
5. audited artifacts,
6. merge decisions,
7. repair cycles,
8. sealed execution records,
9. replayable checkpoints,
10. and safe learning signals for downstream governance layers.

LAPOP-003 is the core execution protocol underneath the Coordination Discipline Overlay (`CDO-003`) and the adaptive governance layers:

* `PCL-003` — Policy Composition Layer
* `SLL-003` — Strategy Learning Layer
* `EIL-003` — Economic Intelligence Layer
* `CHL-003` — Collaboration Health Layer
* `TGL-003` — Temporal Governance Layer

---

## 2. Design Goals

LAPOP-003 exists to make multi-agent execution:

* deterministic where possible,
* auditable,
* replayable,
* artifact-bound,
* contamination-resistant,
* validation-driven,
* supervisor-mediated,
* and safe for long-running work.

The protocol should support both:

* human-mediated workflows where users paste outputs between model sessions,
* and future automated reference runners.

---

## 3. Non-Goals

LAPOP-003 does **not** define:

* policy precedence internals,
* strategy learning internals,
* economic value-of-information internals,
* collaboration trust graph internals,
* temporal rollback internals,
* model weights,
* model consciousness,
* autonomous production deployment,
* or unrestricted self-modifying agents.

Those concerns belong to other layers.

LAPOP may emit records, events, and schemas consumed by those layers, but it does not own their internal algorithms.

---

## 4. Core Responsibilities

| Responsibility           | Description                                                                          |
| ------------------------ | ------------------------------------------------------------------------------------ |
| Task Intake              | Convert user request into a structured execution context.                            |
| Task Typing              | Classify task type, artifact type, domain, risk, and side-effect level.              |
| Decomposition            | Break complex tasks into task nodes.                                                 |
| Routing                  | Assign task nodes to agent classes or human reviewers.                               |
| Artifact Assignment      | Bind each task node to owned artifacts and permission boundaries.                    |
| Execution Coordination   | Collect worker outputs according to output contracts.                                |
| Validation               | Check syntax, schema, boundary, completeness, tests, and evidence alignment.         |
| Audit                    | Detect hallucination, contamination, unsafe claims, drift, and hidden contradiction. |
| Merge                    | Combine valid outputs or preserve disagreement.                                      |
| Repair                   | Request bounded repairs based on original validation failures.                       |
| Escalation               | Route unresolved or high-risk items to supervisor or human.                          |
| Checkpointing            | Record reproducible intermediate states.                                             |
| Execution Record Sealing | Produce a final trace of the run.                                                    |
| Memory Eligibility       | Mark outputs as eligible or ineligible for memory/learning.                          |

---

## 5. Execution State Machine

LAPOP execution moves through explicit states.

```yaml
ExecutionState:
  - created
  - typed
  - policy_checked
  - retrieved
  - decomposed
  - routed
  - assigned
  - planned
  - approved
  - executing
  - validating
  - auditing
  - merging
  - repaired
  - completed
  - partial
  - failed
  - escalated
  - cancelled
```

### State Meaning

| State            | Meaning                                                      |
| ---------------- | ------------------------------------------------------------ |
| `created`        | User task received and execution context created.            |
| `typed`          | Task and artifact types classified.                          |
| `policy_checked` | PCL has checked whether execution may proceed.               |
| `retrieved`      | Required source material or context collected.               |
| `decomposed`     | Task graph generated.                                        |
| `routed`         | Task nodes routed to agent classes.                          |
| `assigned`       | Artifact boundaries and permissions assigned.                |
| `planned`        | Workers or supervisor have prepared execution plan.          |
| `approved`       | Plan approved for execution.                                 |
| `executing`      | Workers produce artifacts or dependency notes.               |
| `validating`     | Outputs are checked against rules and schemas.               |
| `auditing`       | Deeper review for grounding, drift, contamination, and risk. |
| `merging`        | Valid outputs are merged or disagreement preserved.          |
| `repaired`       | Failed outputs have entered repair cycle.                    |
| `completed`      | Execution completed successfully.                            |
| `partial`        | Some outputs accepted; others failed or deferred.            |
| `failed`         | Execution failed without acceptable output.                  |
| `escalated`      | Human or supervisor intervention required.                   |
| `cancelled`      | Execution stopped intentionally.                             |

---

## 6. Core Schemas

### 6.1 ExecutionContext

The shared context object passed across layers.

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

---

### 6.2 TaskRequest

```yaml
TaskRequest:
  request_id: UUID
  user_prompt: string
  submitted_at: timestamp
  project_id: string
  requested_outputs: list[ArtifactType]
  constraints: list[string]
  risk_notes: optional[list[string]]
  attachments: optional[list[ArtifactRef]]
```

---

### 6.3 TaskNode

```yaml
TaskNode:
  task_node_id: UUID
  parent_task_id: optional[UUID]
  title: string
  description: string
  task_type: enum
    - analysis
    - code_generation
    - code_repair
    - schema_design
    - documentation
    - research
    - validation
    - audit
    - merge
    - planning
    - data_operation
  artifact_target: optional[string]
  required_agent_class: optional[string]
  dependencies: list[UUID]
  status: ExecutionState
  side_effect_class: SideEffectClass
  validation_requirements: list[string]
```

---

### 6.4 ArtifactType

```yaml
ArtifactType:
  - file
  - module
  - schema
  - dataset
  - interface
  - config
  - diagram
  - report
  - patch
  - prompt
  - benchmark
  - execution_record
```

---

### 6.5 SideEffectClass

```yaml
SideEffectClass:
  - read_only
  - simulated
  - advisory
  - write_internal
  - write_external
  - destructive
```

### Side-Effect Rules

| Class            | Meaning                                                     | Required Authority               |
| ---------------- | ----------------------------------------------------------- | -------------------------------- |
| `read_only`      | Inspection only                                             | Normal execution                 |
| `simulated`      | Dry-run or generated plan                                   | Normal execution                 |
| `advisory`       | Recommendation only                                         | Normal execution                 |
| `write_internal` | Writes local/internal artifact                              | Supervisor approval              |
| `write_external` | Sends or publishes externally                               | Explicit user/human approval     |
| `destructive`    | Deletes, overwrites, revokes, or causes irreversible effect | Explicit high-authority approval |

---

### 6.6 AgentAssignment

```yaml
AgentAssignment:
  assignment_id: UUID
  task_node_id: UUID
  agent_class: string
  agent_role: enum
    - planner
    - worker
    - critic
    - auditor
    - merger
    - supervisor
    - human_reviewer
  artifact_boundary: Artifact_Boundary
  permission_profile: string
  prompt_template_id: optional[string]
  assigned_at: timestamp
  status: enum [pending, active, completed, failed, cancelled]
```

---

### 6.7 ArtifactRef

```yaml
ArtifactRef:
  artifact_id: string
  artifact_type: ArtifactType
  path: optional[string]
  version_id: optional[UUID]
  hash: optional[string]
  created_by: optional[string]
  created_at: optional[timestamp]
```

---

### 6.8 ValidationReport

```yaml
ValidationReport:
  report_id: UUID
  artifact_id: string
  task_node_id: UUID
  syntax_valid: bool
  schema_valid: bool
  boundary_respected: bool
  tests_passed: optional[bool]
  evidence_grounded: bool
  completeness_score: float
  semantic_integrity_score: float
  contamination_detected: bool
  validation_errors: list[ValidationError]
  created_at: timestamp
```

---

### 6.9 ValidationError

```yaml
ValidationError:
  error_id: UUID
  error_type: enum
    - syntax_error
    - schema_error
    - boundary_violation
    - unsupported_claim
    - missing_section
    - failed_test
    - interface_break
    - architectural_drift
    - contamination
    - unsafe_side_effect
  severity: enum [low, medium, high, blocking]
  description: string
  evidence: optional[string]
  repair_hint: optional[string]
```

---

### 6.10 AuditReport

```yaml
AuditReport:
  audit_id: UUID
  artifact_id: string
  task_node_id: UUID
  audit_type: enum
    - hallucination_audit
    - evidence_audit
    - security_audit
    - policy_audit
    - semantic_integrity_audit
    - contamination_audit
    - merge_audit
  findings: list[AuditFinding]
  confidence: float
  contamination_detected: bool
  recommended_action: enum [accept, repair, reject, escalate, preserve_disagreement]
  created_at: timestamp
```

---

### 6.11 AuditFinding

```yaml
AuditFinding:
  finding_id: UUID
  severity: enum [low, medium, high, critical]
  title: string
  description: string
  evidence: optional[string]
  affected_artifact: optional[string]
  recommended_fix: optional[string]
```

---

### 6.12 MergeLog

```yaml
MergeLog:
  merge_id: UUID
  task_node_ids: list[UUID]
  artifacts_considered: list[ArtifactRef]
  artifacts_accepted: list[ArtifactRef]
  artifacts_rejected: list[ArtifactRef]
  disagreement_records: list[UUID]
  merge_method: enum
    - direct_accept
    - supervisor_merge
    - confidence_weighted
    - evidence_weighted
    - human_reviewed
    - preserved_disagreement
  conflict_resolution_notes: list[string]
  merged_by: string
  created_at: timestamp
```

---

### 6.13 Checkpoint

```yaml
Checkpoint:
  checkpoint_id: UUID
  execution_id: UUID
  cycle_id: string
  state: ExecutionState
  input_hash: string
  tool_output_hash: optional[string]
  artifact_hash: optional[string]
  validation_hash: optional[string]
  policy_attestation_id: optional[UUID]
  strategy_ledger_head: optional[string]
  replay_eligible: bool
  cacheable: bool
  invalidation_triggers: list[string]
  created_at: timestamp
```

---

### 6.14 Execution_Record

```yaml
Execution_Record:
  execution_id: UUID
  context: ExecutionContext
  task_request: TaskRequest
  task_graph: list[TaskNode]
  agent_assignments: list[AgentAssignment]
  artifacts_produced: list[ArtifactRef]
  validation_reports: list[ValidationReport]
  audit_reports: list[AuditReport]
  merge_log: optional[MergeLog]
  dependency_notes: list[Dependency_Note]
  checkpoints: list[Checkpoint]
  side_effects: list[SideEffectRecord]
  contamination_free: bool
  failure_events: list[FailureEvent]
  reproducibility_profile: ReproducibilityProfile
  created_at: timestamp
  completed_at: optional[timestamp]
```

---

### 6.15 FailureEvent

```yaml
FailureEvent:
  failure_id: UUID
  task_node_id: optional[UUID]
  artifact_id: optional[string]
  failure_type: enum
    - timeout
    - schema_mismatch
    - tool_error
    - budget_exceeded
    - ambiguity_unresolved
    - dependency_failure
    - policy_denied
    - boundary_violation
    - validation_failed
    - audit_failed
    - contamination_detected
    - merge_failed
    - human_escalation_required
  severity: enum [low, medium, high, critical]
  description: string
  recoverable: bool
  repair_attempted: bool
  created_at: timestamp
```

---

### 6.16 SideEffectRecord

```yaml
SideEffectRecord:
  side_effect_id: UUID
  task_node_id: UUID
  side_effect_class: SideEffectClass
  target: string
  action: string
  approved_by: optional[string]
  result: enum [simulated, completed, denied, failed, cancelled]
  created_at: timestamp
```

---

### 6.17 ReproducibilityProfile

```yaml
ReproducibilityProfile:
  model_id: optional[string]
  model_version: optional[string]
  prompt_template_version: optional[string]
  retrieval_snapshot_id: optional[string]
  temperature: optional[float]
  top_p: optional[float]
  deterministic_mode: bool
  seed: optional[int]
  tool_versions: map[string]string
  environment_hash: optional[string]
  replay_notes: optional[string]
```

---

## 7. Core Flow

### 7.1 Intake Flow

```text
User Request
  → TaskRequest
  → ExecutionContext
  → Initial Policy Check
  → Task Typing
```

Required outputs:

* `TaskRequest`
* `ExecutionContext`
* initial `ExecutionState = created`

---

### 7.2 Typing Flow

The task is classified by:

* domain,
* artifact type,
* risk,
* side-effect class,
* ambiguity,
* required validation type,
* expected output structure.

```yaml
TaskTypingResult:
  task_type: string
  artifact_types: list[ArtifactType]
  side_effect_class: SideEffectClass
  risk_level: enum [low, medium, high, critical]
  ambiguity_level: enum [low, medium, high]
  required_validators: list[string]
```

---

### 7.3 Decomposition Flow

Complex requests are decomposed into task nodes.

Decomposition should preserve:

* dependencies,
* artifact boundaries,
* expected output contracts,
* validation requirements,
* merge expectations.

---

### 7.4 Routing Flow

Task nodes are routed to agents based on:

* capability,
* permission profile,
* artifact ownership,
* policy allowance,
* economic decision,
* collaboration health,
* temporal availability.

Routing may consume signals from:

* `PCL-003`
* `SLL-003`
* `EIL-003`
* `CHL-003`
* `TGL-003`

---

### 7.5 Assignment Flow

Each routed node receives an `AgentAssignment`.

Assignment must include:

* agent role,
* artifact boundary,
* permission profile,
* output contract,
* validation requirements.

---

### 7.6 Execution Flow

Workers produce output according to the required output contract.

Default worker output contract:

```text
=== ARTIFACT UPDATED: <artifact_id> ===

<complete artifact content>

=== VALIDATION SUMMARY ===

<self-declared validation status>

=== DEPENDENCY NOTES ===

<structured dependency notes or None>
```

Workers must not silently edit outside their assigned artifact.

---

### 7.7 Validation Flow

Validation checks:

* syntax validity,
* schema validity,
* boundary compliance,
* required sections,
* dependency note structure,
* evidence grounding,
* interface stability,
* semantic integrity,
* test execution if applicable.

Validation produces `ValidationReport`.

---

### 7.8 Audit Flow

Audit checks:

* unsupported claims,
* hallucinated dependencies,
* speculative material treated as evidence,
* contamination risk,
* policy conflict,
* architectural drift,
* unsafe side effects,
* merge risk,
* reproducibility gaps.

Audit produces `AuditReport`.

---

### 7.9 Merge Flow

Valid artifacts may be merged.

If outputs conflict, the merger may:

* accept one,
* combine compatible parts,
* request repair,
* preserve disagreement,
* escalate to supervisor,
* escalate to human.

Merge produces `MergeLog`.

---

### 7.10 Repair Flow

Repair prompts must reference original validation/audit failures.

Repair prompts must not treat hallucinated critic output as source truth.

Repair outputs are revalidated.

Repair attempts are recorded.

---

### 7.11 Completion Flow

The run completes when:

* required artifacts are accepted,
* unresolved blocking notes are resolved or escalated,
* contamination status is determined,
* execution record is sealed.

---

## 8. Anti-Contamination Rules

### 8.1 Contamination Sources

Contamination may arise from:

* hallucinated citations,
* unsupported claims,
* generated speculation treated as evidence,
* critic output treated as authority,
* failed repair output entering memory,
* unverified external artifacts,
* stale branch revival without validation,
* policy-violating outputs,
* boundary-violating edits.

### 8.2 Contamination Effects

If contamination is detected:

```yaml
contamination_free: false
```

The run must not update:

* strategy learning,
* trust graph,
* validated memory,
* collaboration health,
* routing priors,
* benchmark success records.

### 8.3 Contamination Repair

A contaminated run may be:

* rejected,
* repaired,
* quarantined,
* partially salvaged,
* escalated,
* converted to advisory output only.

---

## 9. Memory Eligibility

An artifact may enter validated memory only if:

* validation passed,
* audit passed,
* contamination_free is true,
* policy allowed memory write,
* provenance is recorded,
* source evidence is preserved.

Generated output may not become source evidence for itself.

---

## 10. Integration With CDO-003

LAPOP delegates artifact lane discipline to CDO.

CDO provides:

* artifact boundaries,
* permission profiles,
* dependency note schemas,
* violation taxonomy,
* semantic integrity policies,
* ownership transfer rules.

LAPOP consumes CDO outputs during:

* assignment,
* validation,
* merge,
* repair,
* execution record sealing.

---

## 11. Integration With PCL-003

PCL provides policy checks.

LAPOP calls:

```text
is_action_allowed(context, action) -> allowed, reason, constraint
```

Policy checks are required before:

* tool calls,
* artifact writes,
* memory writes,
* external side effects,
* destructive actions,
* exception windows,
* publication,
* learning updates.

---

## 12. Integration With SLL-003

SLL consumes clean execution records.

LAPOP must expose:

* routing outcomes,
* repair outcomes,
* validation outcomes,
* audit outcomes,
* merge outcomes,
* human review outcomes,
* benchmark outcomes,
* contamination status.

SLL must ignore any run where:

```yaml
contamination_free: false
```

---

## 13. Integration With EIL-003

EIL supports value-aware decisioning.

LAPOP may call EIL before:

* additional retrieval,
* another audit,
* another tool call,
* alternate agent execution,
* human review,
* extended repair loop,
* early stop.

EIL returns:

* recommended action,
* expected information gain,
* expected cost,
* expected latency,
* contamination risk,
* stop/probe decision.

---

## 14. Integration With CHL-003

CHL consumes collaboration records.

LAPOP emits:

* execution records,
* validation reports,
* audit reports,
* merge logs,
* dependency notes,
* boundary violation events,
* agent assignment maps.

CHL returns:

* collaboration health reports,
* trust graph summaries,
* disagreement preservation advice,
* emergence risk signals,
* intervention signals.

---

## 15. Integration With TGL-003

TGL consumes temporal execution state.

LAPOP emits:

* checkpoints,
* task state changes,
* artifact lineage,
* dependency note lifecycle updates,
* branch state,
* rollback candidates.

TGL returns:

* stale item warnings,
* oscillation status,
* rollback recommendations,
* branch archive/reopen decisions,
* governance debt signals.

---

## 16. Failure Modes

| Failure                | Description                          | Default Response                     |
| ---------------------- | ------------------------------------ | ------------------------------------ |
| Boundary violation     | Agent edits unowned artifact         | Reject or salvage as dependency note |
| Grounding failure      | Unsupported claim                    | Reject or strip unsupported claim    |
| Schema mismatch        | Output violates schema               | Repair or reject                     |
| Tool error             | Tool fails or returns invalid output | Retry or fallback                    |
| Timeout                | Execution exceeds time               | Retry, split, or escalate            |
| Policy denied          | PCL denies action                    | Stop or escalate                     |
| Contamination detected | Run unsafe for learning/memory       | Quarantine                           |
| Merge conflict         | Outputs incompatible                 | Preserve disagreement or escalate    |
| Repair loop            | Repeated failed repairs              | Stop and escalate                    |
| Oscillation            | Artifact flips repeatedly            | TGL intervention                     |
| Human review overload  | Too many escalations                 | EIL prioritization                   |

---

## 17. Abuse and Misconfiguration Risks

### 17.1 Agent Overreach

Agents may attempt to edit outside their assigned boundary.

Mitigation:

* CDO boundary validation,
* supervisor rejection,
* dependency note conversion.

### 17.2 Policy Ambiguity

Conflicting policies may permit unsafe actions.

Mitigation:

* PCL block-by-default,
* conflict proofs,
* supervisor review.

### 17.3 Contaminated Learning

Bad runs may poison strategy.

Mitigation:

* contamination flags,
* SLL quarantine gate,
* benchmark regression gate.

### 17.4 Merge Laundering

Bad content may enter mainline through merge.

Mitigation:

* validation reports,
* audit reports,
* merge logs,
* disagreement preservation.

### 17.5 Human Review Abuse

System may over-escalate to humans.

Mitigation:

* EIL human review cost model,
* escalation thresholds,
* priority queues.

### 17.6 Silent Drift

Small changes may alter architecture over time.

Mitigation:

* semantic integrity checks,
* temporal consistency scores,
* TGL constitutional review.

---

## 18. Minimal Implementation Requirements

A minimal LAPOP runner must support:

1. `ExecutionContext`
2. `TaskRequest`
3. `TaskNode`
4. `AgentAssignment`
5. `Artifact_Boundary`
6. `Dependency_Note`
7. `ValidationReport`
8. `AuditReport`
9. `MergeLog`
10. `Checkpoint`
11. `Execution_Record`
12. `contamination_free` flag

---

## 19. Reference Runner Pseudocode

```python
def run_lapop_task(task_request):
    context = create_execution_context(task_request)

    policy_result = pcl.is_action_allowed(
        context=context,
        action={"type": "start_execution", "target": task_request.request_id}
    )

    if not policy_result.allowed:
        return fail_execution(context, "policy_denied", policy_result.reason)

    typing = type_task(task_request)
    task_graph = decompose_task(task_request, typing)

    assignments = route_and_assign(task_graph)

    outputs = []
    for assignment in assignments:
        output = execute_worker(assignment)
        outputs.append(output)

    validation_reports = [
        validate_output(output, assignment)
        for output, assignment in zip(outputs, assignments)
    ]

    audit_reports = [
        audit_output(output, validation)
        for output, validation in zip(outputs, validation_reports)
    ]

    merge_log = merge_or_preserve_disagreement(outputs, validation_reports, audit_reports)

    contamination_free = determine_contamination_status(validation_reports, audit_reports)

    record = seal_execution_record(
        context=context,
        task_request=task_request,
        task_graph=task_graph,
        assignments=assignments,
        outputs=outputs,
        validation_reports=validation_reports,
        audit_reports=audit_reports,
        merge_log=merge_log,
        contamination_free=contamination_free,
    )

    return record
```

---

## 20. Open Questions

1. What is the canonical schema language: JSON Schema, YAML Schema, Pydantic, or all three?
2. Should `Execution_Record` be stored as JSONL, SQLite rows, or both?
3. How strict should validation be before reference implementation v1.0?
4. How should human review identity be represented?
5. How much of PCL should be implemented before the runner?
6. Should the first runner support real model APIs or manual paste-in sessions?
7. Should prompts be treated as artifacts with lineage?
8. How should benchmark suites be versioned?
9. What is the minimal replay format?
10. How should artifact hashes handle generated markdown?

---

## 21. Version Notes

```yaml
Version: LAPOP-003
Status: Production_Candidate
Primary_Function: Multi-agent execution backbone
Depends_On:
  - CDO-003
Interfaces_With:
  - PCL-003
  - SLL-003
  - EIL-003
  - CHL-003
  - TGL-003
```

---

## 22. Summary

LAPOP-003 is the execution backbone of the stack.

It does not try to make agents magical.

It makes their work:

* bounded,
* validated,
* audited,
* mergeable,
* repairable,
* replayable,
* and safe to learn from.

The protocol exists because serious multi-agent work needs more than prompts.

It needs governance.


