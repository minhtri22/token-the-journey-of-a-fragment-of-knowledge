# TOKEN_VI_READER_EDITION_PART_II_QA

**Scope:** Chương 5–10  
**Branch:** `reader-edition-v1`  
**Research baseline:** `c57d2488cf9bd198d562e0410163c2e8cd7fd6c5`  
**Part I adjudication:** `48976e924466e1af31b80e20db09164090284e3c`  
**Part II final revision head before this QA record:** `1f290114d0aa6a108915c4cadfffd4ffab5ed111`

## Verdict

```text
CLAIM PRESERVATION
PASS

EVIDENCE CLASSIFICATION
PASS

TERMINOLOGY / PREREQUISITE
PASS

PACING / REPETITION
PASS

FIGURE F06–F12 MATERIALIZATION
PASS

GQA / MQA SIDEBAR
PASS

CHAPTER 8 SIDEBAR SCOPE
PASS

PART II → PART III BOUNDARY
PASS

MAIN-BRANCH ISOLATION
PASS
```

## 1. Revision footprint

Only Reader Edition Chapters 5–10 were changed in this gate.

| Chapter | Reader / baseline |
|---|---:|
| Ch.5 | 0.892 |
| Ch.6 | 0.908 |
| Ch.7 | 0.866 |
| Ch.8 | 0.758 |
| Ch.9 | 0.932 |
| Ch.10 | 0.960 |
| **Part II total** | **0.876** |

Part II is approximately **12.4% shorter** than the frozen Research Edition baseline, within the book-level 10–15% tightening target.

Chapter 8 is intentionally reduced more strongly because the long cache/online-learning digression was moved into a bounded sidebar.

## 2. Claim-preservation QA

The following claims remain intact and were not strengthened:

- a Transformer layer contains multiple interacting steps rather than one monolithic operation;
- attention is a mechanism for mixing information across positions, not a complete explanation of model reasoning;
- Q/K/V are distinct roles in attention;
- causal masking controls access to future sequence positions and is not a claim of real-world causality;
- MQA/GQA change the organization/count of query versus key/value heads but do not invalidate the basic Q/K/V mental model;
- FFN transforms per-position state and should not be described as an independent cabinet of stored knowledge;
- residual connections provide a direct path for prior state but do not guarantee that all old information is preserved unchanged;
- residual-stream updates are path-dependent;
- representation trajectory refers to the sequence of states through depth, not a token physically changing identity;
- token generation uses the full contextual state, not a one-token-to-one-token transformation;
- logits are scores, not probabilities;
- the logical path drawn at model level is not identical to physical execution on hardware.

## 3. Evidence QA

Part II contains **no real measured-result claim** requiring an Evidence Note.

Invented numbers used for attention scores, softmax weights, FFN dimensions and logits are classified as **MINH HỌA — Illustration**.

No `KẾT QUẢ ĐO — Measured Result` callout was introduced.

## 4. Terminology / prerequisite QA

Verified:

- normalization / RMSNorm introduced in Ch.5;
- Q/K/V, attention score, softmax, causal masking and attention heads introduced in Ch.6;
- GQA/MQA are isolated in a sidebar and `KV cache` is introduced as **bộ nhớ đệm khóa–giá trị (KV cache)**;
- FFN nonlinearity and gating introduced in Ch.7;
- residual connection / residual stream introduced in Ch.8;
- cache versus model update is explicitly separated;
- successor-research names are absent;
- `semantic operation`, `dispatch` and hardware-counter terminology are not introduced before Part III/IV.

## 5. Chapter 8 sidebar QA

Original long section: **3,448 chars**.  
Reader Edition sidebar: **1,229 chars**.  
Ratio: **0.356**.

This satisfies the frozen revision-plan target of roughly **35–45%** of the original section.

The retained material is limited to:

- model weights ≠ run-time state;
- path dependence (`x1 → x2 → x3`);
- why updates cannot be treated as fixed `a+b+c` pieces;
- cache reuse versus model update;
- brief KV-cache/prefix-cache intuition.

The survey-like discussion of fast weights / online learning / test-time adaptation was removed from the main reader flow.

## 6. Figure QA

Materialized markers:

- F06 — canonical Transformer layer;
- F07 — Q/K/V attention flow;
- F08 — FFN transform;
- F09 — residual-stream update;
- F10 — representation trajectory **hero figure**;
- F11 — token-generation loop;
- F12 — model/logical path versus physical execution boundary.

All required F06–F12 markers are present.

## 7. Representation trajectory QA

Chapter 9 now makes the representation trajectory the central framing:

```text
TOKEN ID giữ nguyên
       ↓
x0 → x1 → x2 → ... → xN
     state changes
```

The text preserves the boundary that state change does not itself establish the semantic meaning or causal role of that change.

## 8. Part boundary QA

Chapter 10 ends Part II explicitly and preserves the central transition:

> **Đường đi logic của token chưa phải đường đi vật lý của token.**

F12 materializes the transition from model-level boxes to concrete execution work.

Chapter 11 remains byte-for-byte unchanged from the frozen Research baseline at blob SHA:

`13ac704643840405448f0f0c1b388b6996d4361d`

Therefore Part III was not edited early.

## 9. Structural QA

- Every Chapter 5–10 file retains exactly one `Nhớ 3 điều` section.
- Every Chapter 5–10 file retains `Tiếp theo` navigation.
- All Markdown `~~~` fences are balanced.
- No Part III prose was changed.
- `main` remained unchanged at `e7404e26bd0d58301f9999d802e6dc05469e4354`.

## 10. Adjudication

`TOKEN_VI_READER_EDITION_PART_II_PROSE_REVISION` is **PASS**.

The next valid editorial step is now open:

`TOKEN_VI_READER_EDITION_PART_III_PROSE_REVISION`

Scope:

- Chương 11–16 only;
- materialize F13–F19;
- preserve the distinction between logical/model-level identity and physical execution;
- classify all real measured values with Evidence Note IDs;
- preserve 469/451, LM-head 19-dispatch, device-span accounting and hotspot values exactly;
- preserve localization ≠ mechanism;
- run claim/evidence/terminology QA before opening Part IV.
