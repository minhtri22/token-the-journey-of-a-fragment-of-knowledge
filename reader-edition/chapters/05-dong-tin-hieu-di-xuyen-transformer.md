# Chương 5 — Dòng tín hiệu đi xuyên Transformer

> **Mức đọc: Đi sâu**
>
> **Ta đã đi tới đâu?**
>
> ~~~text
> token
>   ↓
> biểu diễn ban đầu
>   ↓
> trạng thái phụ thuộc ngữ cảnh
>   ↓
> [ nhiều lớp Transformer (layers) ]
> ~~~
>
> Chương này chưa mở công thức. Mục tiêu chỉ là nhìn được **bản đồ bên trong một lớp** trước khi đi sâu vào từng phần.

Ở Chương 4, ta liên tục nói rằng trạng thái của token được cập nhật khi đi qua nhiều lớp.

Nhưng **một lớp xử lý (layer)** là gì?

Ta thử mở một chiếc hộp.

## Một lớp không phải một phép tính duy nhất

Hãy hình dung một dây chuyền: trạng thái đi vào, qua vài trạm xử lý, rồi đi ra với trạng thái đã thay đổi.

Một **lớp Transformer (Transformer layer)** cũng có thể được nhìn như vậy. Với loại mô hình ngôn ngữ sinh token phổ biến hiện nay, một lớp thường có:

- bước chuẩn hóa;
- cơ chế cho các vị trí lấy thông tin từ nhau;
- một nhánh biến đổi trạng thái;
- các đường cộng lại tín hiệu cũ.

Tên kỹ thuật sẽ xuất hiện ngay sau khi ta có bản đồ tổng thể.
## Bản đồ đơn giản của một lớp

Ta bắt đầu từ trạng thái của các token:

> **[FIGURE F06] — Bản đồ chuẩn của một lớp Transformer**
>
~~~text
trạng thái đi vào lớp
        │
        ↓
   [ chuẩn hóa ]
        │
        ↓
 [ cơ chế chú ý ]
        │
        ↓
   cộng với đường cũ
        │
        ↓
   [ chuẩn hóa ]
        │
        ↓
 [ nhánh biến đổi ]
        │
        ↓
   cộng với đường cũ
        │
        ↓
trạng thái đi ra khỏi lớp
~~~

Đây là bản đồ đủ tốt để đọc các chương tiếp theo. Các Transformer cụ thể có thể khác về chi tiết, thứ tự hay công thức; cuốn sách chủ yếu dùng trực giác của mô hình ngôn ngữ **chỉ dùng bộ giải mã (decoder-only)**.

## Chuẩn hóa để làm gì?

Trạng thái bên trong mô hình là những dãy số. Sau nhiều phép biến đổi, độ lớn và phân bố của chúng có thể thay đổi.

Một bước **chuẩn hóa (normalization)** giúp đưa tín hiệu về dạng thuận lợi hơn cho phép tính tiếp theo. Một loại thường gặp là **chuẩn hóa RMS (RMSNorm)**.

Ta chưa cần công thức. Chỉ cần hiểu:

~~~text
dãy số đầu vào
      ↓
điều chỉnh thang đo
      ↓
dãy số phù hợp hơn cho bước tiếp
~~~

Chuẩn hóa không phải “chiếc hộp chứa tri thức”; nó là một phần của cách dòng tín hiệu được duy trì và xử lý.
## Cơ chế chú ý: nhìn sang những vị trí khác

Tên của phần này là **cơ chế chú ý (attention)**.

Hãy trở lại câu:

~~~text
Tôi nhặt một hòn đá.
~~~

Khi xử lý vị trí “đá”, trạng thái hiện tại có thể cần thông tin từ những vị trí trước như “nhặt”, “một”, “hòn”.

Cơ chế chú ý tạo ra một cách để một vị trí tính xem:

> Những vị trí nào trước đó có liên quan tới việc cập nhật trạng thái của tôi?

Ta có thể hình dung:

~~~text
[Tôi] [nhặt] [một] [hòn] [đá]
  │      │      │      │     ↑
  └──────┴──────┴──────┴─────┘
        thông tin có thể
        được gom về đây
~~~

Đây chỉ là trực giác.

Chương 6 sẽ mở bên trong và giới thiệu truy vấn, khóa, giá trị và điểm chú ý.

## Nhánh biến đổi: xử lý trạng thái tại từng vị trí

