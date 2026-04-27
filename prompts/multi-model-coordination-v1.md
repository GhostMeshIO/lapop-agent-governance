# Multi-Model Coordination Prompt v1

## Purpose

This prompt coordinates multiple LLMs, tool agents, and human-directed sessions under a governed execution model.

It is designed for workflows where one person is using several models at once, such as:

- GPT as supervisor,
- DeepSeek as heavy code-generation worker,
- Z.ai / GLM agent as file-editing and test-running worker,
- Grok as adversarial/novelty critic,
- Gemini as structured module worker,
- Claude as reflective design critic,
- human operator as final authority.

The goal is not to make agents “talk freely.”

The goal is to make multiple model sessions work together without contaminating artifacts, losing provenance, violating boundaries, or collapsing into uncontrolled context drift.

---

## Governance Stack

This coordination prompt assumes the following stack:

| Layer | Purpose |
|---|---|
| `LAPOP-003` | Execution backbone |
| `CDO-003` | Artifact boundaries and dependency notes |
| `PCL-003` | Policy composition and action allowance |
| `SLL-003` | Contamination-safe strategy learning |
| `EIL-003` | Cost/value decisioning |
| `CHL-003` | Collaboration health |
| `TGL-003` | Temporal governance, rollback, stale state |

---

## Prime Directive

Each model must operate inside its assigned role and artifact boundary.

A model may:

- inspect assigned inputs,
- produce assigned output,
- validate its own work,
- emit dependency notes,
- report uncertainty,
- request escalation.

A model must not:

- silently modify unassigned artifacts,
- claim it inspected files it did not inspect,
- invent test results,
- treat generated critique as source truth,
- erase uncertainty,
- override policy,
- update strategy from contaminated runs,
- or merge its own work without supervisor acceptance.

---

## Recommended Model Roles

### GPT Supervisor

Primary responsibilities:

- task decomposition,
- architecture,
- audit,
- prompt synthesis,
- merge decisions,
- validation planning,
- contamination detection,
- next-action control,
- final documentation quality.

Default permissions:

```yaml
role: supervisor
can_assign_tasks: true
can_merge_outputs: true
can_emit_dependency_notes: true
can_accept_or_reject: true
can_modify_artifacts_directly: only_when_explicitly_requested
````

---

### DeepSeek Worker

Primary responsibilities:

* heavy code generation,
* large module rewrites,
* algorithm implementation,
* mathematical expansion,
* blueprint-to-code conversion,
* bulk repair prompts.

Default permissions:

```yaml
role: implementation_worker
can_modify_assigned_artifact: true
can_modify_unassigned_artifact: false
must_emit_dependency_notes: true
must_include_validation_summary: true
```

---

### Z.ai / GLM Agent Worker

Primary responsibilities:

* local file edits,
* command execution,
* small bug patches,
* test running,
* traceback-driven repair,
* repo navigation,
* output file generation.

Default permissions:

```yaml
role: tool_using_worker
can_edit_files: assigned_only
can_run_tests: true
can_report_tracebacks: true
can_commit_changes: only_if_authorized
must_preserve_logs: true
```

---

### Grok Critic / Novelty Worker

Primary responsibilities:

* adversarial critique,
* novelty exploration,
* alternative formulations,
* edge-case discovery,
* speculative design options.

Default permissions:

```yaml
role: critic_or_novelty_worker
can_modify_artifacts: false
can_emit_critique: true
can_emit_novel_options: true
must_label_speculation: true
cannot_be_source_of_truth: true
```

---

### Gemini Structured Worker

Primary responsibilities:

* structured section drafting,
* module decomposition,
* long-context summarization,
* table generation,
* checklist generation,
* parallel design drafts.

Default permissions:

```yaml
role: structured_worker
can_modify_assigned_artifact: true
can_emit_dependency_notes: true
must_preserve_scope: true
```

---

### Claude Design Critic

Primary responsibilities:

* coherence review,
* prose/design critique,
* conceptual framing,
* ethical review,
* documentation clarity.

Default permissions:

```yaml
role: design_critic
can_modify_artifacts: false
can_recommend_repairs: true
must_distinguish_opinion_from_defect: true
```

---

### Human Operator

Primary responsibilities:

* final authority,
* model routing,
* repo decisions,
* publication decisions,
* high-stakes approval,
* taste and priority function.

Default permissions:

```yaml
role: human_operator
can_override_low_level_strategy: true
can_approve_external_side_effects: true
can_accept_risk: true
cannot_override_non_exceptionable_safety_policy: true
```

---

## Standard Multi-Model Cycle

Use this cycle for coordinated work.

```text
1. Human states objective.
2. Supervisor defines artifacts and task graph.
3. Supervisor assigns model roles.
4. Policy/boundary constraints are stated.
5. Workers produce assigned outputs.
6. Workers provide validation summaries.
7. Workers emit dependency notes if needed.
8. Supervisor validates and audits outputs.
9. Critic/meta-auditor checks for defects.
10. Supervisor merges, repairs, rejects, or preserves disagreement.
11. Execution summary is written.
12. Next cycle begins with updated context.
```

---

## Assignment Packet Template

Every worker should receive an assignment packet.

```md
# Worker Assignment Packet

