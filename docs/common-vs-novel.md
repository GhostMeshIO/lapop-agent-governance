# Common vs Novel Patterns

This document separates common multi-agent patterns from the more novel governance ideas in `lapop-agent-governance`.

The goal is to be honest about what already exists in many systems and clear about what this project adds.

## Common Multi-Agent Patterns

Most multi-agent LLM systems already include some version of the following.

| Category | Common Patterns |
|---|---|
| Agent roles | Planner, worker, critic, merger, supervisor |
| Execution phases | Intent parsing, decomposition, routing, execution, validation, merge |
| Validation | Schema checks, syntax checks, rubric scoring, test execution |
| Memory | Session memory, task memory, project memory |
| Retry logic | Max retries, backoff, fallback agent |
| Cost governance | Token budget, model tier selection, early stopping |
| Routing | Capability matching, domain affinity, load balancing |
| Failure types | Timeout, schema mismatch, tool error, ambiguity, dependency failure |
| Artifact types | Code, SQL, diagrams, JSON plans, reports, patches |
| Human review | Escalation, approval, correction |
| Provenance | Source tracking, parent artifact links, tool logs |

These are useful, but they are not enough for robust multi-agent execution.

## Common Agent Roles

### Planner

Breaks the task into smaller work units.

### Worker

Produces assigned content or code.

### Critic

Reviews output for quality, correctness, completeness, or safety.

### Merger

Combines valid worker outputs.

### Supervisor

Controls assignment, validation, merge, escalation, and final decisions.

### Human Reviewer

Provides high-authority judgment when the system cannot safely resolve ambiguity.

## Common Artifact Types

- source code,
- schema,
- patch,
- report,
- diagram,
- SQL query,
- JSON plan,
- YAML config,
- test file,
- benchmark report,
- state machine,
- validation table,
- evidence table.

## Common Failure Types

- timeout,
- syntax error,
- schema mismatch,
- unsupported claim,
- tool failure,
- dependency failure,
- policy violation,
- budget exceeded,
- merge conflict,
- ambiguity unresolved.

## What This Project Adds

`lapop-agent-governance` focuses on the harder parts of agent collaboration.

## Novel or Less-Common Patterns

### 1. Artifact Boundaries with Forbidden Actions

Many systems assign tasks. Fewer systems explicitly define what an agent is forbidden to do.

Artifact boundaries include:

- allowed actions,
- forbidden actions,
- owner agent,
- artifact type,
- interface change rules,
- merge constraints.

This prevents silent cross-file drift.

### 2. Structured Dependency Notes

When an agent needs another artifact changed, it emits a structured note instead of editing outside its lane.

Dependency notes include:

- target artifact,
- target agent class,
- required change,
- justification,
- blocking flag,
- priority,
- dependency type,
- lifecycle state.

This allows coordination without boundary collapse.

### 3. Dependency Note Lifecycle

Notes are not loose comments.

They move through lifecycle states:

