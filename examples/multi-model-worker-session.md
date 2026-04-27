# Multi-Model Worker Session Example

## Purpose

This example shows how one human operator can coordinate multiple LLM sessions under `lapop-agent-governance`.

It is designed for workflows where the operator uses several models at once:

- GPT as supervisor,
- DeepSeek as implementation worker,
- Z.ai / GLM as tool-using file editor,
- Grok as novelty/adversarial critic,
- Claude as coherence/design critic,
- Gemini as structured documentation worker,
- human operator as final authority.

The goal is not to make models “compete.”

The goal is to make their outputs useful without letting context drift, hallucinated evidence, boundary violations, or unsupported critique contaminate the project.

---

## Scenario

The human operator is building a governed agent framework repository.

The current task is:

```text
Create schemas, prompts, specs, docs, and examples for lapop-agent-governance.
````

The operator wants to parallelize work:

```yaml id="d7xymt"
parallel_sessions:
  GPT:
    role: master_supervisor
  DeepSeek:
    role: heavy_implementation_worker
  Z.ai:
    role: repo_agent_and_file_editor
  Grok:
    role: novelty_critic
  Claude:
    role: coherence_critic
  Gemini:
    role: structured_documentation_worker
```

The danger:

```yaml id="zaa34h"
risks:
  - models modify overlapping files
  - critics invent issues not grounded in artifacts
  - workers claim tests were run when they were not
  - schema/interface changes happen without dependency notes
  - polished synthesis hides disagreement
  - old session context contaminates new artifacts
  - one model's style dominates architecture without validation
```

---

## Cycle ID

```yaml id="z5xc0l"
cycle_id: cycle-010-multi-model-schema-session
project_id: lapop-agent-governance
execution_mode: assisted_manual
side_effect_class: write_internal
risk_level: medium
```

---

## Execution Context

```yaml id="0ukmc8"
project_id: lapop-agent-governance
repository_id: GhostMeshIO/lapop-agent-governance
session_id: session-2026-04-27-multimodel
cycle_id: cycle-010-multi-model-schema-session
run_id: run-multimodel-010
branch_id: main
artifact_id: schema.batch
artifact_path: schemas/
artifact_type: schema
agent_class: supervisor
agent_role: supervisor
execution_mode: assisted_manual
side_effect_class: write_internal
risk_level: medium
policy_attestation_id: null
strategy_ledger_head: null
temporal_position:
  cycle_index: 10
  branch_depth: 0
  replay_index: 0
governance_stack:
  lapop_version: LAPOP-003
  cdo_version: CDO-003
  pcl_version: PCL-003
  sll_version: SLL-003
  eil_version: EIL-003
  chl_version: CHL-003
  tgl_version: TGL-003
contamination_state:
  contamination_free: true
  contamination_status: clean
  contamination_sources: []
  learning_eligible: false
  trust_update_allowed: false
  memory_write_allowed: false
permissions:
  allowed_actions:
    - read
    - draft
    - propose_change
    - patch_assigned_artifact
    - validate_yaml
  forbidden_actions:
    - modify_unassigned_artifacts
    - fabricate_test_results
    - publish_external_without_approval
    - merge_without_supervisor_review
tags:
  - multi-model
  - schemas
  - prompts
  - governance
```

---

## Model Role Table

| Model / Session | Role                  | Strength                            | Main Risk                  | Boundary                                |
| --------------- | --------------------- | ----------------------------------- | -------------------------- | --------------------------------------- |
| GPT             | Master Supervisor     | Architecture, merge, audit, prompts | Over-smoothing conflict    | May assign and merge, but must validate |
| DeepSeek        | Implementation Worker | Large code/schema generation        | Overbroad edits            | Assigned artifact only                  |
| Z.ai / GLM      | Tool Worker           | Local edits and tests               | File sprawl                | Assigned files only                     |
| Grok            | Novelty Critic        | Edge cases and adversarial ideas    | Speculation as truth       | Critique only, no merge authority       |
| Claude          | Design Critic         | Coherence and prose critique        | Persuasive vagueness       | Critique only                           |
| Gemini          | Structured Worker     | Tables/docs/checklists              | Verbose false completeness | Assigned artifact only                  |
| Human           | Operator              | Taste, priority, final approval     | Manual copy errors         | Final authority                         |

---

## Artifact Assignment Plan

```yaml id="hri2l5"
assignments:
  GPT:
    artifact: supervisor_merge_packet
    permissions:
      - assign_tasks
      - review_outputs
      - create_final_merge
  DeepSeek:
    artifact: schemas/execution-record.schema.yaml
    permissions:
      - draft_schema
      - propose_patch
    forbidden:
      - edit_policy_attestation_schema
      - edit_strategy_patch_schema
  Z.ai:
    artifact: repository_files
    permissions:
      - create_files_from_accepted_content
      - run_yaml_validation
      - report_terminal_output
    forbidden:
      - invent_validation_result
      - modify_unapproved_files
  Grok:
    artifact: adversarial_schema_audit.md
    permissions:
      - critique
      - propose_edge_cases
    forbidden:
      - claim_repo_inspection_without_access
      - final_merge
  Claude:
    artifact: coherence_review.md
    permissions:
      - critique
      - identify conceptual contradictions
    forbidden:
      - rewrite accepted schemas directly
  Gemini:
    artifact: docs/glossary.md
    permissions:
      - draft_documentation
      - emit_dependency_notes
    forbidden:
      - change schemas directly
