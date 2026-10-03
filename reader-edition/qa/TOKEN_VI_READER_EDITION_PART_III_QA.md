# TOKEN_VI_READER_EDITION_PART_III_QA

**Scope:** Chương 11–16  
**Branch:** `reader-edition-v1`  
**Research baseline:** `c57d2488cf9bd198d562e0410163c2e8cd7fd6c5`  
**Part II adjudication:** `3173f6a87c7ae2987156beb2d9052c23c9b1f0c1`  
**Part III final revision head before this QA record:** `d4dbff95910b80ea5511e2d33cf121fa1e7659b1`

## Verdict

```text
CLAIM PRESERVATION
PASS

EXACT-VALUE PRESERVATION
PASS

EVIDENCE-NOTE BODY LINKAGE
PASS

PROVENANCE COMPLETION
PENDING R7 — NOT CLAIMED COMPLETE

TERMINOLOGY / PREREQUISITE
PASS

FIGURE F13–F19 MATERIALIZATION
PASS

LOCALIZATION ≠ MECHANISM BOUNDARY
PASS

PART III SCOPE ISOLATION
PASS

MAIN-BRANCH ISOLATION
PASS
```

## 1. Revision footprint

Only Chapters 11–16 plus the Reader Edition Evidence Note registry were modified.

Part III prose is approximately **10.1% shorter** than the frozen Research Edition baseline.

## 2. Exact measured values preserved

### E-TRACE-01
- 469 physical dispatches
- 469 dispatches with timestamps
- 470 barrier records
- 451 measured logical operations
- unmeasured logical operations = 0
- unknown logical identities = 0
- unattributed dispatches = 0
- shared-logical-operation-per-dispatch cases observed in the target trace = 0

### E-DECOMP-01
- 1 LM-head logical operation → 19 physical dispatches

### E-TRACE-02
- device span = `1,240,766,914 ns`
- summed dispatch duration = `1,239,803,214 ns`
- difference = `963,700 ns`
- difference / device span ≈ `0.078%`

### E-HOTSPOT-01
- LM head = `63.9109%`
- FFN-down = `16.2659%`
- FFN-up = `6.0136%`
- FFN-gate = `5.9812%`
- top four = `92.1715%`

## 3. Evidence-classification QA

All real Part III measurements are now marked with `KẾT QUẢ ĐO — Measured Result` and linked to an Evidence Note ID.

Body-linked IDs used in Part III:
- `[E-TRACE-01]`
- `[E-DECOMP-01]`
- `[E-TRACE-02]`
- `[E-HOTSPOT-01]`
- `[E-MECH-01]`

`EVIDENCE_NOTES.md` now records exact values, allowed interpretation and unsupported claims for these body links.

Canonical source repository / freeze / artifact / lineage pointers remain explicitly **PENDING**. This is intentional: formal provenance completion belongs to R7 and has not been fabricated or prematurely claimed.

## 4. Claim-preservation QA

Preserved boundaries include:

- logical/model-level operation ≠ kernel ≠ dispatch;
- 469/451 demonstrates a non-one-to-one mapping in the target trace, not a universal count;
- 19 dispatches demonstrates LM-head one-to-many decomposition in the target trace, not why exactly 19 were chosen;
- more dispatches does not automatically mean slower;
- fusion is a valid execution possibility, but the target trace does **not** directly demonstrate fusion;
- memory-related directional pattern is hypothesis-supporting evidence only;
- cache-hit or memory-traffic signals alone do not establish memory-bound causality;
- trace/timestamps localize execution time but do not explain mechanism;
- `963,700 ns` is unattributed device time and must not be renamed barrier cost;
- hotspot localization answers **where**, not **why**;
- `localization ≠ mechanism`.

## 5. Terminology QA

Reader-facing terminology follows the frozen plan:

- `phép tính logic (logical/model-level operation)`;
- `chương trình GPU (kernel)`;
- `lần giao việc (dispatch)`;
- `quy chiếu (attribution)`;
- `hình học thực thi (execution geometry)`;
- `phân rã vật lý (physical decomposition)`;
- `gộp phép tính (fusion)`;
- `dấu vết thực thi (execution trace)`;
- `điểm nóng (hotspot)`;
- `định vị chi phí (localization)`.

The deprecated reader-facing phrase `semantic operation` was not introduced in revised Part III prose.

## 6. Figure QA

Materialized markers:

- F13 — model operation ↔ dispatch attribution;
- F14 — one logical → many physical;
- F15 — many logical → one physical;
- F16 — compute + data movement / memory hierarchy;
- F17 — execution trace timeline;
- F18 — measured coverage + unattributed time;
- F19 — hotspot ranked distribution.

## 7. Structural QA

- Every Chapter 11–16 file retains exactly one `Nhớ 3 điều` section.
- Every Chapter 11–16 file retains `Tiếp theo` navigation.
- All Markdown `~~~` fences are balanced.
- Chapter 17 remains byte-for-byte unchanged at blob SHA `ce2a1300643137e8cdfb3e142b321b3929390480`.
- No Part IV prose was edited.
- `main` remained unchanged at `e7404e26bd0d58301f9999d802e6dc05469e4354`.

## 8. Adjudication

`TOKEN_VI_READER_EDITION_PART_III_PROSE_REVISION` is **PASS**.

The next valid editorial step is now open:

`TOKEN_VI_READER_EDITION_PART_IV_PROSE_REVISION`

Scope:

- Chương 17–20 only;
- materialize F20–F24;
- attach `[E-COUNTER-01]` and `[E-MECH-01]` measured results;
- preserve the `counter = 0 ≠ physical quantity = 0` boundary;
- preserve the Ch.19 official **UNRESOLVED / no confirmed mechanism** verdict;
- preserve observation → pattern → hypothesis → adequacy → intervention → claim;
- keep Ch.20 as the epistemic close of this book and do not add speculative STATE/RRE/SIX content;
- run claim/evidence/terminology QA before opening Evidence-Provenance completion.