# Evidence Notes — Reader Edition v1

## Provenance policy

Every real measured-result callout must point to a canonical research record.

Three provenance levels are used:

- **RAW-RESOLVED** — committed raw/frozen artifact plus adjudication are available.
- **LINEAGE-RESOLVED** — an exact audited canonical lineage record exists, but the underlying raw run artifact is not committed in the canonical repositories inspected.
- **COMPOSITE-RESOLVED** — different parts of the claim are resolved by multiple canonical artifacts/lineage records.

`LINEAGE-RESOLVED` remains measured research evidence; it is not silently downgraded to illustration. Its limitation is explicit: a reader cannot reconstruct the complete raw run solely from the canonical Git repositories.

No source below is inferred from filename similarity. Every pointer was checked at the exact commit listed.

---

## E-TRACE-01 — Dispatch / logical-operation coverage

**Used in:** Ch.11, Ch.13, Ch.15  
**Provenance level:** **LINEAGE-RESOLVED**  
**Scientific status:** **PASS — L2 Vulkan RuntimeTrace on-device acceptance**

### Claim supported

In the accepted one-token Vulkan trace:

- physical dispatches = **469**
- timestamped dispatches = **469**
- barrier records = **470**
- measured semantic/logical nodes = **451**
- exact-timing semantic nodes = **451**
- shared-dispatch nodes = **0**
- unmeasured semantic nodes = **0**
- unknown semantic IDs = **0**
- unmapped dispatch IDs = **0**

### Canonical source

- repository: `minhtri22/token-xray`
- audited lineage commit: `17baf9e9e561bdb5efe9904dd4cd678f9e19368d`
- artifact: `lineage.md`
- artifact blob: `87b1b73fad6552a8f758589a733fe497c189d435`
- lineage entry: `2026-09-25 — L2: Vulkan RuntimeTrace on-device acceptance`
- verdict: `PASS`

### Supporting execution/integration reference

- repository: `minhtri22/ArcLLM`
- branch/freeze: `integration/token-xray-phase2`
- branch head inspected: `5af37f21345793eacd149cae2c99901bc62ecfae`
- role: Phase-2 integration/acceptance harness, **not** the committed raw acceptance result

### Raw-artifact availability

At the audited Token-XRay lineage commit and the ArcLLM Phase-2 integration tree inspected, no committed acceptance `TOKEN_TRACE.json`, `RUNTIME_TRACE_SUMMARY.json`, `NODE_LEDGER_TRACED.json`, or equivalent raw-result artifact was found.

Therefore the canonical evidence pointer is the audited scientific lineage, not a fabricated raw path.

### Allowed interpretation

- exact coverage/attribution facts for the accepted trace
- physical dispatch count is not identical to logical-operation count

### Not supported

- universal dispatch/node counts for other models, runtimes, or runs
- the claim that fusion can never occur

---

## E-DECOMP-01 — LM-head physical decomposition

**Used in:** Ch.11, Ch.12, Ch.15  
**Provenance level:** **LINEAGE-RESOLVED**  
**Scientific status:** **PASS — L2**

### Exact measured result

- LM-head Q6 = **1 semantic node / 19 physical dispatches**

### Canonical source

- repository: `minhtri22/token-xray`
- commit: `17baf9e9e561bdb5efe9904dd4cd678f9e19368d`
- artifact: `lineage.md`
- blob: `87b1b73fad6552a8f758589a733fe497c189d435`
- lineage entry: `2026-09-25 — L2: Vulkan RuntimeTrace on-device acceptance`
- subentry: `Quantization census cho các target family về sau`

The ArcLLM Phase-2 integration implementation states that all 19 LM-head chunks map intentionally to the same semantic NodeID, but the measured `1 → 19` acceptance result is sourced to the audited Token-XRay L2 record.

### Allowed interpretation

- one-to-many logical→physical decomposition occurred in the accepted trace

### Not supported

- why the runtime chose exactly 19 dispatches
- a universal LM-head dispatch count

---

## E-TRACE-02 — Device-span accounting

**Used in:** Ch.15  
**Provenance level:** **LINEAGE-RESOLVED + INDEPENDENT ARITHMETIC RECOMPUTATION**  
**Scientific status:** **PASS — L2**

### Exact measured values

- device span = **1,240,766,914 ns**
- summed dispatch duration = **1,239,803,214 ns**
- unattributed device time = **963,700 ns**
- unattributed / device span = **0.0776697048515915%**, reported in prose as **≈ 0.078%**

### Canonical source

- repository: `minhtri22/token-xray`
- commit: `17baf9e9e561bdb5efe9904dd4cd678f9e19368d`
- artifact: `lineage.md`
- blob: `87b1b73fad6552a8f758589a733fe497c189d435`
- lineage entry: `2026-09-25 — L2: Vulkan RuntimeTrace on-device acceptance`

### Independent check

```text
1,240,766,914
-1,239,803,214
----------------
       963,700 ns

963,700 / 1,240,766,914 × 100
= 0.0776697048515915%
≈ 0.078%
```

### Required interpretation boundary

`963,700 ns` is **unattributed device time**.

