# TOKEN_VI_READER_EDITION_FULL_BOOK_CONTINUITY_TERMINOLOGY_REPETITION_PASS

**Branch:** `reader-edition-v1`  
**Predecessor gate:** `TOKEN_VI_READER_EDITION_GLOSSARY_BUILD`  
**Predecessor adjudication:** `9bf990f23d99d66bc9f381feda6c60bde6f51d49`  
**R9 materialization commit:** `16ed4f60766e1e85a750bd3e5a91571935e7776f`  
**Scientific claim-preservation final QA:** NOT RUN IN THIS GATE  
**Reader Edition freeze:** NOT RUN IN THIS GATE

## Verdict

```text
CHAPTER-TO-CHAPTER CONTINUITY
PASS

TERMINOLOGY CONTINUITY
PASS

REPETITION / REDUNDANCY
PASS

FIGURE SEQUENCE F01–F24
PASS

NAVIGATION / MARKDOWN STRUCTURE
PASS

EXACT MEASURED VALUES
UNCHANGED

EVIDENCE NOTES
UNCHANGED

GLOSSARY REFERENCE
UNCHANGED

MAIN-BRANCH ISOLATION
PASS

FRONT/BACK MATTER PLACEHOLDERS
12 TRACKED PRE-FREEZE BLOCKERS — OUTSIDE R9 PROSE REPAIR

OVERALL R9
PASS
```

## 1. What R9 did

R9 read the Vietnamese Reader Edition as one continuous manuscript rather than as four independently revised parts.

The gate was deliberately bounded: it repaired continuity, terminology and avoidable repetition, but did **not** rerun science, alter evidence provenance, perform the final scientific claim-preservation QA, or freeze the Reader Edition.

## 2. Continuity repairs

### Part I / II

- Ch.1 now explicitly introduces **từ vựng token (vocabulary)** so the frozen glossary term has a real reader-facing teaching point.
- Ch.6 now names **Multi-Head Attention (MHA)** before contrasting MQA/GQA.
- Ch.10 removes the redundant embedded `HẾT PHẦN II` heading; Part transitions are now handled by the manuscript Part markers.

### Part III

- Ch.11 uses the frozen singular English bridge `logical/model-level operation` and avoids teaching `execution trace` before Ch.15.
- Ch.12 normalizes the one-to-many decomposition language to reader-facing Vietnamese while retaining `dispatch` as the technical bridge term.
- Ch.13 replaces premature `trace/runtime/execution path` shorthand with `phép đo / hệ thực thi / đường thực thi` and keeps fusion semantics unchanged.
- Ch.14 now explicitly introduces **phân cấp bộ nhớ (memory hierarchy)** and no longer uses `measurement adequacy` before that concept is formally taught in Ch.17.
- Ch.14 leaves the formal definition of **dấu vết thực thi (execution trace)** to Ch.15 rather than defining it twice.
- Ch.15 normalizes `trace/timeline/operation` prose after the formal bilingual introduction.
- Ch.16 consistently uses `định vị`, `điểm nóng`, `thời gian dispatch đã đo` and `bản đồ điểm nóng` in reader-facing prose.

### Part IV

- Ch.17 no longer uses `measurement adequacy` before defining it; **cơ chế đo đạc (instrumentation)** now has a formal reader-facing introduction.
- Ch.18 explicitly introduces **phần dư / khác biệt dư (residual difference)** and keeps it distinct from `residual connection`.
- Ch.19 normalizes research shorthand such as `state space`, `directional pattern`, `measurement channel`, and English evidence-ladder labels into the vocabulary already taught by the book.
- Ch.20 now closes the book with the same Vietnamese-first terminology used throughout the Reader Edition instead of reverting to a dense block of English research shorthand.

## 3. Part-marker continuity

The four `reader-edition/parts/PART_*.md` files are no longer preflight structural markers.

Each now contains:

- the frozen Part title;
- a short transition sentence;
- the frozen reader promise for that Part.

This removes the structural mismatch where only Ch.10 carried an explicit end-of-Part heading while the actual Part files still said `Preflight structural marker only`.

## 4. Glossary/body continuity

The four frozen glossary items that previously lacked an explicit reader-facing occurrence now have one in the body:

- `từ vựng token (vocabulary)` — Ch.1;
- `Multi-Head Attention (MHA)` — Ch.6;
- `phân cấp bộ nhớ (memory hierarchy)` — Ch.14;
- `phần dư / khác biệt dư (residual difference)` — Ch.18.

No glossary entry was added or deleted in R9.

`reader-edition/backmatter/02-glossary.md` remained byte-identical at blob:

`a9a2917ce88849cabad7b2da538fe44ed14b99d1`.

## 5. Repetition adjudication

R9 removed repetition only when it was structurally redundant or created a prerequisite problem.

Examples:

