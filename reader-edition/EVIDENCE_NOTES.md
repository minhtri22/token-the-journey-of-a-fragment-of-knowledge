# Evidence Notes — schema and initial registry

## Schema

```yaml
id:
title:
used_in_chapters:
claim_supported:
exact_measured_values:
measurement_scope:
model:
runtime_or_execution_path:
hardware:
source_repository:
source_commit_or_freeze:
source_artifact:
lineage_entry:
qa_or_adjudication_status:
allowed_interpretation:
not_supported:
notes:
```

## Initial IDs

- E-TRACE-01 — Dispatch / logical-operation coverage
- E-DECOMP-01 — LM-head physical decomposition
- E-TRACE-02 — Device-span accounting
- E-HOTSPOT-01 — Dispatch-time localization
- E-COUNTER-01 — Hardware-counter inventory and adequacy failure
- E-MECH-01 — Directional mechanism pattern with unresolved verdict

Commercial release gate: every measured-result callout must resolve to a canonical provenance pointer or be explicitly downgraded to illustration.


## Part III body linkage

Status below records **book-body linkage only**. Canonical source repository / freeze / artifact / lineage fields remain **PENDING R7 PROVENANCE COMPLETION**.

### E-TRACE-01 — Dispatch / logical-operation coverage

- Used in: Ch.11, Ch.13, Ch.15
- Exact measured values currently cited:
  - 469 physical dispatches
  - 469 dispatches with timestamps
  - 470 barrier records
  - 451 measured logical operations
  - unmeasured logical operations = 0
  - unknown logical identities = 0
  - unattributed dispatches = 0
  - shared-logical-operation-per-dispatch cases observed in this trace = 0
- Allowed interpretation:
  - trace-level coverage / attribution properties of the measured run
  - physical-dispatch count is not identical to logical-operation count
- Not supported:
  - universal counts for other runs/models
  - claim that fusion never occurs
- Provenance status: **PENDING**

### E-DECOMP-01 — LM-head physical decomposition

- Used in: Ch.11, Ch.12, Ch.15
- Exact measured value:
  - 1 LM-head logical operation → 19 physical dispatches
- Allowed interpretation:
  - direct one-to-many mapping in the measured trace
- Not supported:
  - why the runtime chose exactly 19
  - universal LM-head decomposition count
- Provenance status: **PENDING**

### E-TRACE-02 — Device-span accounting

- Used in: Ch.15
- Exact measured values:
  - device span = 1,240,766,914 ns
  - summed dispatch duration = 1,239,803,214 ns
  - difference = 963,700 ns
  - difference / device span ≈ 0.078%
- Allowed interpretation:
  - unattributed device time within the measured trace
- Not supported:
  - treating all 963,700 ns as barrier cost
- Provenance status: **PENDING**

### E-HOTSPOT-01 — Dispatch-time localization

- Used in: Ch.16
- Exact measured values:
  - LM head = 63.9109%
  - FFN-down = 16.2659%
  - FFN-up = 6.0136%
  - FFN-gate = 5.9812%
  - top four = 92.1715%
- Scope:
  - percentage of total measured dispatch time in the target trace
- Allowed interpretation:
  - localization / prioritization
- Not supported:
  - universal profile
  - causal mechanism
  - guaranteed end-to-end speedup equal to hotspot share
- Provenance status: **PENDING**

### E-MECH-01 — Directional mechanism pattern with unresolved verdict

- Used in: Ch.14, Ch.19
- Exact measured values cited in Ch.19:
  - DRAM read amplification: LM head = 4.268; FFN-down = 1.436
  - LSC hit ratio: LM head = 0.103; FFN-down = 0.738
  - XVE SBID stall: LM head = 81.78%; FFN-down = 56.50%
  - ALU1 utilization: LM head ≈ 1.58%; FFN-down ≈ 13.84%
- Ch.14/19 interpretation:
  - directional pattern motivating a memory-related hypothesis
- Required scientific verdict:
  - **UNRESOLVED / no confirmed mechanism**
- Not supported:
  - causal claim that memory is the confirmed bottleneck
- Provenance status: **PENDING**

### E-COUNTER-01 — Hardware-counter inventory and adequacy failure

- Used in: Ch.17; supports adequacy context in Ch.19
- Exact measured values / observations:
  - exposed counters = 268
  - inventory measurement passes = 12
  - GpuTime = 0 in all three counter groups used
  - XVE_STALL: execution/occupancy group = 0; stall-cause group = non-zero
  - GPU_MEMORY_REQUEST_QUEUE_FULL: memory/cache group includes non-zero samples; stall-cause group = 0
- Allowed interpretation:
  - the selected measurement channel was inadequate for the target causal-mechanism confirmation
  - counter value 0 cannot be promoted to physical quantity 0 when the channel is inadequate
- Required boundary:
  - **counter = 0 ≠ physical quantity = 0**
- Required scientific consequence:
  - measurement adequacy FAIL → mechanism remains **UNRESOLVED**
- Not supported:
  - physical GPU time was zero
  - a confirmed memory bottleneck
- Provenance status: **PENDING**
