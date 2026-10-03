# TOKEN_COMMERCIAL_EDITORIAL_REVISION_PLAN_V1

**Scope:** Vietnamese Reader Edition planning only  
**Source baseline:** canonical Vietnamese Research Edition in `vi/`  
**Predecessor gate:** `TOKEN_COMMERCIAL_EDITORIAL_AUDIT_V1`  
**Manuscript mutation in this step:** NONE  
**Status:** FROZEN REVISION PLAN  

---

## 1. Mục tiêu

Bước này khóa trước toàn bộ quyết định biên tập cần thiết để biến bản Research Edition hiện tại thành một **Vietnamese Reader Edition** có thể xuất bản thương mại, mà không thay đổi ý nghĩa khoa học, không làm đẹp một FAIL thành PASS, và không mở English Edition sớm.

Reader Edition phải giữ nguyên đường dây trung tâm:

```text
văn bản
↓
token
↓
biểu diễn / trạng thái
↓
Transformer
↓
phép tính logic
↓
thực thi vật lý
↓
dấu vết / bộ đếm
↓
độ đầy đủ của phép đo
↓
bằng chứng
↓
điều ta có quyền kết luận
```

**Không được** biên tập cuốn sách thành:

- một giáo trình Transformer tổng quát;
- một catalog các kỹ thuật interpretability;
- một sách GPU programming;
- một lịch sử phát triển Token-XRay/ArcLLM;
- hay một câu chuyện chỉ giữ các kết quả đẹp.

---

## 2. Editorial invariants — các điều bất biến

Các invariant dưới đây có quyền chặn mọi sửa đổi prose về sau.

### I1 — Scientific meaning preservation

Không được thay đổi phạm vi của bất kỳ claim khoa học nào. Một câu được làm mượt hơn không được mạnh hơn bằng chứng gốc.

### I2 — PASS / FAIL / UNRESOLVED preservation

Mọi trạng thái `PASS`, `FAIL`, `KHÔNG ĐẠT`, `CHƯA GIẢI QUYẾT`, `CHƯA ĐỦ BẰNG CHỨNG`, `ngoài phạm vi đã đo` phải giữ nguyên ý nghĩa.

### I3 — Measured value preservation

Số liệu thực nghiệm không được làm tròn theo cách thay đổi diễn giải. Bản thân body có thể dùng số làm tròn để đọc dễ hơn nếu Evidence Note lưu exact value.

### I4 — Illustration ≠ measurement

Mọi số liệu minh họa phải được nhận diện rõ là minh họa. Không được trình bày ví dụ tưởng tượng với visual treatment giống số đo thật.

### I5 — Observation ≠ mechanism

Localization, correlation, pattern, counter value hay trajectory difference không được viết thành nguyên nhân nếu lineage chưa chứng minh nhân quả.

### I6 — Research provenance remains canonical elsewhere

Repo sách không sao chép nguyên lineage nghiên cứu. Repo sách chỉ giữ index/provenance pointer tới nguồn canonical.

### I7 — Accessibility without technical erasure

Giữ nguyên chiến lược `tiếng Việt (English)` khi thuật ngữ xuất hiện lần đầu. Không đơn giản hóa tới mức làm mất danh tính kỹ thuật của khái niệm.

### I8 — No speculative STATE content

Không thêm chương về functional state, intervention, RRE, SIX hay successor research chưa hội tụ. Những câu hỏi đó chỉ được giữ như hướng mở ở cuối sách.

### I9 — Vietnamese Reader Edition precedes English adaptation

Không dịch trực tiếp Research Edition hiện tại. English Edition chỉ được mở sau khi Vietnamese Reader Edition được freeze.

### I10 — Public research edition remains readable

Biên tập thương mại không dựa vào việc xóa hoặc làm nghèo bản GitHub miễn phí để tạo paywall nhân tạo.

---

## 3. Cấu trúc thương mại được khóa

Research Edition hiện chia 5 phần trong `vi/README.md`. Reader Edition **chuyển sang 4 phần** để giữ đường kể liên tục hơn.

### FRONT MATTER