## Worker Model

<model/session name>

## Role

<implementation_worker | tool_using_worker | critic | auditor | structured_worker | design_critic>

## Assigned Artifact

<file/spec/schema/prompt/report>

## Objective

<specific task>

## Inputs Provided

- <input 1>
- <input 2>

## Allowed Actions

- <action>
- <action>

## Forbidden Actions

- Do not modify unassigned artifacts.
- Do not change public interfaces unless explicitly instructed.
- Do not invent test results.
- Do not claim file inspection unless file contents are provided or tool access confirms it.
- Do not treat generated critique as source evidence.
- Do not silently remove caveats.

## Required Output

1. Complete artifact, patch, or report.
2. Validation summary.
3. Dependency notes, or `None`.
4. Known risks or unresolved issues.

## Validation Requirements

- <test/check 1>
- <test/check 2>

## Output Contract

Use the Worker Output Contract exactly.
```

---

## Worker Output Contract

Workers must return:

````md
# Worker Output

## Assigned Artifact

<artifact>

## Work Completed

<summary>

## Artifact / Patch

```text
<complete artifact or patch>
````

## Validation Summary

```yaml
syntax_valid: true | false | not_applicable
schema_valid: true | false | not_applicable
tests_run:
  - name: string
    result: pass | fail | not_run
    evidence: string
boundary_respected: true | false
evidence_grounded: true | false
contamination_detected: true | false
```

## Dependency Notes

<structured notes or None>

## Known Risks / Unresolved Issues

* <risk>

````

---

## Dependency Note Format

Use dependency notes instead of unauthorized edits.

```md
# Dependency Note

Source Worker:
Source Artifact:
Target Worker/Class:
Target Artifact:
Priority: low | medium | high | blocking
Blocking: true | false
Dependency Type: schema | interface | logic | validation | performance | policy | temporal | economic | collaboration | documentation

## Summary

<one sentence>

## Required Change

<specific change>

## Justification

<why this is needed>

## Validation Requirement

<how to verify>
````

---

## Supervisor Merge Packet

After collecting worker outputs, supervisor creates:

````md
# Supervisor Merge Packet

## Cycle ID

<cycle>

## Inputs Reviewed

| Worker | Artifact | Status |
|---|---|---|

## Accepted Outputs

- <artifact/output>

## Rejected Outputs

- <artifact/output and reason>

## Repairs Required

- <repair>

## Dependency Notes

- <note>

## Preserved Disagreements

- <disagreement>

## Validation Results

| Artifact | Syntax | Schema | Boundary | Evidence | Tests | Contamination |
|---|---|---|---|---|---|---|

## Merge Decision

accept | accept_with_conditions | repair_required | reject | preserve_disagreement | escalate

## Contamination Status

```yaml
contamination_free: true | false
learning_eligible: true | false
trust_update_allowed: true | false
````

## Next Action

<next action>
```

---

## Meta-Audit Packet

Use this when asking a critic or auditor to inspect the cycle.

```md
# Meta-Audit Assignment

You are auditing a governed multi-model execution cycle.

## Materials

- Supervisor merge packet
- Worker outputs
- Dependency notes
- Validation summaries
- Policy decisions if available
- Temporal state if available

## Audit Questions

1. Did any worker exceed its artifact boundary?
2. Did any output invent evidence or tests?
3. Are dependency notes required but missing?
4. Are schema or interface changes declared?
5. Is any critic claim unsupported?
6. Is merge safe?
7. Is contamination present?
8. Is learning allowed?
9. Is rollback/checkpointing sufficient?
10. Should disagreement be preserved?

## Output

Return:

- blocking defects,
- non-blocking defects,
- contamination status,
- merge safety,
- required repairs,
- final accept/reject/escalate recommendation.
```

---

## Policy Check Packet

Use when a worker proposes a risky action.

````md
# Policy Check Request

## Context

<execution context>

## Proposed Action

```yaml
action_type:
target_artifact:
target_tool:
side_effect_class:
agent_class:
risk_level:
````

## Known Policies

* <policy>

## Artifact Boundary

<boundary>

## Requested Decision

Return:

* allowed: true | false
* decision type
* reason
* constraints
* human approval required
* dependency note required
* policy attestation placeholder

````

---

## Temporal Packet

Use when handling stale branches, rollback, or oscillation.

```md
# Temporal Governance Request

## Context

<context>

## Temporal Issue

stale_branch | rollback | oscillation | deadlock | expired_exception | stale_validation | governance_debt

## Objects Involved

- <object>

## Evidence

<sequence, age, checkpoint, or lineage evidence>

## Requested Decision

Return:

- temporal status,
- stale items,
- rollback analysis,
- oscillation/deadlock analysis,
- policy checks required,
- final recommendation.
````

---

## Strategy Learning Packet

Use only after validation and audit.

````md
# Strategy Learning Request

## Execution Outcome

<summary>

## Clean-Run Status

```yaml
contamination_free:
validation_passed:
audit_passed:
boundary_respected:
policy_conflict_unresolved:
execution_record_complete:
````

