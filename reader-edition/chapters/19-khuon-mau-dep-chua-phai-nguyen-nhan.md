# Chương 19 — Một khuôn mẫu đẹp vẫn chưa phải nguyên nhân

> **Mức đọc: Nâng cao**
>
> **Ta đang đứng trước một cám dỗ rất lớn:**
>
> ~~~text
> hai nhóm khác nhau rất rõ
>          ↓
> "chắc đây là nguyên nhân"
> ~~~
>
> Chương này giải thích vì sao một khuôn mẫu mạnh có thể là **bằng chứng định hướng**, nhưng vẫn chưa đủ để trở thành **kết luận nhân quả**.

Giả sử ta quan sát hai nhóm phép tính.

Nhóm A có:

- đọc bộ nhớ nhiều hơn;
- tỷ lệ trúng bộ nhớ đệm thấp hơn;
- thời gian chờ cao hơn;
- mức sử dụng một số đơn vị tính toán thấp hơn.

Nhóm B thì ngược lại.

Một câu chuyện rất tự nhiên xuất hiện:

> “A chậm vì đang bị nghẽn bộ nhớ.”

Có thể đúng.

Nhưng “có thể đúng” khác “đã chứng minh”.

## Tương quan là gì?

Khi hai đại lượng thay đổi cùng nhau, ta có thể nói chúng có **tương quan (correlation)**.

Ví dụ:

~~~text
nhiệt độ tăng
↓
doanh số kem tăng
~~~

Hai thứ có quan hệ.

Nhưng không phải vì kem làm thời tiết nóng hơn.

Một yếu tố khác — mùa hè — ảnh hưởng cả hai.

Yếu tố làm ta dễ nhầm quan hệ như vậy được gọi là **biến gây nhiễu (confounder)**.

Trong hệ thống máy tính cũng vậy.

## Một khuôn mẫu thật rất thuyết phục

> **KẾT QUẢ ĐO — Measured Result `[E-MECH-01]`**
>
> Trong **W-S** của phép đo mục tiêu, so sánh LM-head Q6 với FFN-down Q6 cho thấy:
>
> ~~~text
> DRAM read amplification
> LM head      4.268
> FFN-down     1.436
>
> LSC hit ratio
> LM head      0.103
> FFN-down     0.738
>
> XVE SBID stall
> LM head      81.78%
> FFN-down     56.50%
>
> ALU1 utilization
> LM head      ~1.58%
> FFN-down     ~13.84%
> ~~~

Khuôn mẫu này rất dễ dẫn tới câu chuyện:

~~~text
DRAM cao
+ cache hit thấp
+ stall cao
+ ALU utilization thấp
↓
“memory bottleneck”
~~~

Nhưng nghiên cứu không được phép dừng ở câu chuyện hợp lý nhất.
## Vì sao khuôn mẫu đó chưa đủ?

Vì các counter cần để phân biệt cơ chế không phải tất cả đều đáng tin trong kênh đo hiện tại.

Chương 17 đã thấy `GpuTime = 0` và các bất nhất cross-group ở `XVE_STALL` cùng `GPU_MEMORY_REQUEST_QUEUE_FULL`.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> Một khuôn mẫu định hướng mạnh **không thể vượt qua** khi đánh giá độ đầy đủ của phép đo đã FAIL.

Kết luận chính thức phải giữ nguyên:

~~~text
MEASUREMENT ADEQUACY: FAIL
MECHANISM: UNRESOLVED
CONFIRMED MECHANISM: NONE
~~~

> **UNRESOLVED / no confirmed mechanism**

Đây là verdict của bằng chứng hiện tại, không phải một khoảng trống được phép lấp bằng diễn giải thuận mắt.
## Bằng chứng định hướng

Các con số vẫn có giá trị như **bằng chứng định hướng (directional evidence)**.

Nghĩa là: chúng chỉ ra một hướng đáng kiểm tra tiếp, không chứng minh hướng đó đúng.

~~~text
khuôn mẫu
   ↓
giả thuyết
   ↓
phép thử đủ phân biệt
   ↓
bằng chứng đủ mạnh
   ↓