- duplicate Part-II end marker removed;
- `execution trace` is now foreshadowed in Ch.14 and formally defined in Ch.15 rather than defined twice;
- `measurement adequacy` is foreshadowed before Ch.17 without using the term prematurely, then formally named in Ch.17.

Intentional recurrence was **retained** when it performs epistemic escalation rather than simple repetition, especially:

- token ≠ knowledge;
- representation ≠ essence;
- localization ≠ mechanism;
- pattern/directional evidence ≠ causation;
- `counter = 0 ≠ physical quantity = 0`;
- `UNRESOLVED` as a valid scientific state.

Those repetitions are part of the book's argument and were not cut merely to reduce word count.

## 6. Scientific immutability check

The R9 commit did **not** modify `reader-edition/EVIDENCE_NOTES.md`.

Evidence Notes remain byte-identical at blob:

`ed1125cf01e14a6b9c4bf16d90dbb610006e37b5`.

Independent body checks confirmed the following exact payloads remain present:

- Ch.11: `469 / 451`;
- Ch.12: `19 dispatch`;
- Ch.15: `1,240,766,914 ns`, `1,239,803,214 ns`, `963,700 ns`, `0.078%`;
- Ch.16: `63.9109%`, `16.2659%`, `6.0136%`, `5.9812%`, `92.1715%`;
- Ch.17: `268 counters`, `12 measurement passes`, `GpuTime = 0`, and `counter = 0 ≠ physical quantity = 0`;
- Ch.19: W-S values `4.268 / 1.436`, `0.103 / 0.738`, `81.78% / 56.50%`, `~1.58% / ~13.84%`, plus **UNRESOLVED / no confirmed mechanism**.

R9 therefore changed presentation/terminology continuity, not scientific result values or verdicts.

## 7. Structural QA

Verified across Opening + Ch.1–20:

- figure markers remain complete and ordered **F01–F24**;
- Ch.1–20 each retain exactly one `Nhớ 3 điều` section;
- Ch.0–19 retain sequential `Tiếp theo` navigation;
- Ch.20 correctly has no next-chapter link;
- all checked `~~~` fenced blocks are balanced;
- no chapter retains a stray `HẾT PHẦN` heading after Part-marker materialization.

Result: **PASS**.

## 8. Reader-facing terminology cleanup

Static checks on the repaired chapters found no remaining reader-facing occurrences of the targeted drift phrases:

- `semantic operation`;
- plural `logical/model-level operations` at the canonical introduction;
- `physical dispatches` as reader prose;
- `execution path`;
- premature `measurement adequacy này`;
- `Trace thực nghiệm`;
- `runtime thay đổi`;
- `state space`;
- `lineage chưa hội tụ`;
- `measured dispatch time`;
- `hotspot map`;
- `directional pattern`;
- `causal evidence`;
- `measurement channel hiện tại`.

Result: **PASS**.

## 9. Pre-freeze placeholders discovered

R9 also inspected manuscript surroundings and confirmed that the following files are still preflight placeholders:

### Front matter — 6

- `frontmatter/00-half-title.md`
- `frontmatter/01-title-page.md`
- `frontmatter/02-copyright-edition.md`
- `frontmatter/03-loi-tac-gia.md`
- `frontmatter/04-cach-doc-cuon-sach.md`
- `frontmatter/05-callout-legend.md`

### Back matter — 6

- `backmatter/01-evidence-notes.md`
- `backmatter/03-research-provenance.md`
- `backmatter/04-acknowledgements.md`
- `backmatter/05-about-author.md`
- `backmatter/06-canonical-repository.md`
- `backmatter/07-print-index.md`

These are **not silently marked complete** by R9.

They do not invalidate chapter continuity QA because they contain no manuscript prose to contradict, but they remain explicit **pre-freeze blockers**. Acknowledgements and author biography must not be invented.

## 10. Mutation footprint

R9 materialization changed:

- 13 chapter files with bounded terminology/continuity edits;
- 4 Part marker files.

It did not modify:

- Evidence Notes;
- glossary;
- front matter;
- back-matter placeholders;
- public Research Edition on `main`.

`main` remained unchanged at:

`e7404e26bd0d58301f9999d802e6dc05469e4354`.

## 11. Adjudication

`TOKEN_VI_READER_EDITION_FULL_BOOK_CONTINUITY_TERMINOLOGY_REPETITION_PASS` is **PASS**.

This is an editorial continuity PASS only.

It is **not** the final scientific claim-preservation QA and **not** the Reader Edition freeze.

The next valid gate in the frozen R0→R12 sequence is:

`TOKEN_VI_READER_EDITION_SCIENTIFIC_CLAIM_PRESERVATION_QA`

That R10 gate must independently compare the final Reader Edition claims against the Research Edition baseline and canonical Evidence Notes/lineage. It must not treat this R9 PASS as proof that the science itself is preserved.

Reader Edition freeze remains blocked until the tracked front/back-matter placeholders required for release are resolved or explicitly adjudicated as optional/deferred.