- Half title
- Title page
- Copyright / edition statement
- Lời tác giả
- Cách đọc cuốn sách
- Chú giải ba loại hộp: Minh họa / Kết quả đo / Ranh giới diễn giải
- Lời mở đầu — Đi cùng một token

### PART I — TỪ VĂN BẢN TỚI TRẠNG THÁI

1. Token không phải là một từ  
2. Token cũng không phải là tri thức  
3. Từ mã token tới một biểu diễn bằng số  
4. Cùng một token, ngữ cảnh khác, biểu diễn khác

**Reader promise:** kết thúc Part I, người đọc hiểu token chỉ là điểm vào; thứ được biến đổi trong mô hình là trạng thái số phụ thuộc ngữ cảnh.

### PART II — ĐI XUYÊN TRANSFORMER

5. Dòng tín hiệu đi xuyên Transformer  
6. Cơ chế chú ý: token nhìn những token khác như thế nào?  
7. Nhánh biến đổi tín hiệu: điều gì xảy ra sau cơ chế chú ý?  
8. Đường cộng tắt: vì sao thông tin cũ không biến mất hoàn toàn?  
9. Một lớp chưa biết cả câu — vậy nhiều lớp làm được gì?  
10. Từ trạng thái cuối tới token tiếp theo

**Reader promise:** kết thúc Part II, người đọc thấy một quỹ đạo trạng thái đi qua nhiều lớp và hiểu token tiếp theo là kết quả của toàn ngữ cảnh, không phải một token cũ biến thành token mới.

### PART III — TỪ PHÉP TÍNH MÔ HÌNH TỚI THỰC THI VẬT LÝ

11. Một phép tính logic không phải một chương trình GPU  
12. Một phép tính có thể trở thành nhiều lần giao việc cho phần cứng  
13. Khi nhiều phép tính được gộp thành một công việc  
14. Bộ nhớ cũng nằm trên đường đi của token  
15. Dấu vết thực thi: những dấu chân một token để lại  
16. Thời gian nằm ở đâu?

**Reader promise:** kết thúc Part III, người đọc phân biệt được model-level identity với physical execution, và biết trace có thể định vị chi phí nhưng chưa giải thích nguyên nhân.

### PART IV — TỪ PHÉP ĐO TỚI ĐIỀU TA THẬT SỰ BIẾT

17. Bộ đếm phần cứng: con số đo được có nghĩa gì?  
18. Khi một khác biệt quá nhỏ vẫn đáng để hỏi  
19. Một khuôn mẫu đẹp vẫn chưa phải nguyên nhân  
20. Từ phép đo tới điều ta thật sự biết

**Reader promise:** kết thúc Part IV, người đọc phân biệt hiện tượng, phép đo, bằng chứng, diễn giải và kết luận; `CHƯA GIẢI QUYẾT` được hiểu là một trạng thái tri thức hợp lệ.

### BACK MATTER

- Evidence Notes
- Glossary / Thuật ngữ
- Research & provenance note
- Acknowledgements
- About the author
- Canonical repository / updates
- Index cho bản in

---

## 4. Chapter action matrix — KEEP / TIGHTEN / MOVE / ADD

Không chương nào bị xóa. Mỗi chương giữ câu hỏi khoa học trung tâm.

### Lời mở đầu

**KEEP:** ví dụ Hà Nội; câu hỏi “mảnh tri thức”; hai đường chạy song song; nguyên tắc measurement ≠ reality.  
**TIGHTEN:** rút các cảnh báo lặp lại sẽ được dạy đầy đủ ở Ch.17–19.  
**ADD:** một box rất ngắn giải thích ba loại nội dung của Reader Edition.  
**TARGET:** giảm khoảng 10–15% chữ.

### Chương 1

**KEEP:** token ≠ word; tokenizer; token ID.  
**TIGHTEN:** một số đoạn nhắc lại rằng token chưa phải ý nghĩa.  
**ADD:** hình `text → tokenizer → token pieces → IDs`.  
**TARGET:** đọc nhanh, tạo tự tin.

### Chương 2

**KEEP:** ID là identity; Paris example; knowledge not localized to token.  
**TIGHTEN:** các cảnh báo lặp với Ch.1/3.  
**ADD:** visual `identity ≠ content`.

### Chương 3

