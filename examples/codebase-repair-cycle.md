# Codebase Repair Cycle Example

## Purpose

This example shows how `lapop-agent-governance` can coordinate a governed codebase repair cycle.

It is designed for cases where a local script, service, package, or repository fails due to:

- runtime exception,
- missing configuration,
- schema mismatch,
- bad import,
- broken CLI argument,
- malformed CSV/JSON/YAML output,
- performance hang,
- failing test,
- dependency drift,
- or silent metric regression.

The key rule:

> Repair the smallest validated cause, preserve evidence, and do not let the repair worker mutate unrelated artifacts.

---

## Scenario

A Python script crashes at shutdown while writing a CSV log.

Traceback:

```text
Traceback (most recent call last):
  File "/home/user/project/sim.py", line 1100, in <module>
    main()
  File "/home/user/project/sim.py", line 1081, in main
    vm.write_death_csv(os.path.join(config.outdir, args.death_csv))
  File "/home/user/project/sim.py", line 774, in write_death_csv
    w.writerows(self.death_log)
  File "/usr/lib/python3.13/csv.py", line 229, in writerows
    return self.writer.writerows(map(self._dict_to_list, rowdicts))
  File "/usr/lib/python3.13/csv.py", line 221, in _dict_to_list
    raise ValueError("dict contains fields not in fieldnames: "
ValueError: dict contains fields not in fieldnames: 'collapse_age'
````

Observed failure:

```yaml
failure_type: runtime_exception
artifact: sim.py
function: write_death_csv
root_cause_hypothesis: >
  CSV DictWriter fieldnames do not include all keys present in death_log rows.
```

Repair objective:

```yaml
objective: >
  Fix CSV writing so variable death_log row fields do not crash output.
minimum_viable_fix: >
  Build fieldnames from the union of keys across all rows, with preferred ordering.
```

---

## Cycle ID

```yaml
cycle_id: cycle-001-codebase-csv-repair
project_id: codebase-repair-example
execution_mode: local_runner
side_effect_class: write_internal
risk_level: medium
```

---

## Execution Context

```yaml
project_id: codebase-repair-example
repository_id: local/sim-project
session_id: session-2026-04-27-code-repair
cycle_id: cycle-001-codebase-csv-repair
run_id: run-code-repair-001
branch_id: repair/csv-fieldnames
artifact_id: file.sim-py
artifact_path: sim.py
artifact_type: file
agent_class: supervisor
agent_role: supervisor
execution_mode: local_runner
side_effect_class: write_internal
risk_level: medium
policy_attestation_id: null
strategy_ledger_head: null
temporal_position:
  cycle_index: 1
  branch_depth: 1
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
    - patch_assigned_file
    - run_local_tests
    - write_internal
  forbidden_actions:
    - modify_unowned
    - delete_logs
    - fabricate_test_results
    - publish_external
tags:
  - code-repair
  - csv
  - python
```

---

## Artifact Boundary

```yaml
artifact_boundary:
  artifact_id: file.sim-py
  artifact_path: sim.py
  owner_agent_class: tool_using_worker
  allowed_actions:
    - inspect
    - patch
    - run_test
    - report_traceback
  forbidden_actions:
    - modify_unassigned_files
    - remove_logging
    - suppress_exception_without_fix
    - alter_model_dynamics
    - change_cli_contract
  validation_requirements:
    - reproduce_or_explain_traceback
    - run_script_short_case
    - verify_death_csv_written
    - inspect_csv_header_contains_collapse_age
```

---

## Policy Check

Before patching, the Policy Composition Worker evaluates the action.

```yaml
PolicyWorkerOutput:
  decision: allow_with_constraints
  allowed: true
  reason: "Internal write to assigned file is allowed if patch is scoped and validation is run."
  constraints:
    - constraint_type: require_validation
      description: "Run a short script execution or targeted unit test after patch."
      blocking: true
    - constraint_type: require_checkpoint
      description: "Preserve pre-patch file or git diff for rollback."
      blocking: true
    - constraint_type: limit_artifact_scope
      description: "Patch only sim.py unless a dependency note is emitted."
      blocking: true
  human_approval_required: false
  dependency_note_required: false