kết luận trong đúng phạm vi
~~~
## “Vì sao” cần mạnh hơn “ở đâu”

Chương 16 trả lời:

~~~text
định vị → thời gian nằm ở đâu?
~~~

Chương này hỏi:

~~~text
cơ chế → vì sao thời gian nằm ở đó?
~~~

Muốn trả lời “vì sao”, phép thử phải có khả năng phân biệt các giải thích cạnh tranh, ví dụ lưu lượng bộ nhớ, hình học thực thi hay phụ thuộc tuần tự.
## Can thiệp có kiểm soát

Một cách tăng sức mạnh bằng chứng là **can thiệp (intervention)**: thay đổi có kiểm soát một yếu tố rồi quan sát phản ứng so với đối chứng.

~~~text
giữ các yếu tố khác ổn định
↓
thay đổi một yếu tố mục tiêu
↓
đo phản ứng
↓
so với đối chứng
↓
đánh giá giả thuyết
~~~

Nhưng can thiệp chỉ có giá trị khi nó giữ tính đúng, tránh thay nhiều yếu tố cùng lúc và có kênh đo đủ đáng tin.
## Can thiệp không phải phép màu

Đổi một kernel rồi thấy nhanh hơn vẫn chưa đủ để khẳng định cơ chế nếu đầu ra thay đổi, nhiều yếu tố bị đổi cùng lúc, phép đo không ổn định hoặc kết quả không lặp lại.

Nhân quả không đến từ chữ “can thiệp”. Nó đến từ **thiết kế phép thử đủ phân biệt**.
## Một sơ đồ kỷ luật bằng chứng

> **[FIGURE F23] — Thang bằng chứng: quan sát → khuôn mẫu → giả thuyết → độ đầy đủ → can thiệp → kết luận**
>
> ~~~text
> QUAN SÁT
>    ↓
> KHUÔN MẪU
>    ↓
> GIẢ THUYẾT
>    ↓
> ĐỘ ĐẦY ĐỦ PHÉP ĐO?
>    ├─ FAIL → UNRESOLVED
>    │
>    └─ PASS
>        ↓
> PHÉP THỬ PHÂN BIỆT / CAN THIỆP
>        ↓
> KẾT QUẢ
>        ↓
> KẾT LUẬN TRONG PHẠM VI
> ~~~

`UNRESOLVED` không phải ô trống. Nó là trạng thái tri thức: **ta biết bằng chứng hiện tại chưa đủ**.
## Quay lại “mảnh tri thức”

Kỷ luật này cũng áp dụng khi ta nhìn biểu diễn trong mô hình.

Nếu một hướng trong không gian trạng thái liên quan mạnh với một thuộc tính, ta mới có một **tín hiệu liên quan**. Muốn gọi nó là nơi lưu, cơ chế hay nguyên nhân của hành vi, ta cần bằng chứng mạnh hơn.

> **Tín hiệu không tự động trở thành cơ chế.**
## Còn câu hỏi cuối cùng

Ta đã đi từ văn bản tới token, biểu diễn, nhiều lớp, thực thi, dấu vết, counter và khuôn mẫu.

Nhưng tên sách vẫn hỏi về **“mảnh tri thức”**.

Vậy sau toàn bộ hành trình này, ta thật sự có quyền nói tới đâu? Chương 20 sẽ chỉ tổng kết những gì bằng chứng đã cho phép — không đi xa hơn.

### Nhớ 3 điều

1. **Một khuôn mẫu định hướng mạnh vẫn chưa phải bằng chứng nhân quả.** `[E-MECH-01]` giữ verdict **UNRESOLVED / no confirmed mechanism**.
2. **Muốn nói “vì sao”, cần phép thử đủ khả năng phân biệt các giải thích cạnh tranh và kênh đo đủ đáng tin.**
3. **UNRESOLVED là kết quả khoa học hợp lệ.** Nó giữ ranh giới giữa điều đã quan sát và điều chưa được chứng minh.

**Tiếp theo: [Chương 20 — Từ phép đo tới điều ta thật sự biết](20-tu-phep-do-toi-dieu-ta-biet.md)**