```

---

## Worker Packet — DeepSeek

```md id="bxxskw"
# Worker Assignment Packet

## Worker Model

DeepSeek

## Role

implementation_worker

## Assigned Artifact

`schemas/execution-record.schema.yaml`

## Objective

Draft the canonical Execution_Record schema for lapop-agent-governance.

## Inputs Provided

- ExecutionContext schema summary
- Dependency_Note schema summary
- LAPOP-003 requirements
- TGL rollback requirements
- SLL learning eligibility requirements
- PCL policy attestation requirements

## Allowed Actions

- Draft `schemas/execution-record.schema.yaml`
- Include references or compatible inline objects for:
  - context
  - task request
  - task graph
  - agent assignments
  - artifacts
  - validation reports
  - audit reports
  - dependency notes
  - policy refs
  - strategy refs
  - checkpoints
  - contamination
  - reproducibility

## Forbidden Actions

- Do not modify other schema files.
- Do not invent test results.
- Do not claim YAML validation was run unless actually run.
- Do not remove contamination flags.
- Do not omit policy_attestation_id or strategy_ledger_head from rollback-relevant records.
- Do not make the schema so generic that governance invariants disappear.

## Required Output

1. Complete YAML schema.
2. Validation notes.
3. Dependency notes, or `None`.
4. Known risks.

## Validation Requirements

- YAML syntax should be valid.
- Schema should include `contamination_free`.
- Schema should include policy and strategy rollback references.
- Schema should include validation and audit report arrays.
```

---

## Worker Packet — Grok Critic

```md id="3i72sb"
# Critic Assignment Packet

## Worker Model

Grok

## Role

novelty_critic

## Assigned Artifact

`adversarial_schema_audit.md`

## Objective

Find edge cases and failure modes in the current schema plan.

## Inputs Provided

- Schema file summaries only.
- No direct repo access unless explicitly provided.

## Allowed Actions

- Produce adversarial critique.
- Propose edge cases.
- Suggest missing fields.
- Identify ways contamination could slip through.
- Label speculative ideas clearly.

## Forbidden Actions

- Do not claim you inspected files not provided.
- Do not treat your critique as source evidence.
- Do not rewrite schemas directly.
- Do not declare final acceptance or rejection.
- Do not invent benchmark results.

## Required Output

1. Blocking defects.
2. Non-blocking concerns.
3. Speculative improvements.
4. Dependency notes if cross-artifact changes are needed.
5. Confidence level for each claim.
```

---

## Worker Packet — Z.ai / GLM

```md id="whkqxn"
# Tool Worker Assignment Packet

## Worker Model

Z.ai / GLM Agent

## Role

tool_using_worker

## Assigned Artifact

Accepted schema files only.

## Objective

Create repository files from supervisor-accepted content and run validation.

## Inputs Provided

- Accepted file contents from supervisor.
- Target paths.

## Allowed Actions

- Create or overwrite only accepted target files.
- Run YAML parser.
- Run JSON Schema validation if tooling exists.
- Report terminal output.

## Forbidden Actions

- Do not edit files not included in accepted target list.
- Do not “fix” architecture without dependency note.
- Do not hide parser errors.
- Do not claim validation passed if command was not run.
- Do not commit or push unless explicitly authorized.

## Required Output

1. Files written.
2. Commands run.
3. Terminal output excerpt.
4. Validation pass/fail.
5. Any dependency notes.
```

---

## Worker Packet — Claude

```md id="o0ludg"
# Design Critic Assignment Packet

