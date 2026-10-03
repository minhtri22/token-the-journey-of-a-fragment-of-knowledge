# Chương 14 — Bộ nhớ cũng nằm trên đường đi của token

> **Mức đọc: Đi sâu**
>
> **Ta đã nói nhiều về tính toán:**
>
> ~~~text
> phép tính logic
> ↓
> kernel
> ↓
> dispatch
> ↓
> GPU tính
> ~~~
>
> Nhưng còn thiếu một nửa rất lớn:
>
> ~~~text
> dữ liệu phải có mặt
> đúng nơi
> đúng lúc
> ~~~

Hãy tưởng tượng một đầu bếp có thể thái rau cực nhanh.

Nhưng nguyên liệu đang ở kho cách bếp 100 mét.

Mỗi lần cần cà rốt, đầu bếp phải đứng chờ người mang cà rốt tới.

Khi đó tốc độ con dao không còn là toàn bộ câu chuyện.

GPU cũng vậy.

## Tính toán cần dữ liệu

Một phép nhân ma trận cần trọng số, trạng thái đầu vào, nơi ghi kết quả và đôi khi thêm dữ liệu phụ.

> **[FIGURE F16] — Compute + data movement trên đường đi của token**
>
> ~~~text
> trọng số ───────┐
>                 │
> trạng thái ─────┼→ phép tính → kết quả
>                 │
> dữ liệu phụ ────┘
> ~~~

Nếu dữ liệu chưa tới nơi cần thiết, đơn vị tính toán có thể phải chờ.
## Bộ nhớ không chỉ có một tầng

Ở mức trực giác, dữ liệu có thể đi qua nhiều tầng lưu trữ:

~~~text
ổ lưu trữ
   ↓
RAM hệ thống
   ↓
vùng bộ nhớ GPU truy cập được
   ↓
cache
   ↓
thanh ghi / vùng gần đơn vị tính toán
~~~

Không phải máy nào cũng có ranh giới vật lý giống hệt sơ đồ này; GPU tích hợp có thể chia sẻ bộ nhớ vật lý với CPU.

Điều cần giữ là: **vị trí dữ liệu và khả năng tái sử dụng ảnh hưởng tới chi phí đưa dữ liệu tới nơi tính toán.**
## Trọng số là phần lớn của đường đi

Khi sinh token, mô hình phải dùng rất nhiều trọng số đã học. Chúng phải được đọc từ nơi lưu trữ phù hợp trước khi phép tính sử dụng được.

Đó là lý do người ta quan tâm tới băng thông bộ nhớ, cache, cách đóng gói trọng số, lượng tử hóa và cách chia công việc.

Nhưng việc trọng số lớn **không tự động** chứng minh một kernel đang bị giới hạn bởi bộ nhớ.
## Dữ liệu trung gian cũng phải đi đâu đó

Attention và FFN tạo ra các trạng thái trung gian mà bước sau cần dùng.

~~~text
phép tính A
   ↓
dữ liệu trung gian
   ↓
phép tính B
~~~

Nếu A và B tách biệt, dữ liệu có thể phải được ghi rồi đọc lại. Nếu execution path gộp tốt, một phần vòng đi-về này có thể giảm. Đây là cầu nối từ fusion sang memory.
## Bộ nhớ đệm là gì?

**Bộ nhớ đệm (cache)** là vùng nhỏ hơn, nhanh hơn, cố gắng giữ dữ liệu có khả năng sớm được dùng lại.

~~~text
bộ nhớ lớn
    ↓
cache
    ↓
đơn vị tính toán
~~~

Nếu dữ liệu cần dùng đã có trong cache, hệ thống có thể tránh một lần lấy từ tầng xa hơn.
## Tỷ lệ trúng cache

Khi dữ liệu cần dùng đã có trong cache, ta gọi đó là **lần trúng bộ nhớ đệm (cache hit)**.

Tỷ lệ hit cao có thể là một tín hiệu về tái sử dụng dữ liệu. Nhưng một tỷ lệ hit thấp không tự động chứng minh:

> “Kernel này chậm vì bộ nhớ.”

Muốn gắn cơ chế, ta cần thêm bằng chứng phù hợp.
## Bị giới hạn bởi tính toán hay bởi bộ nhớ?

Hai nhãn thường gặp là **bị giới hạn bởi tính toán (compute-bound)** và **bị giới hạn bởi bộ nhớ (memory-bound)**.

~~~text
compute-bound → tính toán là giới hạn chính
memory-bound  → cung cấp / di chuyển dữ liệu là giới hạn chính
~~~

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> Không nên gắn các nhãn này cho một phép đo thật chỉ từ một counter hoặc một pattern đơn lẻ.
## Một khuôn mẫu rất hấp dẫn

> **KẾT QUẢ ĐO — Measured Result `[E-MECH-01]`**
>
> Trong một phép đo phần cứng, một nhóm phép tính cho thấy pattern theo hướng:
>
> - DRAM read amplification cao hơn nhóm so sánh;
> - cache hit thấp hơn;
> - mức chờ liên quan phụ thuộc dữ liệu cao hơn;
> - mức sử dụng một số đơn vị tính toán thấp hơn.

Pattern đó rất phù hợp với giả thuyết “vấn đề bộ nhớ”. Nhưng kết luận khoa học cuối cùng **không xác nhận cơ chế đó**, vì một số kênh đo quan trọng chưa đủ đáng tin để phân biệt cơ chế.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> **Pattern phù hợp với giả thuyết bộ nhớ ≠ bằng chứng nhân quả rằng bộ nhớ là nguyên nhân.**

Chương 17 và 19 sẽ mở đầy đủ câu chuyện measurement adequacy này.
## Đường đi của token bây giờ có thêm một lớp

Nửa đầu sách đi theo token → representation → attention/FFN → logits. Ở tầng vật lý, ta thêm:

~~~text
phép tính logic
↓
dispatch
↓
đọc trọng số / trạng thái
↓
tính toán
↓
ghi kết quả
↓
phép tính tiếp theo
~~~

Token không phải hạt vật chất chạy qua dây dẫn. Nhưng **công việc phục vụ trạng thái của token** tạo ra một chuỗi hoạt động vật lý thật trên máy.
## Làm sao nhìn thấy chuỗi đó?

Ta cần ghi lại:

- công việc nào đã chạy;
- khi nào bắt đầu;
- khi nào kết thúc;
- nó thuộc phép tính logic nào;
- có điểm đồng bộ nào giữa chúng.

Một bản ghi như vậy được gọi là **dấu vết thực thi (execution trace)**.

Đó là Chương 15.

### Nhớ 3 điều

1. **Tốc độ tính toán không phải toàn bộ câu chuyện; trọng số và dữ liệu trung gian phải được đưa tới nơi tính toán đúng lúc.**
2. **Bộ nhớ đệm (cache) có thể giảm chi phí lấy lại dữ liệu, nhưng một chỉ số cache đơn lẻ chưa đủ để kết luận cơ chế gây chậm.**
3. **Đường đi vật lý của một bước sinh token gồm cả tính toán lẫn di chuyển dữ liệu; muốn quan sát nó, ta cần một dấu vết thực thi.**

**Tiếp theo: [Chương 15 — Dấu vết thực thi: những dấu chân một token để lại](15-dau-vet-thuc-thi.md)**
