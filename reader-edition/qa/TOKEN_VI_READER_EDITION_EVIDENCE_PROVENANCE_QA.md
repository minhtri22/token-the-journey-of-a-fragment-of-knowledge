# TOKEN_VI_READER_EDITION_EVIDENCE_NOTES_AND_PROVENANCE_COMPLETION

**Branch:** `reader-edition-v1`  
**Scope:** six initial Evidence Notes only  
**Part IV adjudication:** `5d9fc1f3fdeddd1e925db82f79addf4eef94bc01`  
**Provenance materialization commit:** `8e728ce2d8066a91282b2199b6843ec3843577f1`  
**Science rerun:** NONE  
**Model/counter rerun:** NONE  
**Method:** exact canonical-source retrieval + independent transcription/arithmetic/cross-source checks

## Verdict

```text
SIX EVIDENCE NOTES
RESOLVED TO CANONICAL SCIENTIFIC RECORDS

RAW ARTIFACT AVAILABILITY
PARTIAL — EXPLICITLY RECORDED

EXACT VALUE TRANSCRIPTION
PASS

DERIVED ARITHMETIC RECOMPUTATION
PASS

M3-A CROSS-SOURCE CHECK
PASS

M3-C FINAL VERDICT CROSS-PROJECT CHECK
PASS

FALSE SOURCE SUBSTITUTION
PREVENTED

DOWNGRADE TO ILLUSTRATION
NONE REQUIRED

OVERALL
PASS WITH EXPLICIT RAW-ARTIFACT LIMITATIONS
```

## 1. Canonical Token-XRay scientific record

The primary audited Token-XRay record for the book's L2/L3-A/L3-C evidence is:

- repository: `minhtri22/token-xray`
- commit: `17baf9e9e561bdb5efe9904dd4cd678f9e19368d`
- artifact: `lineage.md`
- blob: `87b1b73fad6552a8f758589a733fe497c189d435`
- commit purpose: audited scientific lineage

The record contains the exact L2 timing/attribution values and the exact L3-C directional/adequacy values used in the Reader Edition.

## 2. Evidence Note adjudication

| Evidence Note | Provenance adjudication |
|---|---|
| `E-TRACE-01` | LINEAGE-RESOLVED |
| `E-DECOMP-01` | LINEAGE-RESOLVED |
| `E-TRACE-02` | LINEAGE-RESOLVED + arithmetic recomputed |
| `E-HOTSPOT-01` | LINEAGE-RESOLVED + arithmetic recomputed |
| `E-COUNTER-01` | COMPOSITE-RESOLVED |
| `E-MECH-01` | LINEAGE-RESOLVED + verdict cross-confirmed |

No measured-result callout requires downgrade to illustration because each claim resolves to an exact canonical scientific record.

## 3. Independent L2 arithmetic verification

### Device-span accounting

```text
1,240,766,914 - 1,239,803,214 = 963,700 ns

963,700 / 1,240,766,914 × 100
= 0.0776697048515915%
≈ 0.078%
```

Result: **PASS**.

### Hotspot percentages

Recomputed from exact source durations against `1,239,803,214 ns` total measured dispatch duration:

- LM head: `63.9108745688%` → **63.9109%**
- FFN-down: `16.2658688672%` → **16.2659%**
- FFN-up: `6.0135766030%` → **6.0136%**
- FFN-gate: `5.9812125152%` → **5.9812%**
- exact top-four share: `92.1715325542%` → **92.1715%**

Result: **PASS**.

Important rounding note: the four displayed rounded percentages sum to `92.1716%`; the canonical `92.1715%` is correctly derived from the exact nanosecond durations.

## 4. E-COUNTER-01 cross-source verification

ArcLLM corrected M3-A artifact:

- commit: `b2d6966c3e69468eebe2174437212892a96d0898`
- artifact: `artifacts/ARCLLM_V1/ARCLLM_V1_M3A_FINAL_ADJUDICATION_v0.1.json`
- blob: `d59ee2a703e6cbccb8c69e0e06aca692b0eeb031`
- counters: **268**
- full-inventory passes: **12**
- corrected bundle SHA256: `6F0A39836896EAB0755B9A3BE54AF057971E90F9ABE47FC4CA1F397D5B533BAA`

Independent Token-XRay counter profile:

- commit: `2e8408f482b9aa5de2f1f0f49133ab8e8ce6af67`
- artifact: `profiles/intel/xe2_lpg/arc_140v_vulkan_counter_selection.v0.1.json`
- blob: `713123847588e7c46595b5203fc49a4e154e27f3`
- independently matches **268 / 12** and the corrected bundle SHA256

