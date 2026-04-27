# Glossary

This glossary defines core terms used across `lapop-agent-governance`.

## Agent

An LLM session, tool-using process, script, human-assisted model, or worker role assigned to perform part of a task.

Agents may be specialized by function:

- planner,
- worker,
- critic,
- auditor,
- merger,
- supervisor,
- policy worker,
- strategy worker,
- economic worker,
- collaboration health worker,
- temporal governance worker.

## Agent Assignment

A specific mapping between an agent and a task or artifact.

An assignment should include:

- agent class,
- assignment ID,
- artifact boundary,
- permission profile,
- task node,
- expected output contract.

## Agent Class

A category of agent, such as:

- Code Generation,
- Analysis Only,
- Design Output,
- Data Operation,
- Supervisor Governance,
- Policy Worker,
- Strategy Worker.

## Artifact

Any structured output or work product.

Examples:

- source file,
- schema,
- dataset,
- interface,
- patch,
- report,
- diagram,
- benchmark,
- validation result,
- dependency note set.

## Artifact Boundary

The permission boundary around an artifact.

It defines:

- artifact ID,
- artifact type,
- owner agent,
- allowed actions,
- forbidden actions.

Artifact boundaries prevent uncontrolled cross-file or cross-module edits.

## Artifact Version Lineage

The parent/child history of artifact versions.

Lineage tracks whether a version came from:

- direct edit,
- merge,
- repair,
- rollback,
- supervisor override,
- speculative branch.

## Audit Report

A structured review that checks for deeper issues beyond basic validation.

An audit may inspect:

- hallucination,
- unsupported claims,
- architectural drift,
- contamination,
- hidden contradictions,
- policy violations,
- semantic compression loss.

## Boundary Violation

An event where an agent performs a forbidden action on an artifact.

Examples:

- editing an unowned file,
- changing an interface without permission,
- deleting another agent’s artifact,
- renaming a schema,
- publishing without validation.

## Checkpoint

A saved state that supports replay, resume, or rollback.

A checkpoint may include:

- artifact hashes,
- input hashes,
- validation hashes,
- tool output hashes,
- policy attestation ID,
- strategy ledger head.

## CHL-003

Collaboration Health Layer.

Measures structural health of multi-agent collaboration.

Tracks:

- boundary health,
- note density,
- trust,
- disagreement,
- emergence risk,
- intervention signals.

## CDO-003

Coordination Discipline Overlay.

Enforces artifact boundaries, dependency notes, ownership transfer, semantic integrity, and lane discipline.

## Clean Run

An execution run that passed contamination checks.

A clean run may be used for:

- strategy learning,
- collaboration health metrics,
- trust updates,
- validated memory.

## Collaboration Health Report

A report produced by CHL.

It may include:

- boundary health score,
- trust graph summary,
- disagreement preservation advice,
- emergence risk signal,
- intervention signals.

## Conflict Proof

A record showing a detected policy conflict and how it was resolved.

Conflict proof usually includes:

- conflicting policy fragments,
- conflict type,
- resolution method,
- timestamp,
- resolved effective rule.

## Contamination

A condition where unreliable information enters or threatens to enter the system’s authoritative state.

Examples:

- hallucinated source,
- fabricated dependency,
- speculative output cited as fact,
- critic hallucination treated as truth,
- failed repair becoming canonical,
- unverified artifact entering memory.

## Contamination-Free Flag

A boolean on the execution record.