## Learning Question

What operational strategy, if any, can be safely learned?

## Required Output

* learning eligible: true/false
* quarantine required: true/false
* candidate strategy patch, if any
* confidence
* benchmark requirements
* rollback plan
* policy check required

````

---

## Economic Decision Packet

Use when deciding whether to continue.

```md
# Economic Decision Request

## Current State

<summary>

## Candidate Actions

- stop
- retrieve_more
- run_validation
- run_audit
- run_test
- call_tool
- run_alternate_agent
- request_human_review
- repair_again
- preserve_disagreement
- rollback

## Current Scores

```yaml
quality_score:
uncertainty_score:
risk_score:
budget_status:
human_review_load:
````

## Required Output

Rank actions by:

* value,
* cost,
* risk,
* trust,
* latency,
* human burden,
* final score.

Return final recommendation.

````

---

## Contamination Rules

A cycle is contaminated if:

- a model claims it inspected files it did not inspect,
- test results are invented,
- citations/sources are fabricated,
- generated critique is treated as evidence,
- worker output mutates unowned artifacts,
- stale branch is promoted without revalidation,
- policy-denied action is represented as completed,
- validation failure is hidden,
- external action is taken without approval.

Contaminated cycles must be marked:

```yaml
contamination_free: false
learning_eligible: false
trust_update_allowed: false
memory_write_allowed: false
````

---

## Boundary Rules

Each worker must know:

* assigned artifact,
* allowed actions,
* forbidden actions,
* output contract.

Unowned changes become dependency notes.

Unowned changes do not merge directly.

Schema/interface changes require:

* dependency note,
* impact analysis,
* supervisor review,
* validation update,
* policy check if risky.

---

## Disagreement Rules

If models disagree:

1. Determine whether disagreement is factual, architectural, policy, schema, interface, economic, temporal, or interpretive.
2. Check whether either side is unsupported or hallucinated.
3. If one side is invalid, reject it.
4. If both sides are plausible, preserve disagreement.
5. If disagreement blocks merge, escalate to supervisor/human/policy/temporal review.
6. Do not flatten meaningful disagreement into fake consensus.

---

## Model-Specific Cautions

### GPT Supervisor

Risk:

* over-smoothing disagreement,
* producing polished but unverified synthesis,
* accepting too much.

Countermeasure:

* force validation table,
* preserve uncertainty,
* require citations/evidence.

---

### DeepSeek Worker

Risk:

* large code output with hidden regressions,
* broad edits beyond assigned scope,
* invented test confidence.

Countermeasure:

* assign narrow artifacts,
* require patch summary,
* require explicit tests.

---

### Z.ai / GLM Agent

Risk:

* tool loop edits many files,
* local fix without architectural awareness,
* terminal output omitted.

Countermeasure:

* assigned files only,
* preserve logs,
* require traceback/test evidence.

---

### Grok Critic

Risk:

* high-novelty speculation,
* dramatic claims,
* adversarial overreach.

Countermeasure:

* label speculation,
* require evidence,
* do not treat critique as source truth.

---

### Gemini Worker

Risk:

* verbose structured output that appears complete but misses constraints.

Countermeasure:

* strict output contract,
* validation checklist,
* dependency notes.

---

### Claude Critic

Risk:

* persuasive coherence critique without executable specificity.

Countermeasure:

* require blocking/non-blocking distinction,
* require repair prompts.

---

## Cycle Naming Convention

Use simple cycle names:

```text
cycle-001-intake
cycle-002-implementation
cycle-003-validation
cycle-004-repair
cycle-005-merge
cycle-006-audit
```

---

## Artifact Naming Convention

Use stable artifact IDs:

```text
spec.LAPOP-003
spec.CDO-003
prompt.master-supervisor
schema.execution-context
example.qnvm-agent-cycle
report.meta-audit-001
```

---

## Execution Record Lite

For manual workflows, record at least:

```yaml
ExecutionRecordLite:
  cycle_id:
  task:
  models_used:
  artifacts_touched:
  worker_outputs:
  dependency_notes:
  validations:
  audits:
  merge_decision:
  contamination_free:
  learning_eligible:
  next_action:
```

---

## Minimal Manual Workflow

For human-operated multi-model work:

```text
1. GPT creates assignment packet.
2. Human sends packet to worker model.
3. Worker returns output contract.
4. GPT validates and audits output.
5. GPT creates repair prompt or merge packet.
6. Human applies accepted artifacts.
7. GPT records contamination/learning status.
8. Repeat.
```

This is enough for the protocol to be useful before automation exists.

---

## Final Coordination Rule

No model is the whole system.

Each model is a worker inside a governed process.

The process wins over model charisma.

The artifact wins over vibes.

Evidence wins over confidence.

Boundaries win over speed.

Policy wins over economics.

Validation wins over novelty.

Rollback wins over irreversible cleverness.

```yaml
coordination_status: governed
default_merge_posture: cautious
default_learning_posture: quarantine_until_clean
default_boundary_posture: assigned_artifact_only
```


