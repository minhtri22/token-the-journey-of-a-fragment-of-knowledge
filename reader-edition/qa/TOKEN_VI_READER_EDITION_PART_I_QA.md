# TOKEN_VI_READER_EDITION_PART_I_QA

**Scope:** Lời mở đầu + Chương 1–4  
**Branch:** `reader-edition-v1`  
**Research baseline:** `c57d2488cf9bd198d562e0410163c2e8cd7fd6c5`  
**Reader preflight:** `8f0afee94e2e9f74cdf7bbdbf14ffe121d724cc7`  
**Part I final revision head before this QA record:** `3c5e2749ae200608f315d946c562742a1417c3ae`

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

FIGURE / CALLOUT CONVENTIONS
PASS

NAVIGATION / MARKDOWN STRUCTURE
PASS

MAIN-BRANCH ISOLATION
PASS
```

## 1. Revision footprint

Only the following Reader Edition chapter files were revised:

- `00-loi-mo-dau.md`
- `01-token-khong-phai-la-mot-tu.md`
- `02-token-khong-phai-la-tri-thuc.md`
- `03-tu-ma-token-toi-bieu-dien-bang-so.md`
- `04-cung-token-ngu-canh-khac.md`

The public Research Edition under `vi/` was not modified.

Character-count ratio against the frozen working baseline:

| File | Reader / baseline |
|---|---:|
| Lời mở đầu | 0.905 |
| Chương 1 | 0.922 |
| Chương 2 | 0.920 |
| Chương 3 | 0.900 |
| Chương 4 | 0.933 |
| **Part I total** | **0.915** |

Part I prose is therefore ~**8.5% shorter**. The full-book 10–15% tightening target remains a book-level target, not a quota that justifies deleting foundational explanation.

## 2. Claim-preservation QA

The following scientific boundaries remain intact:

- token is not necessarily a word;
- token ID is identity, not a complete package of knowledge;
- embedding creates an initial numerical representation from token identity;
- vector proximity does not itself establish semantic equivalence;
- the same token can lead to different internal states in different contexts;
- observing contextual state differences does not justify the claim that a model “understands exactly like a human”;
- the title “mảnh tri thức” remains a question, not an assertion that one token equals one unit of knowledge;
- measurement is explicitly separated from reality/interpretation in the opening.

No revised sentence was found to strengthen a causal, mechanistic or localization claim beyond the Research Edition.

## 3. Evidence QA

Part I contains no real experiment result that requires an Evidence Note.

Illustrative numeric examples remain explicitly classified as **MINH HỌA — Illustration**. In particular, example token IDs and example vectors are not presented as measured facts.

The opening introduces the future **KẾT QUẢ ĐO — Measured Result** convention, but does not itself introduce a measured result.

## 4. Terminology and prerequisite QA

The revision removed terminology that had appeared before its teaching point during the first edit pass.

Verified absent before its intended introduction:

- `quỹ đạo` as a representation-trajectory concept;
- `kênh đo`;
- `tokenization` as an undeclared reader-facing term;
- `semantic operation`;
- `dispatch`;
- `hardware counter`;
- successor-research names such as RRE or SIX.

Chapter 4 remains the first explicit introduction of:

- **ngữ cảnh (context)**;
- **trạng thái ẩn (hidden state)**;
- **biểu diễn theo ngữ cảnh (contextual representation)**.

## 5. Reader Edition conventions QA

Materialized figure markers:

- F01 — whole-book map;
- F02 — text → tokenizer → token pieces → token IDs;
- F03 — token ID ≠ knowledge/content;
- F04 — embedding lookup;
- F05 — same token, two contexts, different states.

The source now uses the Reader Edition callout vocabulary where relevant:

- **MINH HỌA — Illustration**
- **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**

No fake measured-result callout was introduced.

## 6. Structural QA

- All five files retain their **Tiếp theo** navigation.
- Chapters 1–4 retain exactly one **Nhớ 3 điều** section each.
- All `~~~` fenced blocks are balanced.
- No Part II chapter prose was edited.
- `main` remained unchanged at `e7404e26bd0d58301f9999d802e6dc05469e4354` throughout this work.

## 7. Adjudication

`TOKEN_VI_READER_EDITION_PART_I_PROSE_REVISION` is **PASS**.

The next valid editorial step is now open:

`TOKEN_VI_READER_EDITION_PART_II_PROSE_REVISION`

Scope:

- Chương 5–10 only;
- preserve the canonical Transformer-layer progression;
- move GQA/MQA into a sidebar;
- shorten the Chapter 8 cache/online-learning digression;
- materialize F06–F12;
- preserve the Chapter 10 boundary: **logical path ≠ physical path**;
- run the same claim/evidence/terminology QA before opening Part III.