**KEEP:** embedding lookup; vector/tensor; learned parameters; initial representation.  
**TIGHTEN:** ví dụ người `[45,170,...]` nếu visual đã đủ.  
**ADD:** canonical embedding lookup figure.

### Chương 4

**KEEP:** `đá` contextual example; hidden state; contextual representation; position caveat.  
**TIGHTEN:** đoạn analogy `Mai` nếu trùng chức năng với ví dụ chính.  
**ADD:** same-token / two-context split trajectory figure.

### Chương 5

**KEEP:** map of one Transformer layer; normalization/attention/FFN/residual roles.  
**TIGHTEN:** không mở rộng thêm architecture variants.  
**ADD:** một canonical Transformer-layer figure, tái sử dụng ở Ch.6–8.

### Chương 6

**KEEP:** Q/K/V library analogy; causal mask; attention ≠ explanation.  
**MOVE:** GQA/MQA thành **sidebar** rõ ràng, không để nó ngắt main narrative.  
**ADD:** annotated reuse of canonical layer figure.

### Chương 7

**KEEP:** FFN as per-position transformation; expansion/nonlinearity; caution about “knowledge storage”.  
**TIGHTEN:** không thêm survey SwiGLU/GeGLU/MoE.  
**ADD:** FFN flow figure.

### Chương 8

**KEEP:** `x + F(x)`; residual stream; path dependence.  
**MOVE:** đoạn dài “ghi phần đã học lại vào tệp mô hình / cache / online learning” sang boxed sidebar **`Có thể lưu lại trạng thái để đi tắt không?`**; sidebar rút còn khoảng 35–45% độ dài hiện tại.  
**CUT/TIGHTEN:** danh sách các hướng external memory / fast weights / test-time adaptation chỉ giữ nếu phục vụ distinction `cache ≠ update model`.  
**ADD:** residual-stream accumulation figure.

### Chương 9

**KEEP:** composition; depth; rejection of rigid layer labels; representation trajectory.  
**ADD:** **hero figure của cuốn sách** — một token identity cố định với state trajectory `x0 → x1 → … → xN`.  
**TIGHTEN:** không đưa research successor vào đây.

### Chương 10

**KEEP:** logits; LM head; decoding; whole-context prediction; conclusion “logical path ≠ physical path”.  
**ADD:** full loop figure `context → state → logits → token → append → repeat`.  
**LAYOUT:** kết chương bằng một **Part break** rõ ràng trước Ch.11.

### Chương 11

**KEEP:** logical/model-level operation vs kernel vs dispatch; 469/451 measured example; attribution.  
**TIGHTEN:** tránh lặp nhiều lần cùng định nghĩa kernel/dispatch.  
**ADD:** two-layer map `model graph ↔ physical dispatch timeline`.  
**EVIDENCE:** `[E-TRACE-01]`.

### Chương 12

**KEEP:** one logical operation → many dispatches; 19-dispatch LM-head example; geometry caveat.  
**ADD:** decomposition visual `1 → N`.  
**EVIDENCE:** `[E-DECOMP-01]`.

### Chương 13

**KEEP:** fusion concept; explicit statement that current trace does not demonstrate fusion.  
**TIGHTEN:** giữ fusion ở mức cần thiết, không mở thành kernel optimization chapter.  
**ADD:** mirror visual `N → 1`, đặt đối xứng với Ch.12.

### Chương 14

**KEEP:** memory hierarchy intuition; weights/intermediates/cache; compute-bound vs memory-bound caution.  
**TIGHTEN:** tránh mở rộng thành hardware architecture survey.  
**ADD:** data-movement path figure.

### Chương 15

**KEEP:** execution trace; timestamps; barriers; attribution; exact 0.078% unattributed-device-time lesson.  
**TIGHTEN:** các định nghĩa đã có ở Ch.11–12 chỉ nhắc ngắn.  
**ADD:** clean measured timeline visual.  
**EVIDENCE:** `[E-TRACE-01]`, `[E-TRACE-02]`.

### Chương 16

**KEEP:** hotspot percentages; localization; Amdahl intuition; hotspot expiry after optimization.  
**TIGHTEN:** Amdahl không thêm công thức nặng.  
**ADD:** hotspot bar chart / ranked time map.  
**EVIDENCE:** `[E-HOTSPOT-01]`.