```

---

## Worker Assignment Packet

````md
# Worker Assignment Packet

## Worker Model

Local code agent or Z.ai / GLM agent

## Role

tool_using_worker

## Assigned Artifact

`sim.py`

## Objective

Fix the CSV DictWriter crash caused by row dictionaries containing keys not listed in fieldnames.

## Evidence

Traceback shows:

```text
ValueError: dict contains fields not in fieldnames: 'collapse_age'
````

Crash occurs inside:

```text
write_death_csv(...)
w.writerows(self.death_log)
```

## Allowed Actions

* Inspect `sim.py`
* Patch CSV writer helper
* Add reusable `_write_dicts_csv` helper if absent
* Run targeted test or short simulation
* Report terminal output

## Forbidden Actions

* Do not change simulation dynamics.
* Do not remove death logging.
* Do not hide the exception by skipping rows.
* Do not modify unrelated files.
* Do not claim tests passed unless run.
* Do not overwrite logs without preserving result summary.

## Required Fix

Implement a CSV writer that:

1. Handles empty row lists safely.
2. Computes union of all row keys.
3. Preserves preferred column order when provided.
4. Appends remaining fields in deterministic sorted order.
5. Uses `extrasaction='ignore'` only after union has included all known keys.
6. Writes header and rows.

Suggested helper:

```python
@staticmethod
def _write_dicts_csv(filename, rows, preferred=None):
    if not rows:
        return
    all_keys = set()
    for row in rows:
        all_keys.update(row.keys())
    preferred = preferred or []
    fieldnames = [k for k in preferred if k in all_keys]
    fieldnames += sorted(k for k in all_keys if k not in fieldnames)
    with open(filename, "w", newline="") as f:
        writer = csv.DictWriter(f, fieldnames=fieldnames, extrasaction="ignore")
        writer.writeheader()
        for row in rows:
            writer.writerow(row)
```

Then update:

```python
def write_death_csv(self, filename):
    self._write_dicts_csv(
        filename,
        self.death_log,
        preferred=[
            "generation",
            "id",
            "archetype",
            "cause",
            "age",
            "collapse_age",
            "survival_prob",
        ],
    )
```

## Validation Requirements

Run at least one:

```bash
python3 sim.py --generations 5 --init-pop 20
```

or a targeted mini-test:

```python
rows = [
    {"generation": 1, "id": 1, "cause": "selection"},
    {"generation": 2, "id": 2, "cause": "collapse_timeout", "collapse_age": 12},
]
QNVM._write_dicts_csv("/tmp/deaths.csv", rows, preferred=["generation", "id", "cause", "collapse_age"])
```

Verify:

* no exception,
* output CSV exists,
* header includes `collapse_age`,
* existing preferred columns stay first.

## Output Contract

Return:

1. Patch or full modified function.
2. Commands/tests run.
3. Terminal output excerpt.
4. Validation summary.
5. Dependency notes, or `None`.

````

---

## Expected Worker Output

```md
# Worker Output

## Assigned Artifact

`sim.py`

## Work Completed

Added a reusable `_write_dicts_csv` helper and updated `write_death_csv`, `write_birth_csv`, `write_phase_csv`, and related CSV writers to use dynamic union fieldnames with preferred ordering.

## Artifact / Patch

```diff
+    @staticmethod
+    def _write_dicts_csv(filename, rows, preferred=None):
+        if not rows:
+            return
+        all_keys = set()
+        for row in rows:
+            all_keys.update(row.keys())
+        preferred = preferred or []
+        fieldnames = [k for k in preferred if k in all_keys]
+        fieldnames += sorted(k for k in all_keys if k not in fieldnames)
+        with open(filename, "w", newline="") as f:
+            w = csv.DictWriter(f, fieldnames=fieldnames, extrasaction="ignore")
+            w.writeheader()
+            for row in rows:
+                w.writerow(row)
+
     def write_death_csv(self, filename):
