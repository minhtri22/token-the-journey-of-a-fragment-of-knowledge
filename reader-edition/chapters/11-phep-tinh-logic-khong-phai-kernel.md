# Chương 11 — Một phép tính logic không phải một chương trình GPU

> **Mức đọc: Đi sâu**
>
> **Ta đang đổi tầng quan sát:**
>
> ~~~text
> sơ đồ mô hình
>      ↓
> phép tính logic
>      ↓
> [ hệ thực thi biến nó thành công việc thật ]
>      ↓
> GPU
> ~~~
>
> Nửa đầu cuốn sách nói về token trong **mô hình**. Từ chương này, ta nhìn xuống một tầng thấp hơn: **phần cứng thật sự phải làm gì để những phép tính đó xảy ra?**

Ở Chương 10, ta nói:

~~~text
trạng thái cuối
↓
lớp tạo điểm đầu ra
↓
logits
~~~

Trên sơ đồ, “lớp tạo điểm đầu ra” trông giống một chiếc hộp.

Nhưng GPU không nhận một tờ giấy ghi:

> “Hãy chạy LM head.”

Phần cứng nhận những công việc cụ thể hơn.

Đây là chỗ ta phải tách hai thế giới.

## Thế giới thứ nhất: phép tính logic

Khi mô tả mô hình, ta dùng những tên như attention, phép chiếu Q/K/V, FFN-up, FFN-down hay LM head.

Đó là các **phép tính logic (logical/model-level operation)**: danh tính công việc theo vai trò của nó trong đồ thị mô hình.

Ví dụ:

~~~text
FFN-down ở lớp 12
~~~

cho ta biết công việc đó đang đóng vai trò gì trong mô hình, nhưng chưa nói phần cứng thực thi nó bằng bao nhiêu kernel hay dispatch.
## Thế giới thứ hai: công việc vật lý

GPU cần một chương trình tính toán cụ thể.

Một chương trình nhỏ chạy trên GPU thường được gọi là **chương trình GPU (kernel)**.

Khi hệ thực thi gửi một lần công việc GPU đi chạy, ta gọi đó là một **lần giao việc (dispatch)**.

Ta có thể tạm hình dung:

> **[FIGURE F13] — Hai tầng: model operation ↔ physical dispatch timeline**
>
~~~text
phép tính logic
      ↓
chọn / chuẩn bị chương trình GPU
      ↓
gửi một hoặc nhiều lần giao việc
      ↓
GPU thực thi
~~~

Điều quan trọng là:

> **Một phép tính logic không bắt buộc tương ứng đúng một lần giao việc.**

## Ví dụ đời thường: “chuyển nhà”

Trên danh sách công việc, bạn có một mục:

~~~text
Chuyển tủ sách sang nhà mới
~~~

Đó là **một công việc theo ý nghĩa**.

Nhưng thực tế có thể cần:

~~~text
tháo kệ
↓
chuyển thùng 1
↓
chuyển thùng 2
↓
chuyển khung
↓
lắp lại
~~~

Một dòng trong kế hoạch có thể trở thành nhiều việc vật lý.

Ngược lại, một chuyến xe cũng có thể chở đồ của nhiều mục công việc cùng lúc.

Sơ đồ logic và cách thi công thật không nhất thiết một-một.

## Một phép đo thật cho thấy điều đó

> **KẾT QUẢ ĐO — Measured Result `[E-TRACE-01]`**
>
> Trong một phép đo của **một bước sinh token** trên Intel Arc 140V:
>
> ~~~text
> 469 lần giao việc vật lý (dispatch)
> 451 phép tính logic được đo
> ~~~
>
> Cả 469 dispatch đều có dấu thời gian và đều được quy chiếu về danh tính logic đã biết trong phạm vi phép đo.

Vì:

~~~text
469 ≠ 451
~~~

phép đo này trực tiếp bác bỏ cách hình dung “mỗi phép tính logic luôn tương ứng đúng một dispatch”.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> `469` và `451` chỉ mô tả phép đo, mô hình/hệ thực thi và phần cứng đã đo. Chúng không phải hằng số chung cho mọi LLM.
## Tại sao cần giữ hai danh tính riêng?

Nếu chỉ biết:

~~~text
dispatch 127 mất 3 ms
~~~

ta biết có một công việc vật lý chậm nhưng chưa biết nó thuộc phần nào của mô hình.

Ngược lại, nếu chỉ biết:

~~~text
FFN-down lớp 12
~~~

ta biết vai trò logic nhưng chưa biết phần cứng đã chia và chạy nó thế nào.

Vì vậy cần một cầu nối:

~~~text
danh tính logic
      ↕
quy chiếu
      ↕
công việc vật lý
~~~

**Quy chiếu (attribution)** là việc gắn một quan sát vật lý trở lại phép tính logic mà nó phục vụ.
## Một sơ đồ có hai tầng

F13 đặt hai cách nhìn cạnh nhau:

~~~text
TẦNG LOGIC                  TẦNG VẬT LÝ

Attention                   dispatch 1
FFN-up                      dispatch 2
FFN-gate                    dispatch 3
FFN-down                    ...
LM head                     dispatch 469
       \                    /
        \---- attribution --/
~~~

Không có ánh xạ đó, ta chỉ có hai danh sách rời nhau.
## “Chương trình GPU” và “lần giao việc” có giống nhau không?

Không.

**Chương trình GPU (kernel)** là phần mã tính toán. **Lần giao việc (dispatch)** là một lần cụ thể kernel được gửi đi với cấu hình công việc cụ thể.

Cùng một kernel có thể được dispatch nhiều lần. Vì vậy đếm “bao nhiêu lần GPU nhận việc” là đếm dispatch, không nhất thiết là đếm bao nhiêu kernel khác nhau.
## Tại sao điều này quan trọng khi đi theo token?

Ở nửa đầu sách, ta nói:

> token đi qua lớp 1, lớp 2, lớp 3...

Đó là cách nhìn logic.

Nhưng ở phần cứng, cùng đoạn đường có thể trông như:

~~~text
token hiện tại
↓
hàng chục phép tính logic
↓
hàng trăm lần giao việc vật lý
↓
đọc / ghi bộ nhớ
↓
đồng bộ
↓
tiếp tục
~~~

Nếu muốn hỏi:

> “Token mất thời gian ở đâu?”

ta không thể chỉ nhìn sơ đồ Transformer.

Ta phải biết các phép tính logic đã trở thành công việc vật lý nào.

## Và một phép tính có thể bị chia nhỏ

Trong chính trace vừa nhắc, LM head ở cuối mô hình không chạy bằng một dispatch duy nhất.

> **KẾT QUẢ ĐO — Measured Result `[E-DECOMP-01]`**
>
> ~~~text
> 1 LM-head logical operation
> ↓
> 19 dispatch vật lý
> ~~~

Đây là ví dụ trực tiếp cho quan hệ một-nhiều. Vì sao hệ thực thi chia như vậy là câu hỏi của Chương 12.

### Nhớ 3 điều

1. **Phép tính logic mô tả công việc ở mức mô hình; kernel và dispatch mô tả cách công việc được thực thi trên GPU.**
2. **Hai tầng không bắt buộc ánh xạ một-một.** Trace mục tiêu có 451 phép tính logic và 469 dispatch.
3. **Muốn biết thời gian vật lý thuộc phần nào của mô hình, ta cần quy chiếu (attribution).**

**Tiếp theo: [Chương 12 — Một phép tính có thể trở thành nhiều lần giao việc cho phần cứng](12-mot-phep-tinh-nhieu-lan-giao-viec.md)**
