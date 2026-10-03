# Chương 15 — Dấu vết thực thi: những dấu chân một token để lại

> **Mức đọc: Nghiên cứu**
>
> **Ta đã đi tới tầng vật lý:**
>
> ~~~text
> token hiện tại
>      ↓
> phép tính logic
>      ↓
> dispatch
>      ↓
> bộ nhớ + GPU + đồng bộ
> ~~~
>
> Bây giờ câu hỏi là: **làm sao ghi lại chuỗi hoạt động đó đủ rõ để ta biết công việc nào chạy, lúc nào và thuộc phần nào của mô hình?**

Hãy tưởng tượng muốn điều tra một chuyến tàu.

Nếu chỉ biết:

> Tàu đến ga lúc 10 giờ.

ta chưa biết nhiều.

Nhưng nếu có bản ghi:

~~~text
09:00 rời ga A
09:18 dừng ga B
09:20 rời ga B
09:47 dừng ga C
10:00 đến ga D
~~~

ta có một **dòng thời gian**.

Trong hệ thống tính toán, ý tưởng tương tự được gọi là **dấu vết thực thi (execution trace)**.

## Một trace ghi lại điều gì?

Tùy công cụ và mục đích, trace có thể chứa công việc đã gửi, thời điểm bắt đầu/kết thúc, thứ tự thực thi, điểm đồng bộ và danh tính logic mà công việc phục vụ.

> **[FIGURE F17] — Execution trace timeline**
>
> ~~~text
> thời gian ─────────────────────────────→
> dispatch A   [██████]
> dispatch B          [███]
> barrier                 |
> dispatch C               [████████]
> ~~~

F17 là sơ đồ giải thích cấu trúc timeline, không phải trace đo thật.
## Dấu thời gian

Một **dấu thời gian (timestamp)** đánh dấu thời điểm trong quá trình thực thi. Ở dạng đơn giản:

~~~text
bắt đầu = t1
kết thúc = t2
thời lượng = t2 - t1
~~~

Trên GPU, timestamp phải được hiểu theo cơ chế mà phần cứng và API cung cấp; không nên giả định mọi timestamp có cùng độ chính xác hay ý nghĩa.
## Đồng bộ là gì?

Một công việc có thể phải đợi công việc khác trước khi tiếp tục. Các cơ chế kiểm soát thứ tự như vậy thuộc **đồng bộ (synchronization)**; trace có thể ghi những điểm như **hàng rào (barrier)**.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> Có barrier trong trace không có nghĩa toàn bộ khoảng thời gian giữa hai dispatch là “barrier cost”. Attribution về thời gian phải có bằng chứng riêng.
## Một bước sinh token thật

> **KẾT QUẢ ĐO — Measured Result `[E-TRACE-01]`**
>
> Trong trace của một bước sinh token trên Intel Arc 140V:
>
> ~~~text
> 469 dispatch vật lý
> 469 dispatch có timestamp
> 470 bản ghi barrier
> 451 phép tính logic được đo
>
> phép tính logic chưa đo       = 0
> định danh logic không biết    = 0
> dispatch không quy chiếu được = 0
> ~~~

Trong phạm vi trace đó, không có dispatch mục tiêu nào rơi khỏi bản đồ logic.

> **[FIGURE F18] — Measured coverage + unattributed device time**
## Vì sao 469 lại quy về 451?

Chương 12 đã cho câu trả lời: một phép tính logic có thể tương ứng nhiều dispatch.

> **KẾT QUẢ ĐO — Measured Result `[E-DECOMP-01]`**
>
> LM head của trace mục tiêu ánh xạ từ **1 logical operation → 19 dispatches**.

Vì vậy số physical dispatch có thể lớn hơn số phép tính logic mà không tạo mâu thuẫn.
## Trace có bao phủ hết thời gian không?

> **KẾT QUẢ ĐO — Measured Result `[E-TRACE-02]`**
>
> ~~~text
> device span                  = 1,240,766,914 ns
> summed dispatch duration     = 1,239,803,214 ns
> difference                   =       963,700 ns
> difference / device span     ≈         0.078%
> ~~~

Phần chênh `963,700 ns` được gọi thận trọng là **thời gian thiết bị chưa quy chiếu (unattributed device time)**.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> Không được gọi toàn bộ `963,700 ns` là “barrier cost”. Khoảng chênh có thể chứa gap giữa dispatch, serialization, ảnh hưởng của timestamp hoặc chi phí khác chưa được phân loại.
## Trace không phải lời giải thích

Trace có thể trả lời rất tốt: công việc nào chạy trước/sau, kéo dài bao lâu và dispatch thuộc operation nào.

Nhưng nếu operation X mất nhiều thời gian, trace vẫn chưa tự trả lời **vì sao**.

Nguyên nhân có thể liên quan tới memory traffic, execution geometry, data dependency, parallelism, kernel design hoặc điều kiện khác.

> **Dấu vết định vị “ở đâu”; cơ chế cần thêm bằng chứng để nói “vì sao”.**
## Quy chiếu là chiếc cầu quan trọng nhất

Một timeline GPU thuần túy chỉ cho ta dispatch và timestamp. Người nghiên cứu mô hình cần biết chúng thuộc attention, FFN-down, LM head hay lớp nào.

~~~text
DẤU VẾT VẬT LÝ
dispatch + timestamp
        ↓
QUY CHIẾU
        ↓
DANH TÍNH LOGIC
operation + layer + role
~~~

Nhờ cầu nối này ta mới có thể cộng thời gian theo các họ phép tính có ý nghĩa đối với mô hình.
## Và khi cộng lại, thứ gì hiện ra?

Khi nhóm các dispatch theo danh tính logic rồi cộng thời gian, ta có thể tìm thấy:

> phần nào đang chiếm nhiều thời gian nhất?

Những vùng như vậy được gọi là **điểm nóng (hotspot)**.

Ở trace này, kết quả không phân bố đều.

Một vài họ phép tính chiếm phần lớn thời gian.

Đó là Chương 16.

### Nhớ 3 điều

1. **Dấu vết thực thi (execution trace) ghi lại công việc vật lý theo thời gian và có thể quy chiếu chúng về phép tính logic.**
2. **Trace 469 dispatch có 469 timestamp, 451 phép tính logic được đo và không có dispatch không quy chiếu trong phạm vi phép đo.**
3. **Trace trả lời rất tốt câu “thời gian nằm ở đâu”, nhưng chưa tự trả lời “vì sao chậm”.** Ngay cả 0,078% thời gian chưa quy chiếu cũng không được tự tiện gọi là chi phí barrier.

**Tiếp theo: [Chương 16 — Thời gian nằm ở đâu?](16-thoi-gian-nam-o-dau.md)**