Sau khi một vị trí đã lấy thêm thông tin liên quan từ chuỗi, trạng thái của nó còn đi qua một nhánh biến đổi khác.

Nhánh này thường được gọi là **mạng truyền thẳng (Feed-Forward Network, FFN)**.

Ở mức trực giác:

~~~text
trạng thái hiện tại
      ↓
mở rộng sang không gian lớn hơn
      ↓
biến đổi phi tuyến
      ↓
thu về kích thước cần dùng
      ↓
trạng thái mới
~~~

Nếu attention chủ yếu giúp **trao đổi thông tin giữa các vị trí**, FFN có thể được hình dung như một trạm **biến đổi trạng thái tại từng vị trí**.

Đây là một **mô hình tinh thần (mental model)**. Chương 7 sẽ mở nó kỹ hơn.

## Đường cộng tắt: không vứt bỏ trạng thái cũ

Có một chi tiết rất quan trọng trong sơ đồ lớp:

~~~text
trạng thái cũ ───────────────┐
                             │
             phép biến đổi   │
                  ↓          │
             kết quả mới     │
                  │          │
                  └──── cộng ┘
                        ↓
                  trạng thái sau
~~~

Đường đi tắt đó được gọi là **đường cộng tắt (residual connection)**.

Nó cho phép phần tín hiệu cũ đi vòng qua một phép biến đổi rồi được cộng lại với phần thay đổi mới.

Nhờ vậy, lớp không nhất thiết phải “xóa sạch rồi viết lại” toàn bộ trạng thái.

Chương 8 sẽ dành riêng cho ý này.

## Một lớp chỉ là một lần cập nhật

Nếu có nhiều lớp:

~~~text
biểu diễn ban đầu
       ↓
     lớp 1
       ↓
 trạng thái 1
       ↓
     lớp 2
       ↓
 trạng thái 2
       ↓
      ...
~~~

mỗi lớp có cơ hội lấy thêm thông tin từ ngữ cảnh, biến đổi trạng thái và giữ một đường cho tín hiệu cũ đi tiếp.

> **Không có một lớp duy nhất phải làm toàn bộ công việc.**

Một lớp tạo ra thay đổi mà lớp sau có thể tiếp tục sử dụng. Đó là lý do chiều sâu của mô hình quan trọng.
## Ghép lại bản đồ của một lớp

F06 là hình chuẩn sẽ được dùng lại trong ba chương tiếp theo.

~~~text
trạng thái vào
     │
     ├──────── đường cũ ────────┐
     ↓                          │
chuẩn hóa                       │
     ↓                          │
attention                      │
     ↓                          │
phần cập nhật ──────────────── cộng
                                ↓
                         trạng thái giữa
                                │
                                ├── đường cũ ──┐
                                ↓               │
                           chuẩn hóa            │
                                ↓               │
                              FFN               │
                                ↓               │
                         phần cập nhật ─────── cộng
                                                ↓
                                         trạng thái ra
~~~

Chỉ cần giữ ba ý:

~~~text
lấy thông tin từ vị trí khác
          +
biến đổi trạng thái
          +
giữ đường tín hiệu cũ
~~~
## “Tri thức” nằm ở attention hay FFN?

Đây là câu hỏi hấp dẫn nhưng quá sớm.

Ta chưa nên viết:

> attention là nơi hiểu ngữ cảnh.

hay:

> FFN là nơi lưu tri thức.

Các thành phần tương tác qua nhiều lớp; trọng số đã học nằm ở nhiều nơi; trạng thái còn phụ thuộc ngữ cảnh.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> Một thành phần có thể đóng góp quan trọng cho hành vi mà không trở thành “chiếc tủ chứa tri thức” độc lập.

Câu hỏi tốt hơn là:

> Mỗi thành phần đóng góp kiểu biến đổi nào vào đường đi của trạng thái?

### Nhớ 3 điều

1. **Một lớp Transformer gồm nhiều bước, không phải một phép tính duy nhất.**
2. **Attention giúp các vị trí trao đổi thông tin; FFN biến đổi trạng thái; residual giữ một đường cho tín hiệu cũ đi tiếp.**
3. **Nhiều lớp nối tiếp nhau tạo thành một chuỗi cập nhật trạng thái, không phải một hộp duy nhất chứa toàn bộ tri thức.**

**Tiếp theo: [Chương 6 — Cơ chế chú ý: token nhìn những token khác như thế nào?](06-co-che-chu-y.md)**