```text
open → acknowledged → deferred → resolved
                         ↓
                    superseded / rejected
````

This makes unresolved coordination visible.

### 4. Blocking Semantics

A note may be advisory or blocking.

Blocking notes can halt merge until resolved, superseded, rejected, or escalated.

### 5. Anti-Contamination Barriers

This stack treats contamination as a first-class failure mode.

Contamination includes:

* hallucinated evidence,
* speculative output treated as source,
* critic hallucination treated as authority,
* failed repair becoming canonical,
* unverified external artifacts entering memory,
* contaminated run updating strategy or trust.

### 6. Contamination-Free Learning

Strategy learning only uses clean execution records.

If `contamination_free = false`, the run is excluded from:

* strategy updates,
* trust graph updates,
* collaboration health updates,
* validated memory,
* benchmark learning.

### 7. Policy Attestation

PCL records which policies were active for a context.

A policy attestation includes:

* context hash,
* effective policy set hash,
* conflict proof IDs,
* exception windows,
* compiler version,
* timestamp.

This supports reproducible execution.

### 8. Deterministic Policy Composition

Policies may come from many sources:

* organization,
* project,
* domain pack,
* coordination overlay,
* safety rules,
* temporary override.

PCL compiles them into one deterministic effective policy set.

Ambiguity blocks by default.

### 9. Strategy Memory Ledger

SLL learns how to work better without modifying artifacts directly.

It stores explicit patches to strategy tables:

* routing priors,
* repair patterns,
* validation thresholds,
* escalation triggers,
* merge preferences.

These patches are reversible.

### 10. Benchmark Regression Gate

Strategy updates must pass a benchmark gate before becoming live.

This prevents the system from learning harmful patterns from short-term success.

### 11. Value-of-Information Decisions

EIL models whether another action is worth it.

Actions may include:

* retrieve more,
* run another audit,
* call a tool,
* ask a human,
* run a benchmark,
* stop.

The system should know when not to continue.

### 12. Human Review Load Model

Human review is treated as scarce and costly.

The system should not escalate every ambiguity to humans. It should model:

* reviewer load,
* latency,
* cost,
* specialization,
* reversal rate,
* expected value.

### 13. Disagreement Preservation

Some disagreement is useful.

Instead of forcing a premature merge, CHL can preserve disagreement as an artifact with:

* divergence type,
* evidence,
* non-merge reasons,
* intervention prompt,
* revalidation cycle.

### 14. Collaboration Health Metrics

CHL turns “team health” into measurable signals:

* boundary violation rate,
* cross-boundary churn,
* note density,
* blocking rate,
* resolution efficiency,
* trust decay,
* stale disagreement count,
* emergence risk.

### 15. Decaying Trust Graph

Trust is not a popularity score.

It is:

* directed,
* explainable,
* decaying,
* reversible,
* based on observable behavior,
* and contamination-safe.

### 16. Emergence Risk Auditing

CHL detects harmful coordination patterns such as:

* circular dependencies,
* optimization conflicts,
* feedback amplification,
* unresolved blocking loops.

Oscillation detection is delegated to TGL.

### 17. Temporal Governance

TGL manages multi-cycle work over time.

It handles:

* aging,
* archival,
* reopening,
* deadlock,
* oscillation,
* rollback,
* speculative branches,
* governance debt.

### 18. Oscillation Detection

Long-running agent systems may flip artifacts between states:

```text
A → B → A → B
```

TGL detects these patterns and can freeze or escalate the artifact.

### 19. Speculative Branch Preservation

Rejected branches are not always worthless.

TGL can preserve speculative branches in isolation. If mainline fails later, an alternate branch may be revived.

### 20. Rollback with Policy and Strategy Pinning

Rollback is not only artifact rollback.

It also needs:

* policy attestation ID,
* strategy ledger head,
* artifact snapshot,
* note snapshot,
* checkpoint hash,
* lineage preservation.

### 21. Constitutional Review Cycle

Governance rules themselves can become stale.

TGL schedules review of governance parameters, policies, TTLs, thresholds, and debt.

### 22. Semantic Compression Guard

Summaries can destroy important caveats.

A semantic compression guard checks whether a compressed artifact lost:

* uncertainty,
* provenance,
* caveats,
* conflicting evidence,
* scope limitations.

### 23. Unknown-Unknown Probe

Some tasks need active search for missing assumptions.

An unknown-unknown probe asks:

* What hidden variable is absent?
* What frame is missing?
* What would falsify this?
* What evidence would change the decision?
* What assumption is being silently inherited?

### 24. Anti-Fragile Repair Loop

Instead of only fixing failures, the system can test repairs against controlled perturbations.

Examples:

* alternate agent,
* alternate tool,
* changed prompt order,
* different validation sequence,
* stricter schema.

The repair that survives perturbation becomes preferred.

## Summary Table

| Pattern                   |     Common? | Novelty in This Stack                           |
| ------------------------- | ----------: | ----------------------------------------------- |
| Planner/worker/critic     |         Yes | Formal boundary and contamination rules         |
| Validation                |         Yes | Anti-contamination-aware validation             |
| Supervisor                |         Yes | Supervisor cannot silently violate governance   |
| Artifact ownership        |    Somewhat | Explicit forbidden actions and transfer rules   |
| Dependency notes          | Less common | Structured lifecycle and blocking semantics     |
| Policy composition        | Less common | Deterministic precedence + attestation          |
| Strategy learning         | Less common | Reversible, benchmark-gated, contamination-safe |
| VOI decisioning           |        Rare | Cost, trust, latency, human review, regret      |
| Collaboration health      |        Rare | Quantified boundary/trust/disagreement metrics  |
| Temporal governance       |        Rare | Aging, rollback, oscillation, governance debt   |
| Disagreement preservation |        Rare | First-class preserved disagreement artifacts    |
| Anti-contamination memory |        Rare | Clean-run-only learning and health updates      |

## Practical Novelty Claim

The novelty of this project is not “agents collaborate.”

The novelty is:

> Agents collaborate under explicit governance, artifact boundaries, contamination barriers, temporal rollback, value-aware decisioning, and reversible strategy learning.

That is the core contribution.