Result: **PASS**.

## 5. L3-C adequacy/verdict verification

Token-XRay audited lineage records:

- `GpuTime = 0` in all three selected groups;
- `XVE_STALL` cross-group inconsistency;
- `GPU_MEMORY_REQUEST_QUEUE_FULL` cross-group inconsistency;
- formal result `STOP_M3C_UNRESOLVED_COUNTER_ADEQUACY`;
- no formal mechanism support and no causal result.

ArcLLM independently converges the same closeout at:

- commit: `c9a38b07e4f70c3c800f09e0561cc69617a578c0`
- artifact: `programs/arcllm_v1/lineage.md`
- blob: `23e52aebaf80482861d35e6a46d1da3a13e44324`
- section: `M3-C targeted hardware-counter closeout convergence — 2026-09-29`

It confirms 6/6 quiet-host raw runs, 618/618 targeted observations, full 469-dispatch execution on required passes, invalid/unavailable zero-valued counter channels, and the same unresolved result.

Result: **PASS**.

Required book boundary remains:

> **counter = 0 ≠ physical quantity = 0**

## 6. E-MECH-01 exact-value verification

The Ch.19 values are specifically **W-S** directional observations:

- DRAM-read amplification: `4.268 vs 1.436`
- LSC hit fraction: `0.103 vs 0.738`
- XVE SBID stall: `81.78% vs 56.50%`
- ALU1 utilization: `~1.58% vs ~13.84%`

All four pairs match the Token-XRay audited L3-C record exactly.

Independent arithmetic:

`4.268 / 1.436 = 2.9721448468×`, consistent with the source's reported `2.972×` ratio.

Result: **PASS at canonical-lineage level**.

Required verdict remains:

> **UNRESOLVED / no confirmed mechanism**

## 7. Provenance correction made during this gate

Ch.19 previously presented the four directional values without explicitly naming their workload scope.

The audit established that these exact four pairs are the **W-S** comparison. Ch.19 was corrected to say:

> `Trong W-S của phép đo mục tiêu...`

No numeric value and no scientific verdict changed.

## 8. False-source substitution prevented

The earlier ArcLLM M3-B hardware-mechanism map contains a different earlier counter aggregation.

It is **not** the source of the book's exact `4.268 / 1.436 / 0.103 / 0.738 / ...` values.

`E-MECH-01` now explicitly records this non-source boundary to prevent future provenance drift.

## 9. Raw-artifact availability audit

The canonical trees inspected do not contain committed L2 acceptance outputs such as:

- `TOKEN_TRACE.json`
- `RUNTIME_TRACE_SUMMARY.json`
- `NODE_LEDGER_TRACED.json`

The final L3-C raw/recomputation package was also not found as a committed canonical artifact.

Therefore the audit does **not** claim raw-artifact reproducibility from Git for L2 acceptance or final L3-C recomputation.

This limitation does not erase the measured evidence: the exact results are preserved in the audited canonical scientific lineage and, for the M3-C verdict, independently converged downstream.

## 10. Body-to-source QA

Verified Reader Edition body ↔ canonical source agreement for:

- `469 / 451` trace counts;
- `1 → 19` LM-head decomposition;
- device-span exact values;
- all hotspot percentages;
- `268 / 12` provider inventory;
- counter-zero boundary;
- all four W-S L3-C directional pairs;
- unresolved mechanism verdict.

All checks: **PASS**.

## 11. Scope isolation

The provenance materialization commit changed only:

- `reader-edition/EVIDENCE_NOTES.md`
- `reader-edition/chapters/19-khuon-mau-dep-chua-phai-nguyen-nhan.md` — scope wording only

No other chapter prose was modified.

`main` remained unchanged at `e7404e26bd0d58301f9999d802e6dc05469e4354`.

## 12. Adjudication

`TOKEN_VI_READER_EDITION_EVIDENCE_NOTES_AND_PROVENANCE_COMPLETION` is:

**PASS WITH EXPLICIT RAW-ARTIFACT LIMITATIONS**.

All six initial Evidence Notes now have canonical source/commit/artifact-or-lineage pointers, exact claim scopes, and evidence limitations.

The next valid editorial step is:

`TOKEN_VI_READER_EDITION_GLOSSARY_BUILD`

That step may build the Reader Edition glossary only. It must not silently alter evidence values, scientific verdicts, or provenance pointers.