## Worker Model

Claude

## Role

design_critic

## Assigned Artifact

`coherence_review.md`

## Objective

Review the schema set for coherence, readability, and conceptual contradictions.

## Inputs Provided

- Accepted schema drafts.
- Layer summaries.

## Allowed Actions

- Identify contradictions.
- Recommend clearer naming.
- Flag missing concepts.
- Suggest documentation improvements.

## Forbidden Actions

- Do not rewrite schemas directly.
- Do not claim tests were run.
- Do not override policy.
- Do not treat stylistic preference as blocking defect.
- Do not merge outputs.

## Required Output

1. Blocking coherence issues.
2. Non-blocking clarity issues.
3. Suggested edits.
4. Any preserved disagreements.
```

---

## Worker Packet — Gemini

```md id="rpjop5"
# Structured Documentation Worker Assignment Packet

## Worker Model

Gemini

## Role

structured_worker

## Assigned Artifact

`docs/glossary.md`

## Objective

Draft a glossary for the governance framework.

## Inputs Provided

- README summary.
- Spec names.
- Schema names.
- Prompt names.

## Allowed Actions

- Draft glossary entries.
- Use stable terms from existing specs.
- Emit dependency notes if a term appears ambiguous.

## Forbidden Actions

- Do not alter schemas.
- Do not invent new canonical terms without marking them as proposed.
- Do not remove existing layer names.
- Do not claim repo inspection unless content was provided.

## Required Output

1. Complete `docs/glossary.md`.
2. Ambiguous terms list.
3. Dependency notes, or `None`.
```

---

## Worker Output Intake Checklist

When the human pastes worker output back to the supervisor, the supervisor checks:

```yaml id="yft6sj"
intake_checklist:
  assigned_artifact_named: true
  work_completed_summary_present: true
  artifact_or_patch_present: true
  validation_summary_present: true
  dependency_notes_present_or_none: true
  known_risks_present: true
  no_claimed_unprovided_file_inspection: true
  no_invented_test_results: true
  no_unassigned_artifact_modification: true
```

If any field is false, supervisor requests repair before merge.

---

## Example Worker Output — Grok Critic

````md id="6bqtdw"
# Worker Output

## Assigned Artifact

`adversarial_schema_audit.md`

## Work Completed

Reviewed provided summaries for schema edge cases. No repo inspection performed.

## Artifact / Patch

### Blocking Defects

1. `Execution_Record` must not allow strategy learning if `validation_reports` is empty.
   - Confidence: medium
   - Evidence: Based on provided SLL rules, not file inspection.

2. `PolicyAttestationRecord` needs explicit `restore_policy_state` key.
   - Confidence: high
   - Evidence: TGL rollback requirements require policy restoration.

### Non-Blocking Concerns

1. Collaboration health scoring may need a `confidence` field.
2. Dependency notes need stale/TTL fields for TGL.

### Speculative Improvements

1. Add `semantic_change_magnitude` to validation reports.
2. Add `human_review_outcome_id` to merge logs.

## Validation Summary

```yaml
syntax_valid: not_applicable
schema_valid: not_applicable
tests_run: []
boundary_respected: true
evidence_grounded: partial
contamination_detected: false
````

## Dependency Notes

None.

## Known Risks / Unresolved Issues

* Critique is based on summaries only, not full schema inspection.

````

Supervisor decision:

```yaml id="v59ycc"
critic_output_status: useful_but_not_source_truth
merge_action:
  - inspect each claim against actual schema
  - accept only grounded issues
  - preserve speculative improvements as backlog
contamination_free: true
learning_eligible: false
````

---

## Example Worker Output — Z.ai

````md id="x3ejxt"
# Worker Output

## Assigned Artifact

Accepted schema files:
- schemas/execution-context.schema.yaml
- schemas/dependency-note.schema.yaml

## Work Completed

Created both files from supervisor-approved content.

## Commands Run

```bash
python3 - <<'PY'
import yaml
for path in [
  "schemas/execution-context.schema.yaml",
  "schemas/dependency-note.schema.yaml",
]:
    with open(path) as f:
        yaml.safe_load(f)
    print("OK", path)
PY
````

## Terminal Output

```text
OK schemas/execution-context.schema.yaml
OK schemas/dependency-note.schema.yaml
```

## Validation Summary

```yaml
syntax_valid: true
schema_valid: not_run
tests_run:
  - name: yaml_parse
    result: pass
    evidence: "Both files loaded with yaml.safe_load."
boundary_respected: true
evidence_grounded: true
contamination_detected: false
```

