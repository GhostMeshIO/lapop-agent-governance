# Implementation Roadmap

This roadmap describes a staged path from public specification to a minimal reference implementation.

The goal is not to build the full system at once.

The goal is to create a small, working, auditable core and then add governance layers incrementally.

## Version Plan

```text
v0.1 = public specification release
v0.2 = schema normalization
v0.3 = minimal reference runner
v0.4 = policy and temporal MVP
v0.5 = collaboration health MVP
v0.6 = strategy learning MVP
v0.7 = economic intelligence MVP
v1.0 = integrated reference platform
````

## v0.1 — Public Specification Release

### Goals

Publish the project as a coherent open-source specification stack.

### Deliverables

* `README.md`
* `LICENSE`
* `CHANGELOG.md`
* core layer specs
* initial docs
* prompt packs
* consolidated project notes
* public roadmap

### Required Files

```text
README.md
LICENSE
CHANGELOG.md
docs/overview.md
docs/architecture.md
docs/common-vs-novel.md
docs/implementation-roadmap.md
docs/glossary.md
```

### Success Criteria

* Repository explains what the project is.
* Layer names and purposes are clear.
* Roadmap is public.
* Non-goals are explicit.
* License is present.
* Project is ready for public review.

## v0.2 — Schema Normalization

### Goals

Create canonical shared schemas and remove duplicated definitions across layer specs.

### Deliverables

```text
schemas/execution-context.schema.yaml
schemas/execution-record.schema.yaml
schemas/dependency-note.schema.yaml
schemas/artifact-boundary.schema.yaml
schemas/policy-attestation.schema.yaml
schemas/strategy-update-patch.schema.yaml
schemas/collaboration-health-report.schema.yaml
schemas/rollback-candidate.schema.yaml
schemas/human-review-outcome.schema.yaml
```

### Canonical Shared Schemas

#### ExecutionContext

Required across all layers.

#### Execution_Record

Required for LAPOP, SLL, CHL, TGL.

#### Dependency_Note

Required for CDO, CHL, TGL.

#### Artifact_Boundary

Required for CDO and LAPOP.

#### PolicyAttestationRecord

Required for PCL, LAPOP, TGL.

#### StrategyUpdatePatch

Required for SLL and TGL rollback.

#### HumanReviewOutcome

Required for SLL, EIL, LAPOP.

### Success Criteria

* Each shared schema has one canonical definition.
* Layer specs reference canonical schemas instead of redefining them.
* Required fields are stable.
* Optional fields are marked clearly.
* Schema versioning convention exists.

## v0.3 — Minimal Reference Runner

### Goals

Build the smallest useful local runner.

The runner should not attempt full intelligence or full governance. It should prove the basic execution cycle.

### Minimal Components

```text
ExecutionContextBuilder
TaskNode
AgentAssignment
ArtifactBoundaryValidator
DependencyNoteValidator
WorkerOutputCollector
ValidationReport
ExecutionRecordWriter
SupervisorMergeDecision
```

### Minimal Workflow

```text
1. Create task.
2. Create ExecutionContext.
3. Assign one or more artifacts.
4. Run worker prompts manually or through simple adapters.
5. Collect worker output.
6. Validate artifact boundary.
7. Validate dependency notes.
8. Emit ValidationReport.
9. Supervisor accepts/rejects output.
10. Write Execution_Record.
```

### Out of Scope

* full policy compiler,
* strategy learning,
* trust graph,
* rollback engine,
* complex routing,
* live model API integration,
* economic optimization.

### Success Criteria

* A task can be run through a complete cycle.
* Worker output is accepted or rejected.
* Boundary violations are detected.
* Dependency notes are parsed.
* Execution record is written.
* Contamination flag exists.

## v0.4 — Policy and Temporal MVP

### Goals

Add minimal policy checks and checkpoint/rollback support.

### Components

```text
PolicyQueryStub
EffectivePolicySet
PolicyAttestationRecord
ExceptionWindow
Checkpoint
RollbackCandidate
AgingPolicy
```

### PCL MVP

Implement:

* basic policy fragments,
* scope matching,
* deterministic precedence,
* conflict proof,
* `is_action_allowed`.

### TGL MVP

Implement:

* checkpoint creation,
* rollback candidate creation,
* artifact lineage,
* basic stale item detection,
* basic deadlock detection for dependency notes.

### Success Criteria

* Actions can be allowed or denied by policy.
* Policy decisions are recorded.
* Checkpoints are created.
* A previous artifact version can be restored.
* Stale notes can be detected.

## v0.5 — Collaboration Health MVP

### Goals

Measure collaboration health without mutating authority rules.

### Components

```text
BoundaryHealthAnalyzer
NoteDensityTracker
CrossBoundaryChurnTracker
SimpleTrustGraph
DisagreementRecord
EmergenceRiskSignal
CollaborationHealthReport
```

### Metrics

* violation rate,
* note density,
* blocking rate,
* resolution efficiency,
* cross-boundary churn,
* stale disagreement count,
* trust score.

### Success Criteria

* Boundary health is computed from execution records.
* Trust graph updates only from clean runs.
* Disagreements can be preserved.
* Health report is emitted.
* Contaminated runs are ignored.

## v0.6 — Strategy Learning MVP

### Goals

Allow the system to learn better operational strategy from clean runs.

### Components

```text
OutcomeCollector
LearningQuarantineGate
StrategyFeatureExtractor
StrategyUpdatePatch
BenchmarkRegressionGate
StrategyMemoryLedger
StrategyPatchApplier
```

### Learnable Tables

* routing priors,
* repair strategy priority,
* validation threshold hints,
* escalation triggers.

### Safety Requirements

* Do not learn from contaminated runs.
* Do not mutate artifacts.
* Do not update model weights.
* Do not apply strategy patches without benchmark gate.
* Every patch must be reversible.

### Success Criteria

* Clean runs produce strategy examples.
* Contaminated runs are quarantined.
* Candidate strategy patch is generated.
* Patch can pass or fail benchmark gate.
* Strategy ledger records patch.
* Patch can be rolled back.

## v0.7 — Economic Intelligence MVP

### Goals

Add value-aware stop/probe decisions.

### Components

```text
VOIDecisionRecord
ValueOfInformationController
ToolTrustCalibrationMatrix
HumanReviewLoadModel
EconomicRegretTracker
ActionRecommendationRanker
```

### Action Types

* stop,
* retrieve more,
* run audit,
* run test,
* call tool,
* ask human,
* try alternate agent,
* preserve disagreement.

### Decision Factors

* expected information gain,
* expected quality delta,
* cost,
* latency,
* contamination risk,
* tool trust,
* human review load,
* downstream risk.

### Success Criteria

* Optional actions receive VOI scores.
* Low-value actions can be rejected.
* Human review is treated as costly.
* Tool reliability affects decisioning.
* Economic regret can be logged.

## v1.0 — Integrated Reference Platform

### Goals

Deliver a coherent reference implementation of the full stack.

### Required Capabilities

* end-to-end execution cycle,
* artifact-bound workers,
* dependency note handling,
* validation and audit,
* supervisor merge,
* policy checks,
* checkpointing and rollback,
* collaboration health reports,
* strategy learning from clean runs,
* economic decisioning,
* temporal governance,
* benchmark examples,
* public documentation.

### Success Criteria

* A realistic multi-agent coding task can run end to end.
* A research synthesis task can run end to end.
* A contaminated run is quarantined.
* A policy conflict blocks execution.
* A rollback restores artifact and governance state.
* A strategy patch passes benchmark gate before application.
* Collaboration health report detects boundary problems.
* EIL stops a low-value expansion.
* Documentation matches behavior.

## Implementation Priorities

### Priority 1 — Shared Schemas

Without schemas, the stack becomes prose.

### Priority 2 — Execution Record

Without execution records, nothing can be learned, audited, replayed, or rolled back.

### Priority 3 — Artifact Boundaries

Without boundaries, multi-agent work becomes uncontrolled editing.

### Priority 4 — Dependency Notes

Without notes, cross-boundary coordination becomes hidden and untraceable.

### Priority 5 — Validation Reports

Without validation, merge decisions are guesses.

### Priority 6 — Contamination Flag

Without contamination tracking, bad runs poison learning and trust.

### Priority 7 — Policy Query

Without policy checks, governance is descriptive instead of operational.

### Priority 8 — Checkpoints

Without checkpoints, rollback is impossible.

## Suggested Technology Stack

The reference implementation can start simple.

### Language

```text
Python 3.11+
```

### Data Formats

```text
YAML for schemas and policies
JSONL for execution records
Markdown for human-readable reports
SQLite for local ledger prototype
```

### Optional Libraries

```text
pydantic
jsonschema
typer
rich
networkx
sqlite-utils
pytest
```

### Repository Layout for Implementation

```text
src/lapop_governance/
├── context.py
├── records.py
├── boundaries.py
├── dependency_notes.py
├── validation.py
├── supervisor.py
├── policy.py
├── temporal.py
├── collaboration.py
├── strategy.py
├── economics.py
└── cli.py
```

## Testing Plan

### Unit Tests

* schema validation,
* dependency note parsing,
* artifact boundary validation,
* policy precedence,
* contamination quarantine,
* trust update exclusion,
* rollback candidate creation.

### Integration Tests

* single-worker artifact update,
* multi-worker dependency note cycle,
* boundary violation rejection,
* contaminated run exclusion,
* policy conflict blocking,
* rollback restore,
* strategy patch rejection.

### Benchmark Scenarios

1. Clean code repair.
2. Worker violates boundary.
3. Critic hallucinates evidence.
4. Two workers disagree.
5. Tool output conflicts with generated output.
6. Policy conflict exists.
7. Strategy patch causes regression.
8. Long-running branch becomes stale.
9. Artifact oscillates between versions.
10. Human review reverses merge decision.

## Release Discipline

Every release should include:

* changelog entry,
* schema version changes,
* migration notes if needed,
* known limitations,
* benchmark status,
* open issues.

## MVP Rule

When uncertain, build less.

The correct first implementation is not a giant autonomous agent platform.

The correct first implementation is:

> a tiny, boring, auditable runner that proves boundaries, notes, validation, execution records, contamination flags, and rollback can work.