### Chương 17

**KEEP:** measurement channel; 268 counters / 12 passes; counter zero caveat; instrumentation changes system; adequacy.  
**ADD:** highlighted principle `counter = 0 ≠ physical quantity = 0`; measurement-channel diagram.  
**EVIDENCE:** `[E-COUNTER-01]`.

### Chương 18

**KEEP:** difference/residual; resolution; noise floor; repeatability; structured small difference.  
**TIGHTEN:** giảm số ví dụ số học nếu cùng dạy một ý.  
**ADD:** `noise → repeatability → structure → question` figure.  
**BOUNDARY:** không nhắc tên RRE/SIX hay claim successor.

### Chương 19

**KEEP:** DRAM/LSC/SBID/ALU directional pattern; official unresolved verdict; correlation/confounder/intervention.  
**ADD:** canonical evidence ladder `Observation → Pattern → Hypothesis → Adequacy → Intervention → Claim`.  
**EVIDENCE:** `[E-MECH-01]`.  
**INVARIANT:** unresolved verdict không được làm mềm.

### Chương 20

**KEEP:** two journeys; distributed technical view of knowledge; unknown as valid epistemic state; final principle.  
**TIGHTEN:** tránh mở thêm một mini-book về state/intervention.  
**ADD:** final two-column synthesis figure.  
**ENDING:** giữ cuốn sách kết thúc ở ranh giới bằng chứng hiện tại.

---

## 5. Target về độ dài và nhịp đọc

- Mục tiêu tổng thể: giảm khoảng **10–15% prose** so với Research Edition, chủ yếu từ lặp ý, không phải từ bỏ khái niệm.
- Không đặt quota cứng cho từng chương; chapter coherence quan trọng hơn số từ.
- Part I phải nhanh và tạo cảm giác tiến bộ rõ.
- Part II là phần giải thích model, tránh sa vào công thức.
- Part III tăng độ kỹ thuật nhưng luôn giữ cầu nối với token/state.
- Part IV chậm lại có chủ đích: mỗi bước tăng yêu cầu về bằng chứng.
- `Nhớ 3 điều` vẫn được giữ, nhưng phải tránh lặp nguyên văn phần thân chương.

---

## 6. Visual system specification

### 6.1 Nguyên tắc

Visuals phải giải thích cấu trúc, không trang trí. Mỗi hình phải trả lời được: **người đọc sẽ hiểu điều gì nhanh hơn nhờ hình này?**

### 6.2 Vocabulary cố định

Reader Edition dùng một bộ ký hiệu lặp lại:

1. **Token** — mảnh chữ / label ngắn.
2. **Token ID** — integer badge.
3. **Representation / hidden state** — vector/state capsule.
4. **Context** — nhóm token bao quanh.
5. **Transformer layer** — processing block.
6. **Model/logical operation** — model-graph node.
7. **Kernel / dispatch** — physical execution block.
8. **Memory movement** — directed data arrow.
9. **Trace / timestamp** — timeline mark.
10. **Measurement channel** — instrument/filter path.
11. **Evidence** — evidence card.
12. **Unresolved boundary** — explicit stop marker.

### 6.3 Ba callout classes bắt buộc

**MINH HỌA — Illustration**  
Dùng cho số tưởng tượng, analogy, simplified mental model.

**KẾT QUẢ ĐO — Measured Result**  
Dùng cho số lấy từ experiment thật; luôn kèm Evidence Note ID.

**RANH GIỚI DIỄN GIẢI — Interpretation Boundary**  
Dùng cho câu `kết quả này cho phép nói... / chưa cho phép nói...`.

Ba class phải khác nhau rõ ràng cả ở bản màu lẫn grayscale/e-ink; không phụ thuộc duy nhất vào màu sắc.

### 6.4 Figure inventory — freeze 24 hình mục tiêu