## Dependency Notes

None.

## Known Risks / Unresolved Issues

* YAML syntax was checked, but JSON Schema semantics were not fully validated.

````

Supervisor decision:

```yaml id="e0psvj"
tool_output_status: accepted_with_conditions
conditions:
  - run schema validation later
  - keep YAML parse logs
contamination_free: true
trust_update_allowed: true
````

---

## Supervisor Merge Packet

````md id="zqfm2c"
# Supervisor Merge Packet

## Cycle ID

cycle-010-multi-model-schema-session

## Inputs Reviewed

| Worker | Artifact | Status |
|---|---|---|
| DeepSeek | schemas/execution-record.schema.yaml | accepted_with_repairs |
| Grok | adversarial_schema_audit.md | useful_critique_only |
| Z.ai | repository file creation | accepted_with_conditions |
| Claude | coherence_review.md | pending |
| Gemini | docs/glossary.md | pending |

## Accepted Outputs

- `schemas/execution-context.schema.yaml`
- `schemas/dependency-note.schema.yaml`
- `schemas/execution-record.schema.yaml`

## Rejected Outputs

- Any critic claim not grounded in inspected artifact content.
- Any proposed cross-schema field change without dependency note.

## Repairs Required

1. Add missing `confidence` fields where reports are scored.
2. Add `restore_policy_state` rollback key to policy attestation if absent.
3. Add semantic change field only after dependency note review.

## Dependency Notes

### Dependency Note 1

Target:
`schemas/validation-report.schema.yaml` if created later.

Required Change:
Consider adding `semantic_change_magnitude`.

Reason:
Useful for SLL/EIL/CHL gating but not yet canonical in current schema batch.

Blocking:
false.

## Preserved Disagreements

None currently blocking.

## Validation Results

| Artifact | Syntax | Schema | Boundary | Evidence | Tests | Contamination |
|---|---|---|---|---|---|---|
| execution-context | pass | not_run | pass | pass | yaml_parse | false |
| dependency-note | pass | not_run | pass | pass | yaml_parse | false |
| execution-record | pending | not_run | pass | partial | not_run | false |

## Merge Decision

accept_with_conditions

## Contamination Status

```yaml
contamination_free: true
learning_eligible: false
trust_update_allowed: true
````

## Next Action

Run complete YAML parse for all schemas, then run schema semantics validation if tooling exists.

````

---

## Meta-Audit Packet

```md id="vx6439"
# Meta-Audit Assignment

You are auditing a governed multi-model schema cycle.

## Materials

- Supervisor merge packet
- Worker outputs from DeepSeek, Grok, and Z.ai
- Accepted schema files
- Dependency notes

## Audit Questions

1. Did any worker exceed its artifact boundary?
2. Did any critic claim become source truth without inspection?
3. Are claimed tests backed by terminal output?
4. Are schema/interface changes declared?
5. Are dependency notes required but missing?
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
````

---

## Example Meta-Audit Result

```yaml id="sao1ma"
MetaAuditorOutput:
  final_audit_decision: accept_with_conditions
  confidence: 0.78

  executive_summary:
    - "The multi-model workflow stayed mostly within boundaries."
    - "Critic output was correctly treated as critique, not source truth."
    - "YAML syntax validation exists for two schemas, but full schema semantics validation remains pending."

  blocking_defects: []

  non_blocking_defects:
    - defect_id: N-001
      severity: medium
      layer: LAPOP
      artifact_or_record: supervisor_merge_packet
      issue: "Not all accepted schemas have validation logs yet."
      evidence: "execution-record validation is pending."
      recommended_fix: "Run YAML parse across all schemas before release."
    - defect_id: N-002
      severity: medium
      layer: TGL
      artifact_or_record: cycle checkpoint
      issue: "No explicit checkpoint record was shown."
      evidence: "Merge packet references accepted artifacts but no checkpoint hash."
      recommended_fix: "Create checkpoint before final repository commit."

  contamination_assessment:
    contamination_detected: false
    contamination_sources: []
    contamination_free: true
    learning_allowed: false
    trust_update_allowed: true
    memory_write_allowed: false

  boundary_assessment:
    boundary_respected: true
    violations: []
    dependency_notes_required:
      - "Non-blocking dependency note for semantic_change_magnitude if validation schema is added."

  policy_assessment:
    policy_checked: partial
    policy_conflicts: []
    denied_or_missing_policy_checks:
      - "Repository write action should receive policy check before commit."

  validation_assessment:
    validation_complete: false
    missing_validations:
      - "YAML parse for execution-record schema."
      - "Schema semantics validation for all schemas."
    failed_validations: []

  evidence_assessment:
    evidence_grounded: partial
    unsupported_claims: []
    missing_sources:
      - "No terminal log for execution-record YAML parse yet."

  merge_assessment:
    merge_safe: conditional
    merge_blockers: []
    disagreements_to_preserve: []

  strategy_learning_assessment:
    strategy_learning_safe: false
    reasons:
      - "This is a design/drafting cycle, not a validated operational learning cycle."
    required_benchmark_gates: []

  temporal_assessment:
    rollback_available: unknown
    checkpoint_status: insufficient
    stale_or_oscillating_items: []

  economic_assessment:
    further_work_justified: true
    low_value_actions:
      - "Running more novelty critique before validation."
    high_value_next_actions:
      - "Run YAML parse and schema validation."
      - "Create checkpoint before commit."

  required_repairs:
    - priority: medium
      target_artifact_or_layer: schemas/
      repair_prompt: >
        Run YAML parse validation across all schema files and report terminal output.
    - priority: medium
      target_artifact_or_layer: TGL
      repair_prompt: >
        Create checkpoint record or git commit hash before final merge.

  final_recommendation:
    action: repair
    rationale: "Accept design direction, but run validation and checkpoint before final merge."