-        with open(filename, "w", newline="") as f:
-            w = csv.DictWriter(f, fieldnames=["generation","id","archetype","cause","age"])
-            w.writeheader()
-            w.writerows(self.death_log)
+        self._write_dicts_csv(
+            filename,
+            self.death_log,
+            preferred=[
+                "generation",
+                "id",
+                "archetype",
+                "cause",
+                "age",
+                "collapse_age",
+                "survival_prob",
+            ],
+        )
````

## Validation Summary

```yaml
syntax_valid: true
schema_valid: not_applicable
tests_run:
  - name: targeted_csv_union_test
    result: pass
    evidence: "CSV wrote successfully; header includes collapse_age."
  - name: short_simulation
    result: pass
    evidence: "python3 sim.py --generations 5 --init-pop 20 completed without DictWriter ValueError."
boundary_respected: true
evidence_grounded: true
contamination_detected: false
```

## Dependency Notes

None.

## Known Risks / Unresolved Issues

* This fix prevents CSV field mismatch crashes but does not validate whether all optional fields should be present semantically.
* Column order for non-preferred keys is deterministic alphabetical order.

````

---

## Supervisor Validation

Supervisor checks:

```yaml
validation:
  traceback_root_cause_addressed: true
  patch_scope_respected: true
  unrelated_simulation_dynamics_changed: false
  preferred_column_order_present: true
  union_fieldnames_present: true
  test_evidence_present: true
  contamination_detected: false
````

---

## Meta-Audit

```yaml
MetaAuditorOutput:
  final_audit_decision: accept_with_conditions
  confidence: 0.84

  executive_summary:
    - "Patch addresses the traced CSV DictWriter failure directly."
    - "Artifact boundary was respected."
    - "Validation evidence is sufficient for the reported crash."

  blocking_defects: []

  non_blocking_defects:
    - defect_id: N-001
      severity: medium
      layer: LAPOP
      artifact_or_record: sim.py
      issue: "No formal unit test file was added."
      evidence: "Validation used targeted inline test and short simulation."
      recommended_fix: "Add regression test later if test framework exists."

  contamination_assessment:
    contamination_detected: false
    contamination_sources: []
    contamination_free: true
    learning_allowed: true
    trust_update_allowed: true
    memory_write_allowed: false

  boundary_assessment:
    boundary_respected: true
    violations: []
    dependency_notes_required: []

  policy_assessment:
    policy_checked: true
    policy_conflicts: []
    denied_or_missing_policy_checks: []

  validation_assessment:
    validation_complete: true
    missing_validations: []
    failed_validations: []

  evidence_assessment:
    evidence_grounded: true
    unsupported_claims: []
    missing_sources: []

  merge_assessment:
    merge_safe: true
    merge_blockers: []
    disagreements_to_preserve: []

  strategy_learning_assessment:
    strategy_learning_safe: true
    reasons:
      - "Clean validated operational repair outcome."
    required_benchmark_gates:
      - csv_writer_field_union_regression_case
      - unrelated_csv_layout_regression_case

  temporal_assessment:
    rollback_available: true
    checkpoint_status: sufficient
    stale_or_oscillating_items: []

  economic_assessment:
    further_work_justified: false
    low_value_actions:
      - "Additional broad audit is low value for this narrow crash."
    high_value_next_actions:
      - "Optional regression test in future cycle."

  required_repairs: []

  final_recommendation:
    action: accept
    rationale: "Accept patch. Optional future work: convert targeted test into formal regression test."