1. Book map: token journey + observer journey.
2. Text → tokenizer → token pieces.
3. Token ID ≠ knowledge/content.
4. Embedding lookup.
5. Same token, two contexts, two states.
6. Canonical Transformer layer.
7. Attention Q/K/V flow.
8. FFN transformation flow.
9. Residual stream update.
10. Representation trajectory `x0…xN` — hero figure.
11. Logits / decoding loop.
12. Logical path vs physical path — Part transition.
13. Model operation ↔ dispatch attribution.
14. One logical → many physical.
15. Many logical → one physical (fusion concept).
16. Compute + data movement / memory hierarchy.
17. Execution trace timeline.
18. Measured coverage + unattributed time.
19. Hotspot ranked distribution.
20. Measurement channel.
21. Counter-zero interpretation boundary.
22. Small difference vs noise / repeatability.
23. Evidence ladder from observation to claim.
24. Final two-journey synthesis.

Không tăng quá 30 hình nếu chưa có lý do rõ ràng.

---

## 7. Evidence Note schema — frozen

Evidence Notes sống trong back matter và `evidence/` index. Body chỉ hiển thị ID ngắn.

Mỗi Evidence Note phải có các field:

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

### 7.1 Evidence IDs khóa cho Reader Edition v1

#### `[E-TRACE-01]` — Dispatch / logical-operation coverage

Phục vụ các claim: 469 physical dispatches, 451 measured logical operations, all target dispatches timestamped/attributed trong phạm vi trace.  
Used in: Ch.11, Ch.15.

#### `[E-DECOMP-01]` — LM-head physical decomposition

Phục vụ claim: một LM-head logical operation được materialize thành 19 physical dispatches trong trace mục tiêu.  
Used in: Ch.12.

#### `[E-TRACE-02]` — Device-span accounting

Exact values cần giữ: device span `1,240,766,914 ns`; summed dispatch duration `1,239,803,214 ns`; difference `963,700 ns`, khoảng `0.078%`.  
Allowed claim: unattributed device time trong trace.  
Forbidden claim: gọi toàn bộ 963,700 ns là barrier cost.  
Used in: Ch.15.

#### `[E-HOTSPOT-01]` — Dispatch-time localization

Exact values: LM head `63.9109%`; FFN-down `16.2659%`; FFN-up `6.0136%`; FFN-gate `5.9812%`; top four `92.1715%`.  
Allowed claim: localization trong trace mục tiêu.  
Forbidden claim: universal LLM profile hoặc causal mechanism.  
Used in: Ch.16.

#### `[E-COUNTER-01]` — Hardware-counter inventory and adequacy failure

Includes: 268 exposed counters, 12 passes for inventory, `GpuTime = 0` example, cross-group inconsistencies where applicable.  
Allowed claim: measurement channel was insufficient for selected causal question.  
Forbidden claim: physical GPU time was zero.  
Used in: Ch.17.

#### `[E-MECH-01]` — Directional mechanism pattern with unresolved verdict

Includes the measured DRAM amplification / LSC hit / SBID stall / ALU utilization contrasts used in Ch.19.  
Allowed claim: directional pattern motivating hypothesis.  
Official conclusion to preserve: no confirmed mechanism / unresolved due to inadequate measurement channel.  
Used in: Ch.14, Ch.19 where appropriate.

### 7.2 Provenance gate

Reader Edition prose editing may begin before every canonical source pointer is filled, nhưng **commercial release cannot PASS** until every Evidence Note has a canonical source/freeze/QA pointer or is explicitly downgraded to an illustrative example.

---

## 8. Glossary scope — frozen

Glossary chỉ chứa thuật ngữ thật sự dùng trong sách; không biến thành AI encyclopedia.

### Token / model

- token
- tokenizer
- token ID
- vocabulary
- embedding
- representation
- vector
- tensor
- parameter
- weight
- context
- hidden state
- contextual representation
- position
- Transformer
- decoder-only model
- layer
- normalization / RMSNorm
- attention
- query / key / value
- attention score
- softmax
- attention head
- causal masking
- MHA / MQA / GQA
- FFN
- nonlinearity
- gating
- residual connection
- residual stream
- depth
- composition
- representation trajectory
- LM head
- logit
- greedy decoding
- sampling

### Runtime / hardware