```

---

## Execution Record Lite

```yaml id="fqhyui"
ExecutionRecordLite:
  cycle_id: cycle-010-multi-model-schema-session
  task: "Coordinate multiple LLM sessions to draft and validate governance schemas."
  models_used:
    - GPT Supervisor
    - DeepSeek Worker
    - Z.ai Tool Worker
    - Grok Critic
    - Claude Critic
    - Gemini Structured Worker
  artifacts_touched:
    - schemas/execution-context.schema.yaml
    - schemas/dependency-note.schema.yaml
    - schemas/execution-record.schema.yaml
    - docs/glossary.md
    - adversarial_schema_audit.md
    - coherence_review.md
  worker_outputs:
    - deepseek_execution_record_schema
    - zai_yaml_parse_report
    - grok_adversarial_schema_audit
    - claude_coherence_review_pending
    - gemini_glossary_pending
  dependency_notes:
    - "Consider semantic_change_magnitude in future validation-report schema."
  validations:
    - "YAML parse for first two schema files: pass."
    - "Full schema validation: pending."
  audits:
    - "Meta-audit: accept_with_conditions."
  merge_decision: accept_with_conditions
  contamination_free: true
  learning_eligible: false
  trust_update_allowed: true
  next_action: "Run validation across all schemas and checkpoint before final merge."
```

---

## Clean Coordination Rules Demonstrated

### 1. Critique is not source truth

Grok’s criticism is useful, but each claim must be checked against actual artifact content.

### 2. Tool output needs logs

Z.ai’s validation is accepted only because terminal output was included.

### 3. Workers stay inside assigned files

DeepSeek drafts one schema. Gemini drafts one doc. Critics do not rewrite files.

### 4. Dependency notes preserve cross-artifact intent

A future validation schema change is not silently forced into current schemas.

### 5. Strategy learning is blocked

This is a design cycle, not an operational repair cycle. SLL should not learn durable strategy yet.

### 6. Checkpoint before commit

TGL requires a checkpoint or git hash before final repository merge.

### 7. Human remains final authority

No model publishes, commits, or pushes without explicit human approval.

---

## Final Supervisor Recommendation

```yaml id="xxq4oq"
decision: accept_with_conditions
reason: >
  The multi-model session produced useful schema and documentation artifacts
  while mostly preserving boundaries and avoiding contamination. Final merge
  should wait for full YAML/schema validation and a checkpoint.
contamination_free: true
learning_eligible: false
trust_update_allowed: true
next_action: >
  Ask the repo agent to run validation across all schema files, then create a
  checkpoint or git commit before continuing to the next artifact batch.
```

---

## Minimal Real-World Usage

For a quick manual workflow:

```text id="0dw1ht"
1. GPT creates worker packet.
2. Human pastes packet into DeepSeek/Z.ai/Grok/etc.
3. Worker returns output contract.
4. Human pastes output back to GPT.
5. GPT validates, audits, and merges or rejects.
6. GPT emits next worker packet.
7. Repeat.
```

This keeps multi-model work fast without letting it become chaos.
