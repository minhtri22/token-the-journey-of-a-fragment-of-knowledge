# Chương 8 — Đường cộng tắt: vì sao thông tin cũ không biến mất hoàn toàn?

> **Mức đọc: Đi sâu**
>
> **Ta đang mở chiếc hộp nào?**
>
> ~~~text
> trạng thái cũ ──────────┐
>                        │
>       phép biến đổi     │
>            ↓           │
>       phần thay đổi ───┘
>            ↓
>      trạng thái mới
> ~~~
>
> Chương này giải thích **đường cộng tắt (residual connection)** — một cấu trúc giúp mỗi lớp thêm thay đổi mới mà không buộc phải thay thế toàn bộ tín hiệu cũ.

Hãy tưởng tượng bạn đang sửa một tài liệu.

Có hai cách.

Cách thứ nhất:

> Xóa toàn bộ trang cũ rồi viết lại từ đầu.

Cách thứ hai:

> Giữ bản cũ, tạo phần chỉnh sửa, rồi áp phần chỉnh sửa đó lên nội dung đang có.

Transformer gần với trực giác thứ hai hơn.

## Một phép cộng rất quan trọng

Ta ký hiệu trạng thái đi vào một khối là:

~~~text
x
~~~

Khối biến đổi tạo ra:

~~~text
F(x)
~~~

Đường cộng tắt tạo:

~~~text
x + F(x)
~~~

Không cần công thức sâu hơn. Ý quan trọng là:

~~~text
trạng thái mới
=
trạng thái cũ
+
phần thay đổi
~~~

Đó là lý do từ “residual” thường được dịch gần với ý **phần dư / phần thay đổi còn lại**.

Trong sách, ta dùng cách gọi dễ hiểu hơn:

> **đường cộng tắt (residual connection)**

## Vì sao gọi là “đường tắt”?

Hãy nhìn sơ đồ:

~~~text
             ┌───────────────┐
             │               ↓
trạng thái ──┼→ phép biến đổi ─→ kết quả
             │               │
             └───────────────┘
                     cộng
                      ↓
               trạng thái mới
~~~

Một phần tín hiệu đi qua phép biến đổi.

Một đường khác đi vòng qua phép biến đổi.

Hai đường gặp lại ở phép cộng.

Vì vậy ta có thể nghĩ nó như một “đường tắt”.

## Điều này giúp gì cho câu chuyện của token?

Nhớ rằng ta đang đi theo trạng thái của một token qua nhiều lớp.

Nếu mỗi lớp chỉ thay toàn bộ trạng thái:

~~~text
x0
↓
x1
↓
x2
↓
x3
↓
...
~~~

ta dễ hình dung như mỗi bước viết đè hoàn toàn lên bước trước.

Nhưng với residual:

> **[FIGURE F09] — Residual stream: giữ đường cũ + cộng phần cập nhật**
>
~~~text
x0
↓
x0 + Δ1
↓
x0 + Δ1 + Δ2
↓
x0 + Δ1 + Δ2 + Δ3
↓
...
~~~

Ký hiệu Δ ở đây chỉ là hình ảnh trực giác cho “phần thay đổi”, không phải công thức chính xác cho mọi lớp.

Nó giúp thấy rằng trạng thái có thể được **cập nhật dần**.

## Attention cũng đi qua residual

Trong một lớp kiểu phổ biến:

~~~text
trạng thái ban đầu
      │
      ├──────────────┐
      ↓              │
  attention          │
      ↓              │
  phần thay đổi      │
      └────── cộng ──┘
             ↓
      trạng thái giữa
~~~

Sau đó FFN lại có một residual khác:

~~~text
trạng thái giữa
      │
      ├──────────────┐
      ↓              │
     FFN             │
      ↓              │
  phần thay đổi      │
      └────── cộng ──┘
             ↓
      trạng thái ra
~~~

Do đó một lớp không đơn giản là:

~~~text
attention → FFN
~~~

mà gần hơn với:

~~~text
cập nhật bằng attention
        +
giữ đường cũ

rồi

cập nhật bằng FFN
        +
giữ đường cũ
~~~

## Residual có nghĩa thông tin cũ luôn được bảo toàn nguyên vẹn không?

Không.

Đây là chỗ cần cẩn thận.

Tín hiệu cũ có một đường đi trực tiếp qua phép cộng.

Nhưng sau nhiều lớp, trạng thái tổng thể vẫn có thể thay đổi rất mạnh.

Các phép cộng tiếp theo, chuẩn hóa và biến đổi tiếp tục tác động.

Vì vậy không nên nói:

> “Residual đảm bảo mọi thông tin cũ không bao giờ mất.”

