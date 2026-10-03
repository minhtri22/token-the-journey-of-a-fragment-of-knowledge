# TOKEN_VI_READER_EDITION_PART_IV_QA

**Scope:** Chương 17–20  
**Branch:** `reader-edition-v1`  
**Research baseline:** `c57d2488cf9bd198d562e0410163c2e8cd7fd6c5`  
**Part III adjudication:** `197c256da8cbd501c3a9a52d0282b76c05483f8c`  
**Part IV final revision head before this QA record:** `1edf7bfc440a059ebeac887808064d181e4793aa`

## Verdict

```text
CLAIM PRESERVATION
PASS

EXACT-VALUE PRESERVATION
PASS

EVIDENCE-NOTE BODY LINKAGE
PASS

COUNTER-ZERO BOUNDARY
PASS

UNRESOLVED MECHANISM VERDICT
PASS

FIGURE F20–F24 MATERIALIZATION
PASS

NO SPECULATIVE SUCCESSOR CONTENT
PASS

MAIN-BRANCH ISOLATION
PASS

PROVENANCE COMPLETION
PENDING R7
```

## 1. Revision footprint

Only Chapters 17–20 plus the Reader Edition Evidence Note registry were changed in this gate.

Part IV is approximately **8.9% shorter** than the frozen Research Edition baseline. This is intentionally less aggressive than some earlier parts because Part IV is the epistemic close and must preserve measurement/causality boundaries rather than optimize for brevity alone.

## 2. E-COUNTER-01 exact-value QA

Preserved and body-linked in Ch.17:

- exposed counters = **268**;
- full inventory = **12 measurement passes**;
- `GpuTime = 0` in all three counter groups used;
- `XVE_STALL`: execution/occupancy group = 0; stall-cause group = non-zero;
- `GPU_MEMORY_REQUEST_QUEUE_FULL`: memory/cache group contains non-zero samples; stall-cause group = 0.

Required boundary preserved verbatim:

> **counter = 0 ≠ physical quantity = 0**

The book does not promote a counter-channel zero to a claim that the physical quantity itself was zero.

## 3. E-MECH-01 exact-value QA

Preserved and body-linked in Ch.19:

- DRAM read amplification: LM head `4.268`; FFN-down `1.436`;
- LSC hit ratio: LM head `0.103`; FFN-down `0.738`;
- XVE SBID stall: LM head `81.78%`; FFN-down `56.50%`;
- ALU1 utilization: LM head `~1.58%`; FFN-down `~13.84%`.

Required scientific verdict preserved verbatim:

> **UNRESOLVED / no confirmed mechanism**

Directional evidence is retained as hypothesis-generating evidence only. It is not promoted to a confirmed memory bottleneck or other causal mechanism.

## 4. Evidence ladder QA

F23 materializes the frozen sequence:

```text
OBSERVATION
↓
PATTERN
↓
HYPOTHESIS
↓
MEASUREMENT ADEQUACY?
├─ FAIL → UNRESOLVED
└─ PASS
    ↓
DISCRIMINATING TEST / INTERVENTION
    ↓
RESULT
    ↓
CLAIM WITHIN SCOPE
```

The text explicitly preserves that intervention itself is not magic; causal strength comes from a discriminating design plus adequate measurement.

## 5. Chapter 18 QA

F22 materializes:

`small difference → resolution → noise floor → repeatability → structure`.

The chapter preserves the distinction between:

- small but unresolved difference;
- repeatable/structured difference;
- causal or semantic claim.

`representation trajectory` terminology is consistent with Ch.9.

## 6. Chapter 20 epistemic-close QA

F24 materializes the final two-path synthesis:

- model path: token → state → transformation → behavior;
- observer path: phenomenon → measurement → evidence → knowledge.

The final chapter preserves:

> **Đừng nhầm biểu diễn với bản chất. Đừng nhầm phép đo với thực tại.**

The previous speculative successor-question section was replaced by an explicit epistemic stop: the book ends where current evidence ends.

Exact successor project names are absent from Part IV:

- RRE = 0
- SIX = 0
- STATE = 0
- Token-XRay = 0

No unadjudicated successor research is presented as a continuation or conclusion of the book.

## 7. Figure QA

Materialized markers:

- F20 — measurement channel;
- F21 — counter-zero interpretation boundary;
- F22 — small difference / resolution / noise / repeatability;
- F23 — evidence ladder;
- F24 — final two-journey synthesis.

## 8. Structural QA

- Chapters 17–20 each retain exactly one `Nhớ 3 điều` section.
- Chapters 17–19 retain `Tiếp theo` navigation.
- Chapter 20 correctly has no `Tiếp theo` link.
- All Markdown `~~~` fences are balanced.
- `main` remained unchanged at `e7404e26bd0d58301f9999d802e6dc05469e4354`.

## 9. Evidence provenance status

`EVIDENCE_NOTES.md` now contains body linkage, exact measured values, allowed interpretation and unsupported claims for all six initial IDs.

Canonical source repository / commit-or-freeze / source artifact / lineage-entry fields remain explicitly **PENDING**. This QA does not claim provenance completion.

## 10. Adjudication

`TOKEN_VI_READER_EDITION_PART_IV_PROSE_REVISION` is **PASS**.

All four book parts have now completed prose revision + per-part QA.

The next valid editorial step is:

`TOKEN_VI_READER_EDITION_EVIDENCE_NOTES_AND_PROVENANCE_COMPLETION`

That step must:

- resolve the six initial Evidence Notes to canonical research sources;
- fill source repository, exact commit/freeze, artifact and lineage pointers where available;
- independently verify every exact measured value against canonical evidence;
- downgrade any item that cannot be provenance-resolved rather than inventing a source;
- keep `E-MECH-01` verdict UNRESOLVED;
- stop before glossary/full-book continuity editing.