It must **not** be renamed `barrier cost`; the source explicitly allows gaps, serialization, and timestamp-stage effects among possible contributors.

---

## E-HOTSPOT-01 — Dispatch-time localization

**Used in:** Ch.16  
**Provenance level:** **LINEAGE-RESOLVED + INDEPENDENT ARITHMETIC RECOMPUTATION**  
**Scientific status:** **PASS — localization only**

### Exact source durations

- LM head = **792,369,077 ns**
- FFN-down = **201,664,765 ns**
- FFN-up = **74,556,516 ns**
- FFN-gate = **74,155,265 ns**
- total measured dispatch duration = **1,239,803,214 ns**

### Published percentages

- LM head = **63.9109%**
- FFN-down = **16.2659%**
- FFN-up = **6.0136%**
- FFN-gate = **5.9812%**
- top four = **92.1715%**

### Canonical source

- repository: `minhtri22/token-xray`
- commit: `17baf9e9e561bdb5efe9904dd4cd678f9e19368d`
- artifact: `lineage.md`
- blob: `87b1b73fad6552a8f758589a733fe497c189d435`
- lineage entry: `2026-09-25 — L2: Vulkan RuntimeTrace on-device acceptance`
- subentry: `Hotspot recomputation`

### Independent check

Recomputed from exact nanosecond durations:

- `792,369,077 / 1,239,803,214 = 63.9108745688%` → **63.9109%**
- `201,664,765 / 1,239,803,214 = 16.2658688672%` → **16.2659%**
- `74,556,516 / 1,239,803,214 = 6.0135766030%` → **6.0136%**
- `74,155,265 / 1,239,803,214 = 5.9812125152%` → **5.9812%**
- exact top-four duration = **1,142,745,623 ns**
- `1,142,745,623 / 1,239,803,214 = 92.1715325542%` → **92.1715%**

**Rounding note:** adding the four already-rounded displayed percentages yields `92.1716%`. The canonical `92.1715%` is correctly recomputed from exact nanosecond durations, not from the rounded percentages.

### Allowed interpretation

- localization and prioritization within the accepted trace

### Not supported

- universal LLM profile
- causal mechanism
- guaranteed end-to-end speedup equal to hotspot share

---

## E-COUNTER-01 — Hardware-counter inventory and adequacy failure

**Used in:** Ch.17; adequacy context in Ch.19  
**Provenance level:** **COMPOSITE-RESOLVED**  
**Scientific status:** provider qualification **PASS**; collection integrity **PASS**; counter adequacy **PARTIAL / FAIL FOR CONFIRMATORY MECHANISM DISCRIMINATION**; mechanism **UNRESOLVED**

### A. Provider inventory — artifact-resolved

Exact provider facts:

- exposed counters = **268**
- all-counter inventory = **12 passes**
- scope = `COMMAND`

Primary artifact:

- repository: `minhtri22/ArcLLM`
- corrected provenance commit: `b2d6966c3e69468eebe2174437212892a96d0898`
- artifact: `artifacts/ARCLLM_V1/ARCLLM_V1_M3A_FINAL_ADJUDICATION_v0.1.json`
- blob: `d59ee2a703e6cbccb8c69e0e06aca692b0eeb031`
- result: `PASS_M3A_VULKAN_PERFORMANCE_QUERY_PROVIDER_QUALIFIED`
- collection head: `9ac0d7206b4f128b7660eef52eb26e5d7533874f`
- corrected bundle SHA256: `6F0A39836896EAB0755B9A3BE54AF057971E90F9ABE47FC4CA1F397D5B533BAA`

Independent Token-XRay profile copy:

- repository: `minhtri22/token-xray`
- correction commit: `2e8408f482b9aa5de2f1f0f49133ab8e8ce6af67`
- artifact: `profiles/intel/xe2_lpg/arc_140v_vulkan_counter_selection.v0.1.json`
- blob: `713123847588e7c46595b5203fc49a4e154e27f3`
- profile id: `intel.xe2_lpg.arc_140v.vulkan_khr_perf.m3a_2026_09_25.v0.1`
- independently matches **268 counters / 12 passes** and the corrected M3-A bundle SHA256

### B. Counter-adequacy failure — lineage-resolved

Canonical Token-XRay record:

- repository: `minhtri22/token-xray`
- audited lineage commit: `17baf9e9e561bdb5efe9904dd4cd678f9e19368d`
- artifact: `lineage.md`
- blob: `87b1b73fad6552a8f758589a733fe497c189d435`
- lineage entry: `2026-09-25 — L3-C: Targeted mechanism discrimination bằng hardware counters`

Exact observations preserved there:

- `GpuTime = 0` in all three counter groups
- `XVE_STALL = 0` in execution/occupancy but non-zero in stall-cause
- `GPU_MEMORY_REQUEST_QUEUE_FULL` has non-zero samples in memory/cache but is `0` in stall-cause
- critical clock/occupancy counters are unusable for the frozen discriminator
- formal result: `STOP_M3C_UNRESOLVED_COUNTER_ADEQUACY`

Independent downstream convergence of the verdict:

- repository: `minhtri22/ArcLLM`
- commit: `c9a38b07e4f70c3c800f09e0561cc69617a578c0`
- artifact: `programs/arcllm_v1/lineage.md`
- blob: `23e52aebaf80482861d35e6a46d1da3a13e44324`
- section: `M3-C targeted hardware-counter closeout convergence — 2026-09-29`
- confirms 6/6 quiet-host raw runs, 618/618 targeted `HARDWARE_OBSERVATION` records, full 469-dispatch graph on each required pass, zero-valued clock/occupancy channels treated as invalid/unavailable rather than physical zero, duplicated `XVE_STALL` inconsistency, and result `STOP_M3C_UNRESOLVED_COUNTER_ADEQUACY`

### Raw M3-C artifact availability

The audited L3-C record refers to frozen evidence/recomputation, but the final M3-C raw/recomputation package was not found as a committed artifact in the canonical repositories inspected.

Therefore provider inventory is artifact-resolved, while the final adequacy verdict is independently lineage-resolved across Token-XRay and ArcLLM. Raw-row reanalysis of the final M3-C package cannot be reproduced from Git alone.

### Required boundary

> **counter = 0 ≠ physical quantity = 0**

### Not supported

- physical GPU time was zero
- a confirmed memory/cache/geometry/instruction/clock mechanism

---

## E-MECH-01 — Directional mechanism pattern with unresolved verdict

**Used in:** Ch.14, Ch.19  
**Provenance level:** **LINEAGE-RESOLVED + CROSS-PROJECT VERDICT CONFIRMATION**  
**Scientific status:** **UNRESOLVED / no confirmed mechanism**

### Exact values used in the book

The values printed in Ch.19 are specifically the **W-S** comparison `LM-head Q6 vs FFN-down Q6`:

- DRAM-read amplification: **4.268 vs 1.436**
- LSC hit fraction: **0.103 vs 0.738**
- median XVE SBID stall: **81.78% vs 56.50%**
- median ALU1 utilization: **~1.58% vs ~13.84%**

The audited lineage also preserves the W-C companion values:

- DRAM-read amplification: **2.000 vs 1.212**
- LSC hit fraction: **0.365 vs 0.911**
- median XVE SBID stall: **80.12% vs 56.13%**
- median ALU1 utilization: **~1.15% vs ~14.04%**

### Canonical exact-value source

- repository: `minhtri22/token-xray`
- commit: `17baf9e9e561bdb5efe9904dd4cd678f9e19368d`
- artifact: `lineage.md`
- blob: `87b1b73fad6552a8f758589a733fe497c189d435`
- lineage entry: `2026-09-25 — L3-C: Targeted mechanism discrimination bằng hardware counters`
- subentry: `Directional observations — preserved but NON-CONFIRMATORY`

Independent arithmetic check:

- `4.268 / 1.436 = 2.9721448468×`, consistent with the lineage's reported **2.972×** W-S DRAM-read-amplification ratio

### Verdict cross-check

The final unresolved result is independently converged in:

- repository: `minhtri22/ArcLLM`
- commit: `c9a38b07e4f70c3c800f09e0561cc69617a578c0`
- artifact: `programs/arcllm_v1/lineage.md`
- blob: `23e52aebaf80482861d35e6a46d1da3a13e44324`
- section: `M3-C targeted hardware-counter closeout convergence — 2026-09-29`
- result: `STOP_M3C_UNRESOLVED_COUNTER_ADEQUACY`

### Important non-source

The earlier ArcLLM M3-B hardware-mechanism map contains a different earlier counter aggregation and **must not be cited as the source of the book's exact `4.268 / 1.436 / ...` L3-C values**.

This provenance audit explicitly prevents that incorrect substitution.

### Raw-artifact availability

The final L3-C raw/recomputation package carrying these exact directional summary values was not found as a committed canonical artifact.

The exact values are therefore **lineage-verified measured summaries**, not raw-artifact-recomputable from Git.

### Allowed interpretation

- descriptive/directional pattern
- motivation for a memory-related hypothesis
- evidence that the hypothesis deserved a discriminating test

### Required verdict

> **UNRESOLVED / no confirmed mechanism**

### Not supported

- confirmed memory bottleneck
- confirmed causal mechanism of any kind

---

# Provenance adjudication

```text
E-TRACE-01    LINEAGE-RESOLVED
E-DECOMP-01   LINEAGE-RESOLVED
E-TRACE-02    LINEAGE-RESOLVED + ARITHMETIC RECOMPUTED
E-HOTSPOT-01  LINEAGE-RESOLVED + ARITHMETIC RECOMPUTED
E-COUNTER-01  COMPOSITE-RESOLVED
E-MECH-01     LINEAGE-RESOLVED + VERDICT CROSS-CONFIRMED
```

No Evidence Note is downgraded to illustration because every book claim resolves to an exact canonical scientific record.

Raw-artifact availability is **not overstated**:

- L2 acceptance raw run artifacts were not found committed in the inspected canonical trees.
- final L3-C raw/recomputation artifacts were not found committed in the inspected canonical trees.

The Reader Edition may cite these measured results as adjudicated research evidence, but must preserve those provenance limitations.
