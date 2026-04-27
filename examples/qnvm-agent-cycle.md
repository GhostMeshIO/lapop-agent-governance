# QNVM Agent Cycle Example

## Purpose

This example shows how `lapop-agent-governance` can coordinate a governed multi-model repair cycle for a QNVM simulation script.

The example is based on a realistic workflow:

1. A simulation script crashes or produces unstable metrics.
2. The human operator provides logs, traceback, metrics, or plots.
3. The supervisor decomposes the problem.
4. Implementation workers propose repairs.
5. Validation checks confirm whether the repair worked.
6. Audit workers identify regressions or hidden issues.
7. Economic intelligence decides whether to continue or stop.
8. Temporal governance records checkpoints and rollback state.
9. Strategy learning may propose a low-confidence candidate patch only if the run is clean.

This is an example, not a required implementation.

---

## Scenario

The human operator is developing a QNVM artificial-life simulation.

The current run prints:

```text
QNVM v0.7.1 – Noise & Death Patch
Bootstrap phase: first 20 generations
Target: healthy_pop 5000-7000, collapse_frac 0.02-0.10, coherence ≥0.55, death_rate <0.10

gen=10 pop=1943 healthy=1077 collapsed=0.446 death=0.317 births=3369 deaths=1526 res=254 rec=80 dt=0.653s
gen=20 pop=7471 healthy=4840 collapsed=0.352 death=0.269 births=14745 deaths=7374 res=2346 rec=565 dt=7.456s
gen=30 pop=9404 healthy=4975 collapsed=0.471 death=0.266 births=30972 deaths=21668 res=4979 rec=590 dt=15.406s
gen=40 pop=9790 healthy=4276 collapsed=0.563 death=0.343 births=46781 deaths=37091 res=7143 rec=661 dt=19.416s
gen=50 pop=9744 healthy=4491 collapsed=0.539 death=0.346 births=61879 deaths=52235 res=10348 rec=835 dt=18.367s
