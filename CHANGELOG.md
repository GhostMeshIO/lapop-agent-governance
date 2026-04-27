# Changelog

All notable changes to this project will be documented in this file.

This project follows a lightweight semantic versioning style:

```text
MAJOR.MINOR.PATCH

MAJOR = breaking protocol/schema changes
MINOR = new specs, schemas, layers, or compatible protocol additions
PATCH = clarifications, typo fixes, documentation cleanup, non-breaking refinements
````

---

## [0.1.0] - 2026-04-27

### Added

* Initial public specification release for `lapop-agent-governance`.
* Added project README describing the purpose, scope, roadmap, and governance philosophy.
* Added core stack overview:

  * `LAPOP-003` — LLM-Agent Prompt Orchestration Protocol
  * `CDO-003` — Coordination Discipline Overlay
  * `PCL-003` — Policy Composition Layer
  * `SLL-003` — Strategy Learning Layer
  * `EIL-003` — Economic Intelligence Layer
  * `CHL-003` — Collaboration Health Layer
  * `TGL-003` — Temporal Governance Layer
* Added proposed repository structure:

  * `docs/`
  * `specs/`
  * `prompts/`
  * `schemas/`
  * `examples/`
  * `research/`
  * `roadmap/`
* Added public framing for contamination-safe multi-agent orchestration.
* Added initial roadmap from `v0.1` through `v1.0`.
* Added recommended Apache-2.0 licensing direction.

### Included Concepts

* Artifact boundaries.
* Dependency notes.
* Supervisor-mediated consolidation.
* Policy composition.
* Policy attestation.
* Strategy memory ledger.
* Learning quarantine gate.
* Benchmark regression gate.
* Economic value-of-information decisions.
* Tool trust calibration.
* Human review cost modeling.
* Collaboration health reports.
* Trust graph and disagreement preservation.
* Temporal aging, archive, rollback, and oscillation governance.
* Anti-contamination rules.
* Reproducibility profiles.

### Status

* Protocol/specification stage.
* Reference implementation pending.
* Shared schema normalization pending.
* Public examples pending.

---

## [0.2.0] - Planned

### Planned

* Normalize shared schemas across all layers.
* Add canonical schema files:

  * `execution-context.schema.yaml`
  * `execution-record.schema.yaml`
  * `dependency-note.schema.yaml`
  * `artifact-boundary.schema.yaml`
  * `policy-attestation.schema.yaml`
  * `strategy-update-patch.schema.yaml`
  * `collaboration-health-report.schema.yaml`
  * `rollback-candidate.schema.yaml`
* Remove duplicated schema definitions across specs.
* Define canonical naming rules for shared fields.
* Add glossary of core protocol terms.
* Add cross-layer interface matrix.

---

## [0.3.0] - Planned

### Planned

* Add minimal reference runner design.
* Define simple execution cycle:

  * create task
  * assign artifact
  * enforce boundary
  * collect worker output
  * validate dependency notes
  * emit execution record
  * supervisor merge/reject
* Add example local workflow for multi-agent code repair.
* Add example QNVM-style agent cycle.
* Add example research-audit cycle.

---

## [0.4.0] - Planned

### Planned

* Add Policy Composition MVP.
* Add `is_action_allowed()` reference interface.
* Add exception window examples.
* Add policy conflict proof examples.
* Add policy attestation examples.
* Add temporal checkpoint examples.

---

## [0.5.0] - Planned

### Planned

* Add Collaboration Health MVP.
* Add boundary-health metrics.
* Add note-density and cross-boundary churn examples.
* Add simple trust graph schema.
* Add disagreement preservation examples.
* Add emergence-risk signal examples.

---

## [0.6.0] - Planned

### Planned

* Add Strategy Learning MVP.
* Add outcome collector schema.
* Add learning quarantine gate.
* Add strategy update patch examples.
* Add benchmark regression gate examples.
* Add reversible strategy ledger examples.

---

## [0.7.0] - Planned

### Planned

* Add Economic Intelligence MVP.
* Add value-of-information decision record.
* Add stop/probe decision examples.
* Add tool trust calibration matrix.
* Add human review load model.
* Add economic regret tracker.

---

## [1.0.0] - Planned

### Planned

* Integrated reference platform specification.
* End-to-end protocol example.
* Multi-agent benchmark suite.
* Stable shared schemas.
* Stable prompt packs.
* Stable core layer specs.
* Initial reference implementation candidate.