```

---

## Strategy Learning Candidate

Because the run is clean and validated, SLL may propose a low-confidence candidate patch.

```yaml
StrategyLearningWorkerOutput:
  learning_eligible: true
  quarantine_required: false
  quarantine_record:
    reason: null
    details: null
    reusable_for_negative_training: false

  outcome_summary:
    task_type: code_repair
    artifact_type: file
    agent_class: tool_using_worker
    validation_passed: true
    audit_passed: true
    repair_successful: true
    merge_method: supervisor_merge
    human_review_used: false
    contamination_free: true
    outcome_quality_score: 0.88

  extracted_strategy_features:
    - feature_name: failure_type
      value: csv_dictwriter_fieldname_mismatch
      confidence: 0.95
    - feature_name: repair_strategy
      value: compute_fieldnames_from_union_of_all_row_keys
      confidence: 0.85
    - feature_name: repair_scope
      value: localized_helper_function
      confidence: 0.80

  candidate_strategy_patch:
    patch_recommended: true
    patch_type: repair_strategy_update
    scope:
      task_type: code_repair
      artifact_type: file
      failure_type: csv_dictwriter_fieldname_mismatch
    proposed_changes:
      - target_table: repair_strategy_rules
        target_key: csv_dictwriter_fieldname_mismatch
        old_value: null
        new_value: compute_fieldnames_from_union_of_all_row_keys_with_preferred_order
        change_reason: "Clean execution showed this repair resolves DictWriter wrong_fields error."
        confidence: 0.35
    evidence_outcome_ids:
      - outcome-code-repair-csv-001
    confidence: 0.35
    rollback_plan: "Remove or restore prior repair strategy rule."
    benchmark_requirements:
      - csv_writer_field_union_regression_case
      - unrelated_csv_output_format_case
      - contaminated_traceback_claim_case

  policy_check_required: true
  benchmark_gate_required: true

  final_recommendation:
    action: create_candidate_patch
    rationale: "Single clean run supports a scoped low-confidence candidate patch only."
```

---

## Economic Decision

EIL evaluates whether to continue after patch passes.

```yaml
EconomicIntelligenceWorkerOutput:
  decision: stop
  recommended_action:
    action_id: accept-current-patch
    action_type: accept_current
    allowed_by_policy: true
    target: sim.py
  ranked_actions:
    - action_id: accept-current-patch
      action_type: accept_current
      allowed_by_policy: true
      value_score: 0.85
      cost_score: 0.00
      risk_penalty: 0.08
      trust_adjustment: 0.05
      human_load_penalty: 0.00
      latency_penalty: 0.00
      final_score: 0.82
      rationale: "Patch directly addresses traceback and validation passed."
    - action_id: run-broad-refactor
      action_type: repair_again
      allowed_by_policy: true
      value_score: 0.10
      cost_score: 0.55
      risk_penalty: 0.25
      trust_adjustment: 0.00
      human_load_penalty: 0.00
      latency_penalty: 0.20
      final_score: -0.90
      rationale: "Broad refactor is low-value and increases regression risk."
  stop_conditions_met:
    - "Specific crash fixed."
    - "Validation passed."
    - "Best additional action has non-positive expected net value."
  continue_conditions_met: []
  budget_status:
    token_budget: under_budget
    tool_budget: under_budget
    latency_budget: under_budget
    human_review_budget: under_budget
  risk_summary:
    contamination_risk: 0.02
    policy_risk: 0.02
    boundary_risk: 0.05
    merge_risk: 0.10
    rollback_risk: 0.05
    downstream_rework_risk: 0.15
  regret_risks:
    - regret_type: stopped_too_early
      likelihood: 0.12
      mitigation: "Add formal regression test in later cycle if CSV output changes again."
  final_rationale: "Stop and accept patch. Further repair is not justified."
  confidence: 0.82