Câu đúng hơn là:

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> **Residual tạo một đường trực tiếp để trạng thái trước tham gia vào trạng thái sau, thay vì bảo đảm mọi thông tin cũ được giữ nguyên.**

## Một ví dụ đời thường khác

Hãy tưởng tượng một bản đồ được chỉnh sửa qua nhiều phiên bản: nền cũ vẫn còn, rồi mỗi phiên bản thêm một lớp thông tin mới.

Residual không phải các lớp bản đồ theo nghĩa đen, nhưng trực giác **giữ nền + thêm thay đổi** giúp ta hiểu vì sao trạng thái có thể được cập nhật dần thay vì bị viết đè hoàn toàn.
## Dòng trạng thái tích lũy

Khi residual lặp lại qua nhiều lớp, người ta thường nói tới một **dòng residual (residual stream)**.

Ở mức nhập môn, hãy hiểu:

> **Đó là dòng trạng thái chính được truyền xuyên qua các lớp và liên tục nhận thêm các phần cập nhật.**

Ta có thể vẽ:

~~~text
dòng trạng thái
      ↓
  + attention
      ↓
dòng trạng thái
      ↓
  + FFN
      ↓
dòng trạng thái
      ↓
  sang lớp tiếp
~~~

Đây là một cách nhìn rất quan trọng cho các chương sau.

Vì khi ta hỏi:

> “Một token đã biến dạng thế nào sau lớp 5?”

thứ ta thật sự muốn so sánh không phải chữ token.

Ta muốn nhìn **trạng thái số trên dòng residual** ở các thời điểm khác nhau.

## SIDEBAR — Có thể lưu lại trạng thái để đi tắt không?

Một câu hỏi tự nhiên là: nếu mô hình đã xử lý một ngữ cảnh, tại sao không lưu trạng thái đó để lần sau khỏi tính lại?

Điểm cần tách rõ là:

~~~text
trọng số trong model
≠
trạng thái tạm thời của một lần chạy
~~~

Trạng thái được tạo ra **khi mô hình đang chạy** phụ thuộc token, vị trí, toàn bộ ngữ cảnh, trạng thái lớp trước và trọng số hiện tại. Các cập nhật ở lớp sau lại phụ thuộc kết quả lớp trước, nên không thể coi chúng như những mảnh cố định có thể cộng lại tùy ý.

Có những kỹ thuật gần với ý tưởng “đi tắt”, chẳng hạn giữ lại trạng thái cần thiết của phần đầu vào đã xử lý để tránh tính lại. **Bộ nhớ đệm khóa–giá trị (KV cache)** và một số dạng bộ nhớ đệm phần đầu vào thuộc tinh thần này.

Nhưng cần giữ ranh giới:

~~~text
CACHE
→ dùng lại kết quả của một lần xử lý

UPDATE MODEL
→ thay đổi thứ mô hình đã học
~~~

Hai việc này khác nhau. Sidebar này chỉ mở trực giác, không mở sang các cơ chế học hay thích nghi mô hình trong lúc chạy.
## Đây có phải nơi “mảnh tri thức” chạy qua?

Dòng residual là nơi rất hữu ích để theo dõi sự thay đổi, nhưng không nên kết luận:

> “Dòng residual chính là tri thức.”

Nó là một cấu trúc số được nhiều thành phần liên tục đọc, biến đổi và cập nhật. Hành vi của mô hình còn phụ thuộc trọng số, ngữ cảnh, kiến trúc và các phép biến đổi tiếp theo.

Ta đang theo dấu **trạng thái**, không tìm một viên tri thức vật lý.
## Nhiều residual nối tiếp nhau sẽ tạo ra gì?

Ta đã có:

~~~text
lớp 1
↓
cập nhật trạng thái

lớp 2
↓
cập nhật tiếp

lớp 3
↓
cập nhật tiếp
~~~

Nhưng tại sao cần nhiều lớp như vậy?

Một lớp có thể làm ít hay nhiều đến đâu?

Có phải lớp 1 học từ, lớp 2 học câu, lớp 3 học tri thức?

Câu trả lời không đơn giản như vậy.

Đó là nội dung của Chương 9.

### Nhớ 3 điều

1. **Đường cộng tắt (residual connection) cho phép trạng thái cũ đi trực tiếp tới phép cộng với phần thay đổi mới.**
2. **Residual không đảm bảo mọi thông tin được giữ nguyên mãi mãi; nó chỉ tạo một đường trực tiếp để trạng thái trước tham gia vào trạng thái sau.**
3. **Dòng residual (residual stream) là một cách hữu ích để nghĩ về trạng thái số đang được cập nhật xuyên qua nhiều lớp.**

**Tiếp theo: [Chương 9 — Một lớp chưa biết cả câu — vậy nhiều lớp làm được gì?](09-nhieu-lop-lam-duoc-gi.md)**