```yaml
contamination_free: true
````

If false, the run must not update trust, strategy learning, collaboration metrics, or validated memory.

## Control Plane

The governance layer of the system.

Includes:

* policy composition,
* routing,
* economics,
* strategy learning,
* collaboration health,
* temporal governance,
* rollback,
* contamination control.

## Data Plane

The work-producing layer.

Includes:

* agent execution,
* tool calls,
* artifact generation,
* validation,
* audit,
* merge,
* repair.

## Dependency Note

A structured request from one agent to another when a change is needed outside the requesting agent’s boundary.

A dependency note includes:

* target artifact,
* required change,
* justification,
* priority,
* blocking flag,
* lifecycle state.

## Dependency Note Lifecycle

The states of a dependency note.

```text
open
acknowledged
deferred
resolved
superseded
rejected
```

## Disagreement Preservation

The act of keeping unresolved but meaningful disagreement as a first-class artifact instead of forcing a merge.

Useful when:

* evidence conflicts,
* policy conflict exists,
* agents disagree for valid reasons,
* uncertainty is high,
* merge would hide important caveats.

## EIL-003

Economic Intelligence Layer.

Decides whether additional work is worth its cost.

Uses:

* value of information,
* cost,
* latency,
* contamination risk,
* tool trust,
* human review load.

## Effective Policy Set

The final compiled policy set for a context after PCL resolves applicable policies, precedence, exceptions, and conflicts.

## Emergence Risk

A harmful coordination pattern that appears across agents.

Examples:

* circular dependency,
* feedback amplification,
* optimization conflict,
* repeated unresolved note loop,
* artifact oscillation.

## ExecutionContext

The shared object identifying the current execution context.

Contains:

* project ID,
* session ID,
* cycle ID,
* task node ID,
* artifact ID,
* agent class,
* checkpoint hash,
* policy attestation ID,
* strategy ledger head.

## Execution Record

The sealed trace of an execution.

Contains:

* context,
* tasks,
* assignments,
* outputs,
* validation reports,
* audit reports,
* dependency notes,
* checkpoints,
* contamination flag,
* failures.

## Governance Debt

Accumulated unresolved governance burden.

Examples:

* stale branches,
* old unresolved notes,
* overdue constitutional review,
* repeated deadlocks,
* unresolved oscillations,
* policy lag.

## Human Review Outcome

A structured record of human review.

May include:

* judgment,
* correction delta,
* latency,
* cost,
* reviewer ID.

## LAPOP-003

LLM-Agent Prompt Orchestration Protocol.

The runtime backbone for multi-agent execution.

Handles:

* task typing,
* decomposition,
* routing,
* execution,
* validation,
* audit,
* merge,
* repair,
* provenance,
* memory,
* reproducibility.

## Learning Quarantine Gate

SLL component that blocks contaminated runs from entering the learning pipeline.

## Merge Log

Record of merge decisions.

May include:

* artifacts merged,
* conflict resolution method,
* preserved disagreements,
* rejected artifacts,
* supervisor overrides.

## Non-Goal

A responsibility explicitly excluded from a layer.

Non-goals prevent architectural drift.

## PCL-003

Policy Composition Layer.

Compiles multiple policy sources into one deterministic effective policy set.

## Policy Attestation Record

Immutable record of the effective policy state for a context.

Supports reproducibility and rollback.

## Policy Fragment

A piece of policy from a source such as:

* organization,
* project,
* domain pack,
* coordination overlay,
* temporary override.

## Policy Precedence

The deterministic ordering used to decide which policy wins when multiple policies apply.

## Policy Query Interface

PCL interface for checking whether an action is allowed.

Example:

```text
is_action_allowed(context, action) -> allowed, reason, constraint
```

## Reproducibility Profile

Metadata required to replay or explain execution.

May include:

* model ID,
* model version,
* prompt template version,
* tool versions,
* retrieval snapshot,
* temperature,
* seed,
* deterministic mode,
* checkpoint hashes.

## Rollback Candidate

A checkpoint that can be restored.

May include:

* candidate ID,
* execution ID,
* cycle number,
* checkpoint hash,
* artifact snapshot,
* note snapshot,
* temporal consistency score,
* policy attestation ID,
* strategy ledger head.

## SLL-003

Strategy Learning Layer.

Learns how to improve platform behavior from clean execution outcomes.

Does not modify artifacts or model weights.

## Speculative Branch

An isolated alternate execution branch.

It may be preserved, revived, discarded, or archived.

Speculative branches must not contaminate mainline unless validated and promoted.

## Strategy Memory Ledger

Append-only ledger of strategy updates.

Stores:

* routing prior updates,
* repair pattern updates,
* validation predictor changes,
* escalation rule changes,
* benchmark results,
* rollback state.

## Strategy Update Patch

Explicit patch to strategy tables.

Must be benchmark-gated and reversible.

## Supervisor

The governance role responsible for assignment, validation decisions, merge/reject decisions, escalation, and final release approval.

A supervisor should not silently mutate worker artifacts without provenance.

## Temporal Consistency

A score measuring whether an artifact version aligns with its history.

May consider:

* lineage distance,
* semantic change magnitude,
* interface changes,
* timestamp delta,
* evidence freshness.

## TGL-003

Temporal Governance Layer.

Manages multi-cycle time behavior:

* aging,
* archival,
* deadlock,
* oscillation,
* rollback,
* speculative branches,
* constitutional review.

## Tool Trust Calibration

A matrix or model estimating reliability of tools by domain and task.

Used by EIL and other layers to decide whether tool use is worth it.

## Trust Graph

A directed, decaying, explainable graph of trust relationships between agents or agent classes.

Trust should be:

* reversible,
* evidence-based,
* contamination-safe,
* pruned or summarized over time.

## Validation Report

Structured output from validation checks.

May include:

* syntax validity,
* schema validity,
* boundary compliance,
* evidence coverage,
* test results,
* completeness,
* contamination status.

## Value of Information

Expected value of taking another action to reduce uncertainty or improve output quality.

Used to decide whether to:

* retrieve,
* audit,
* test,
* ask human,
* run alternate agent,
* or stop.

## Worker Output Contract

The required structure for agent output.

Example:

```text
=== ARTIFACT UPDATED: <artifact_id> ===

<complete artifact content>

=== VALIDATION SUMMARY ===

<validation result>

=== DEPENDENCY NOTES ===

<notes or None>