- model-level / logical operation
- kernel
- dispatch
- attribution
- execution geometry
- physical decomposition
- fusion
- memory hierarchy
- DRAM
- cache
- cache hit
- compute-bound
- memory-bound
- synchronization
- barrier
- execution trace
- timestamp
- hotspot
- localization
- Amdahl's law

### Measurement / evidence

- hardware counter
- measurement channel
- instrumentation
- measurement adequacy
- difference / delta
- residual difference
- resolution
- noise floor
- repeatability
- correlation
- confounder
- directional evidence
- intervention
- causal claim
- unresolved / insufficient evidence

Glossary definition phải ngắn, nhất quán với lần định nghĩa đầu tiên trong body.

---

## 9. Terminology decisions

### Vietnamese Reader Edition

- Dùng `phép tính logic` làm thuật ngữ chính trong prose cho model-level operation.
- Có thể ghi `(logical/model-level operation)` ở glossary/back matter nếu cần làm cầu sang English Edition.
- `semantic operation` chỉ xuất hiện khi phải giữ identity của nguồn nghiên cứu; không dùng như thuật ngữ độc giả chính vì dễ nhầm với “ngữ nghĩa”.
- Giữ `kernel` trong ngoặc sau `chương trình GPU` ở lần đầu.
- Giữ `dispatch` trong ngoặc sau `lần giao việc` ở lần đầu.
- Giữ `representation` sau `cách biểu diễn/biểu diễn` tùy ngữ cảnh, không đổi thành một từ Việt khác giữa các chương.
- `hidden state` = `trạng thái ẩn`; `state` nói chung = `trạng thái`.

### English adaptation later

Preferred reader-facing phrase: **logical operation** or **model-level operation**. Final choice được khóa trong English adaptation gate, không ở bước này.

---

## 10. Front matter — frozen scope

### 10.1 Title page

`TOKEN — Đường đi của mảnh tri thức`  
`Nguyễn Minh Trí`

### 10.2 Copyright page

- © 2026 Nguyễn Minh Trí
- edition/version
- All Rights Reserved
- canonical repository
- statement that free GitHub Research Edition and commercial Reader Edition may differ in formatting/editorial presentation.

### 10.3 Lời tác giả

Ngắn, tập trung vào lý do chọn một token làm điểm nhìn và lý do cuốn sách giữ cả những kết quả `CHƯA GIẢI QUYẾT`.

### 10.4 Cách đọc cuốn sách

Giải thích:

- ba cấp độ `Nền tảng / Đi sâu / Nghiên cứu–Nâng cao`;
- người đọc không cần biết code;
- thuật ngữ Việt/Anh;
- ba callout classes;
- Evidence Note IDs;
- có thể bỏ qua chi tiết số liệu mà vẫn theo được narrative chính.

---

## 11. Back matter — frozen scope

### 11.1 Evidence Notes

Theo schema Section 7.

### 11.2 Glossary

Theo scope Section 8.

### 11.3 Research & provenance note

Giải thích ngắn:

- measured examples đến từ nghiên cứu thật của tác giả;
- book repo không phải canonical experiment repo;
- pointers cho phép độc giả kỹ thuật đi sâu hơn;
- evidence provenance không biến thành product marketing.

### 11.4 Acknowledgements

Chỉ thêm khi author cung cấp nội dung thực tế; không tự suy diễn tên người/tổ chức.

### 11.5 About the author

Viết sau, không đưa vào revision prose pass đầu tiên.

### 11.6 Canonical repository

Link repo và version/release tag.

### 11.7 Index

Index khái niệm cho bản print/EPUB final; không làm thủ công trước khi pagination/format gần khóa.

---

## 12. Cross-link policy

Research Edition hiện dùng `Tiếp theo:` với link Markdown tương đối. Reader Edition source vẫn có thể giữ navigation, nhưng export pipeline phải hỗ trợ:

- print: chuyển thành tên chương, không hiển thị raw URL;
- EPUB/Kindle: internal hyperlink;
- GitHub: relative Markdown link;
- PDF: clickable internal destination nếu toolchain hỗ trợ.

Không để URL GitHub dài xuất hiện giữa prose chính.

---

## 13. Revision execution order — frozen

Không sửa tùy hứng theo chương ngẫu nhiên. Thứ tự:

```text
R0  Freeze this revision plan
 ↓
R1  Create Reader Edition working branch / namespace
 ↓
R2  Front matter + Part boundaries + callout conventions
 ↓
R3  Revise Part I (Opening + Ch.1–4)
 ↓
R4  Revise Part II (Ch.5–10)
 ↓
R5  Revise Part III (Ch.11–16)
 ↓
R6  Revise Part IV (Ch.17–20)
 ↓
R7  Build Evidence Notes + provenance completion
 ↓
R8  Build glossary
 ↓
R9  Full-book continuity / terminology / repetition pass
 ↓
R10 Scientific claim-preservation QA
 ↓
R11 Reader Edition freeze
 ↓
R12 English adaptation may open
```

Research Edition `vi/` phải được giữ như baseline; prose revision nên diễn ra trong một namespace/branch Reader Edition riêng thay vì overwrite lịch sử ngay từ đầu.

---

## 14. QA gates cho từng chương

Một chương chỉ PASS revision khi đạt đủ:

1. **Meaning gate:** không mạnh hóa claim.
2. **Evidence gate:** measured values có Evidence Note ID hoặc được ghi rõ là illustration.
3. **Terminology gate:** thuật ngữ nhất quán với glossary.
4. **Prerequisite gate:** không dùng khái niệm trước khi dạy.
5. **Pacing gate:** lặp ý được cắt nhưng safety boundary còn đủ.
6. **Visual gate:** hình/callout có mục đích rõ; nếu chưa có asset thì có figure placeholder ID.
7. **Navigation gate:** mở/kết chương nối logic với chương trước/sau.
8. **Three-things gate:** `Nhớ 3 điều` không mâu thuẫn và không vượt quá prose.

---

## 15. Full-book acceptance gates

Vietnamese Reader Edition chỉ được freeze khi:

- 4-Part structure materialized;
- 21 chapters retained and revised;
- no scientific claim is stronger than Research Edition/lineage;
- repetition reduction achieved without deleting epistemic safeguards;
- all measured results classified and Evidence-Note-linked;
- all six initial Evidence Notes have provenance resolved or downgraded explicitly;
- terminology pass complete;
- glossary complete;
- figure system internally consistent;
- no speculative successor research added;
- front/back matter complete except optional acknowledgements/index final pagination details;
- all internal links/export navigation validated;
- final whole-book read finds no contradiction between early intuition and later correction;
- canonical Reader Edition version/tag can be cited independently of moving `main`.

---

## 16. English Edition opening condition

`en/` remains placeholder-only until all of the following are true:

```text
Vietnamese Reader Edition
        ↓
full-book editorial QA PASS
        ↓
scientific claim-preservation QA PASS
        ↓
Evidence Notes provenance PASS
        ↓
Reader Edition freeze/tag
        ↓
ENGLISH ADAPTATION OPEN
```

English Edition must adapt rhythm and idiom, but may not change claim strength, evidence status or unresolved boundaries.

---

## 17. Decision

```text
FOUR-PART STRUCTURE
FROZEN

CHAPTER KEEP / TIGHTEN / MOVE / ADD PLAN
FROZEN

VISUAL SYSTEM
FROZEN AT SPEC LEVEL

EVIDENCE NOTE SCHEMA + INITIAL IDS
FROZEN

GLOSSARY SCOPE
FROZEN

FRONT / BACK MATTER
FROZEN

EDITORIAL INVARIANTS
FROZEN

MANUSCRIPT PROSE EDITING
NOT STARTED

ENGLISH EDITION
CLOSED
```

## 18. Bước tiếp theo hợp lệ

`TOKEN_VI_READER_EDITION_WORKING_NAMESPACE_AND_EDITORIAL_PREFLIGHT`

Bước đó phải:

- tạo branch/namespace riêng cho Reader Edition;
- materialize 4-Part skeleton và front/back-matter placeholders;
- tạo callout/figure/Evidence-Note conventions ở mức source;
- khóa baseline commit của `vi/` để claim-preservation diff có thể đối chiếu;
- không rewrite toàn bộ chương trong cùng bước preflight.

Chỉ sau preflight đó PASS mới mở prose revision Part I.
