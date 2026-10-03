# Chương 7 — Nhánh biến đổi tín hiệu: điều gì xảy ra sau cơ chế chú ý?

> **Mức đọc: Đi sâu**
>
> **Ta đang mở chiếc hộp nào?**
>
> ~~~text
> thông tin từ ngữ cảnh
>         ↓
> trạng thái hiện tại
>         ↓
> [ nhánh biến đổi tín hiệu ]
>         ↓
> trạng thái đã biến đổi
> ~~~
>
> Chương này giải thích trực giác của **mạng truyền thẳng (Feed-Forward Network, FFN)** — phần thường xuất hiện sau cơ chế chú ý trong mỗi lớp Transformer.

Ở Chương 6, ta thấy attention giúp một vị trí lấy thêm thông tin từ những vị trí khác.

Nhưng sau khi lấy được thông tin, trạng thái vẫn chưa “xong”.

Nó còn phải được biến đổi.

Đây là vai trò chính của FFN.

## Một phép so sánh đơn giản

Attention gần với câu hỏi:

> “Tôi cần lấy thêm thông tin nào từ những vị trí khác?”

FFN gần với câu hỏi:

> “Sau khi có thông tin đó, trạng thái tại vị trí này nên được biến đổi thế nào?”

FFN không chủ yếu đi lấy dữ liệu từ token khác; nó biến đổi trạng thái của **từng vị trí** bằng cùng một bộ trọng số đã học.
## “Truyền thẳng” nghĩa là gì?

Tên tiếng Anh là **Feed-Forward Network, FFN**.

“Truyền thẳng” ở đây chỉ cách tín hiệu đi qua một chuỗi phép biến đổi theo một hướng.

Ở mức rất đơn giản:

> **[FIGURE F08] — FFN: mở rộng → phi tuyến → thu về**
>
~~~text
trạng thái vào
     ↓
mở rộng
     ↓
biến đổi
     ↓
thu về
     ↓
trạng thái ra
~~~

Ta chưa cần công thức.

Chỉ cần thấy rằng FFN không phải một phép cộng đơn giản.

Nó tạo ra một phép biến đổi học được.

## Vì sao lại mở rộng chiều?

FFN thường đưa trạng thái sang một không gian lớn hơn rồi đưa về kích thước ban đầu.

> **MINH HỌA — Illustration**
>
> ~~~text
> [ trạng thái 4 chiều ]
>          ↓
> [ không gian 12 chiều ]
>          ↓
> [ trạng thái 4 chiều ]
> ~~~

Các con số chỉ để minh họa. Trực giác hữu ích là: mô hình có một không gian trung gian rộng hơn để thực hiện biến đổi trước khi quay về kích thước mà phần còn lại của mạng đang dùng.
## Tại sao không chỉ dùng phép biến đổi tuyến tính?

Nếu chuỗi biến đổi chỉ gồm các phép tuyến tính nối tiếp, khả năng biểu diễn sẽ bị hạn chế. Vì vậy FFN có một bước **phi tuyến (nonlinearity)**.

~~~text
trạng thái
   ↓
biến đổi tuyến tính
   ↓
phi tuyến
   ↓
biến đổi tiếp
   ↓
trạng thái mới
~~~

Một số kiến trúc còn dùng **gating** để điều tiết tín hiệu. Cuốn sách dừng ở trực giác này, không mở rộng thành khảo sát các biến thể như SwiGLU.
## Attention và FFN khác nhau ở đâu?

Ta có thể dùng một sơ đồ ngắn:

~~~text
ATTENTION
"nên lấy thêm thông tin từ đâu?"
        ↓
trộn thông tin giữa các vị trí

FFN
"sau khi có thông tin đó,
 trạng thái tại vị trí này
 nên được biến đổi thế nào?"
        ↓
biến đổi tại từng vị trí
~~~

Đây là trực giác, không phải ranh giới tuyệt đối về “ý nghĩa” của hai khối.

Cả hai đều dùng trọng số đã học.

Cả hai đều tham gia vào trạng thái cuối.

## Một ví dụ với từ “đá”

Quay lại:

~~~text
Tôi nhặt một hòn đá.
Tôi thích đá bóng.
~~~

Attention có thể làm hai trạng thái “đá” nhận tín hiệu ngữ cảnh khác nhau. FFN sau đó nhận hai đầu vào khác nhau và có thể tạo hai đầu ra khác nhau.

~~~text
trạng thái A → FFN → trạng thái A'
trạng thái B → FFN → trạng thái B'
~~~

FFN không cần “biết” từ đá theo cách con người; nó thực hiện phép biến đổi số đã học trên trạng thái hiện tại.
## FFN có phải nơi chứa tri thức không?

Đây là câu hỏi dễ bị trả lời quá mạnh.

Các trọng số FFN có thể liên quan tới nhiều mẫu thông tin và hành vi mà mô hình đã học. Nhưng mô hình còn có embedding, attention, residual, nhiều lớp, trạng thái theo ngữ cảnh và lớp đầu ra.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> **FFN là một phần quan trọng của quá trình biến đổi trạng thái đã học, nhưng không nên được coi như một chiếc tủ chứa tri thức độc lập.**
## Một lớp có hai kiểu chuyển động

Tới đây, ta có thể nhìn một lớp theo hai hướng:

~~~text
1. chuyển động NGANG giữa các vị trí
   attention

2. chuyển động SÂU trong trạng thái của từng vị trí
   FFN
~~~

Đây là cách hình dung hữu ích.

Nhưng đừng hiểu chữ “ngang” và “sâu” như thuật ngữ toán học chính thức.

Nó chỉ giúp phân biệt:

- attention chủ yếu trộn thông tin giữa vị trí;
- FFN chủ yếu biến đổi trạng thái tại mỗi vị trí.

## Nếu cứ biến đổi mãi, trạng thái cũ có mất hết không?

Đây là câu hỏi quan trọng.

Nếu mỗi khối chỉ lấy đầu vào rồi thay hoàn toàn bằng đầu ra mới, việc giữ thông tin cũ qua hàng chục lớp sẽ khó hơn.

Transformer dùng một cấu trúc rất quan trọng:

> **đường cộng tắt (residual connection)**

cho phép tín hiệu cũ đi vòng qua phép biến đổi rồi được cộng trở lại.

Ta sẽ mở chiếc hộp đó ở chương sau.

### Nhớ 3 điều

1. **Mạng truyền thẳng (FFN) biến đổi trạng thái tại từng vị trí sau khi thông tin ngữ cảnh đã được trộn.**
2. **FFN thường mở rộng trạng thái sang một không gian lớn hơn, áp dụng biến đổi phi tuyến rồi đưa về kích thước cần dùng.**
3. **FFN không nên được gọi đơn giản là “nơi chứa tri thức”.** Nó là một phần trong chuỗi biến đổi phân tán trên nhiều thành phần của mô hình.

**Tiếp theo: [Chương 8 — Đường cộng tắt: vì sao thông tin cũ không biến mất hoàn toàn?](08-duong-cong-tat.md)**