```

---

## Temporal Governance

TGL records checkpoint/rollback state.

```yaml
TemporalGovernanceWorkerOutput:
  temporal_status:
    active_objects: 1
    stale_objects: 0
    archived_objects: 0
    frozen_objects: 0
    rollback_candidates: 1
    oscillations_detected: 0
    deadlocks_detected: 0
    governance_debt_score: 0.05
    status: healthy

  stale_items: []

  rollback_analysis:
    rollback_recommended: false
    candidate_id: checkpoint-before-csv-repair
    rollback_scope: artifact
    contamination_free: true
    policy_attestation_available: true
    strategy_ledger_head_available: false
    post_rollback_validation_required: true
    rollback_risk_score: 0.10
    rationale: "Rollback available but not needed because validation passed."

  oscillation_analysis:
    oscillation_present: false
    records: []

  deadlock_analysis:
    deadlock_present: false
    records: []

  branch_actions:
    - branch_id: repair/csv-fieldnames
      current_state: active
      recommended_action: merge_review
      rationale: "Repair branch is fresh and validated."

  governance_debt:
    stale_note_count: 0
    unresolved_blocking_note_count: 0
    unresolved_disagreement_count: 0
    stale_branch_count: 0
    expired_policy_exception_count: 0
    old_rollback_candidate_count: 0
    oscillation_count: 0
    deadlock_count: 0
    debt_score: 0.05
    highest_priority_items: []

  policy_checks_required:
    - action_type: create_checkpoint
      target: sim.py
      reason: "Preserve pre-patch rollback state."

  final_recommendation:
    action: checkpoint
    rationale: "Checkpoint pre-patch state and accept validated repair."
  confidence: 0.80
```

---

## Collaboration Health

```yaml
CollaborationHealthWorkerOutput:
  overall_health_score: 0.92
  health_status: healthy

  contamination_exclusion:
    contaminated_runs_excluded: 0
    trust_updates_blocked: 0
    strategy_updates_should_be_blocked: 0

  boundary_health:
    score: 0.95
    violations: 0
    unresolved_violations: 0
    main_issues: []

  dependency_health:
    score: 0.95
    total_notes: 0
    blocking_notes: 0
    stale_or_unresolved_notes: 0
    note_density_status: low
    main_issues: []

  trust_summary:
    trust_update_allowed: true
    high_trust_signals:
      - "Worker used traceback evidence."
      - "Patch was scoped to assigned file."
      - "Validation was run."
    low_trust_signals: []
    trust_risks: []

  disagreement_summary:
    preserve_disagreement_recommended: false
    disagreement_records_to_create: []

  merge_health:
    score: 0.90
    merge_reversal_risk: low
    merge_regret_signals: []

  human_review_burden:
    status: low
    issues: []

  emergence_risk:
    score: 0.05
    detected_patterns: []

  intervention_recommendations: []

  final_recommendation:
    action: continue
    rationale: "Collaboration health is clean and patch can proceed through normal merge review."
  confidence: 0.82
```

---

## Execution Record Lite

```yaml
ExecutionRecordLite:
  cycle_id: cycle-001-codebase-csv-repair
  task: "Fix CSV DictWriter crash caused by row keys missing from fieldnames."
  models_used:
    - GPT Supervisor
    - local code worker
    - Meta-Auditor
  artifacts_touched:
    - sim.py
  worker_outputs:
    - csv-fieldnames-patch
    - validation-summary
  dependency_notes: []
  validations:
    - targeted_csv_union_test: pass
    - short_simulation: pass
  audits:
    - meta_audit: accept_with_conditions
  merge_decision: accept
  contamination_free: true
  learning_eligible: true
  next_action: "Optional: convert targeted CSV test into formal regression test."
```

---

## Final Supervisor Decision

```yaml
decision: accept
reason: >
  The repair directly addresses the observed traceback by generating CSV
  fieldnames from the union of all row keys with deterministic preferred ordering.
  The patch is scoped, validated, and rollback-safe.
contamination_free: true
learning_eligible: true
strategy_patch_allowed: candidate_only_pending_benchmark_and_policy
next_action: >
  Merge patch after checkpoint. Consider adding formal regression test in next cycle.
```

---

## Key Lessons

1. Traceback evidence is stronger than speculative critique.
2. The repair should target the smallest failing function.
3. CSV row schemas can drift when event logs have optional fields.
4. Dynamic field union is safer than static fieldnames for heterogeneous logs.
5. Validation must prove the specific crash is fixed.
6. Strategy learning may occur only as a low-confidence candidate patch.
7. Further broad refactor is economically unjustified after the targeted fix passes.
8. The clean cycle can update trust and strategy candidates, but not factual memory.
