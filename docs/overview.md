# Project Overview

## LAPOP Agent Governance

`lapop-agent-governance` is a protocol and architecture stack for auditable, contamination-safe multi-agent LLM orchestration.

It is designed for workflows where multiple models or agents collaborate across files, modules, schemas, research artifacts, audits, reports, or long-running project cycles.

The core goal is simple:

> Make multi-agent systems less magical, less fragile, less contaminated, and more accountable.

This project grew from practical multi-model coordination patterns: supervisor sessions, worker sessions, artifact boundaries, dependency notes, validation reports, audit passes, merge discipline, rollback points, and contamination-safe learning. The consolidated project folder includes launcher prompts, strict multi-model coordination rules, common-vs-novel analysis, and a novelty automation syntax for turning ideas into structured architecture. 

## Why This Exists

Most multi-agent systems already support:

- planner / worker / critic loops,
- task decomposition,
- tool calls,
- output validation,
- retries,
- and final synthesis.

That is useful, but insufficient for serious long-running work.

Real multi-agent systems need governance:

- Who owns which artifact?
- What is an agent forbidden to edit?
- How are cross-file changes requested?
- How are policy conflicts resolved?
- How do we prevent hallucinated critique from becoming trusted evidence?
- How do we preserve valid disagreement?
- How do we know a strategy update came from a clean run?
- How do we rollback a multi-cycle execution?
- How do we decide whether another tool call, audit, retrieval, or human review is worth the cost?

`lapop-agent-governance` answers these questions with a layered control-plane design.

## Core Stack

| Layer | Name | Purpose |
|---|---|---|
| `LAPOP-003` | LLM-Agent Prompt Orchestration Protocol | Main execution backbone |
| `CDO-003` | Coordination Discipline Overlay | Artifact boundaries and dependency notes |
| `PCL-003` | Policy Composition Layer | Deterministic policy conflict resolution |
| `SLL-003` | Strategy Learning Layer | Contamination-safe learning from outcomes |
| `EIL-003` | Economic Intelligence Layer | Value-of-information and cost decisions |
| `CHL-003` | Collaboration Health Layer | Trust, disagreement, boundary health, emergence risk |
| `TGL-003` | Temporal Governance Layer | Aging, rollback, oscillation, archival, deadlock control |

## Primary Design Principles

### 1. Bound the agents

Agents should not freely modify every artifact. Each worker receives an assignment and an artifact boundary.

If an agent needs another artifact changed, it emits a dependency note.

### 2. Ground every claim

Claims must be based on direct inspection, evidence, tool output, schema validation, test output, citations, or explicit uncertainty.

Unsupported assertions are treated as grounding failures.

### 3. Prevent contamination

Speculation, hallucinated critique, failed repair attempts, and unverified external material must not enter validated memory or learning pipelines.

A run must explicitly track whether it is `contamination_free`.

### 4. Preserve disagreement

Not all disagreement should be merged away. Some divergence should become a first-class artifact with non-merge reasons and revalidation cycles.

### 5. Learn safely

The platform may learn better routing, repair, validation, and escalation strategies, but only through reversible, benchmark-gated strategy patches from clean runs.

### 6. Make rollback possible

Long-running systems need checkpoints, lineage, version pinning, and rollback candidates.

A good system must be able to explain not only what it did, but how it got there.

## What This Is Not

This project is not:

- an AGI claim,
- a model consciousness framework,
- an automatic truth machine,
- a replacement for human judgment,
- an unrestricted self-modifying agent,
- or a production-ready autonomous platform.

It is a governance architecture for disciplined multi-agent execution.

## Intended Users

This project may be useful for:

- developers building local multi-agent coding systems,
- researchers coordinating multiple LLM sessions,
- teams running agentic code repair,
- AI safety or governance researchers,
- prompt engineers building structured multi-model workflows,
- open-source maintainers experimenting with agent-assisted development,
- and anyone who wants agent systems to be more auditable and less chaotic.

## Current Status

```yaml
Project_Status: Early_Public_Specification
Version: 0.1
Reference_Implementation: Pending
Primary_Output: Specs, schemas, prompts, examples, and roadmap
````

## Current Priorities

1. Publish the consolidated protocol documents.
2. Normalize shared schemas.
3. Define canonical execution records.
4. Create public prompt packs.
5. Build a minimal reference runner.
6. Add examples and benchmark workflows.
7. Implement contamination-safe strategy learning.
8. Add policy, temporal, collaboration, and economic governance modules.

## Short Summary

`lapop-agent-governance` is a layered protocol stack for making multi-agent LLM systems:

* more modular,
* more accountable,
* more reproducible,
* more contamination-resistant,
* more rollback-safe,
* and more useful for real work.
