# Chương 3 — Từ mã token tới một biểu diễn bằng số

> **Mức đọc: Nền tảng**
>
> **Ta đã đi tới đâu?**
>
> ~~~text
> văn bản
>    ↓
> token
>    ↓
> mã token
>    ↓
> [ dãy số đầu tiên ]
> ~~~
>
> Chương này trả lời: **vì sao mô hình phải biến một mã token thành một dãy số, và dãy số đó là gì?**

Ở cuối Chương 2, ta có một vấn đề.

Token đã được đổi thành mã số:

~~~text
"Paris"
↓
8421
~~~

Nhưng nếu chỉ đưa số 8421 vào các phép tính, hệ thống gặp một khó khăn.

Con số này chủ yếu là **nhãn nhận diện**.

Khoảng cách giữa 8421 và 8422 không có nghĩa hai token tương ứng “gần nhau về ý nghĩa”.

Hai mã chỉ tình cờ đứng cạnh nhau trong bảng đánh số.

Vì vậy mô hình cần một cách biểu diễn khác.

## Một bảng tra cứu rất lớn

Hãy tưởng tượng mô hình có một bảng.

Mỗi hàng tương ứng với một mã token.

Ví dụ:

~~~text
mã 100  → [ ... nhiều số ... ]
mã 101  → [ ... nhiều số ... ]
mã 102  → [ ... nhiều số ... ]
...
mã 8421 → [ ... nhiều số ... ]
~~~

Khi token có mã 8421 xuất hiện, mô hình lấy hàng số tương ứng.

Dãy số đó là biểu diễn ban đầu của token.

Bước này được gọi là **phép nhúng (embedding)**.

Ta có:

~~~text
token
↓
mã token
↓
tra bảng nhúng
↓
một dãy số
~~~

> **[FIGURE F04] — Embedding lookup: token ID → hàng số đã học**

Bây giờ các phép tính của mô hình đã có thứ để làm việc.

## Tại sao phải là nhiều số?

Một mã số đơn lẻ chủ yếu giúp phân biệt danh tính. Một dãy nhiều số cho hệ thống một không gian phong phú hơn để học những quan hệ hữu ích.

> **Nhiều chiều cho hệ thống một không gian phong phú hơn để mã hóa những quan hệ mà quá trình huấn luyện thấy hữu ích.**

## Vectơ là gì?

Một dãy số như:

> **MINH HỌA — Illustration**
>
> ~~~text
> [0,12, -0,44, 0,07, 1,03, ...]
> ~~~

thường được gọi là một **vectơ (vector)**.

Ở mức cần thiết cho cuốn sách này:

> **Vectơ là một dãy số có thứ tự.**

Biểu diễn của token trong mô hình thường có nhiều chiều hơn rất nhiều.

Nhưng ý tưởng vẫn là một dãy số.

## Khối số là gì?

Nhiều dãy số có thể được sắp thành những cấu trúc lớn hơn để mô hình tính toán.

Một tên chuyên ngành rất phổ biến cho những cấu trúc số như vậy là **khối số (tensor)**.

Ở mức cần thiết cho chương này, ta có thể hiểu:

> **Khối số (tensor) là một nhóm các con số được sắp theo một hình dạng để máy tính xử lý.**

Một vectơ là một trường hợp đơn giản của khối số.

Một bảng nhiều hàng, mỗi hàng là biểu diễn của một token, cũng là một khối số.

Có thể hình dung:

~~~text
bảng nhúng
=
rất nhiều hàng số

mỗi hàng
=
biểu diễn ban đầu của một token
~~~

Vậy phép nhúng không phải phép màu.

Ở mức trực giác, nó là:

> **dùng mã token để chọn đúng hàng số đã được học.**

## Những con số trong hàng đó đến từ đâu?

Chúng không được người lập trình tự tay viết.

Các giá trị này được điều chỉnh trong quá trình **huấn luyện (training)**.

Trong quá trình đó, rất nhiều con số có thể học được bên trong mô hình được điều chỉnh. Những con số như vậy được gọi chung là **tham số (parameters)**; một loại rất phổ biến là **trọng số (weights)**.

Khi mô hình học dự đoán token tiếp theo trên lượng dữ liệu lớn, các trọng số — trong đó có bảng nhúng — dần thay đổi.

Qua rất nhiều bước huấn luyện, hệ thống tìm ra những cấu hình số giúp nó dự đoán tốt hơn.

Vì vậy hàng biểu diễn ban đầu không phải một từ điển do con người gắn nhãn.

Nó là sản phẩm của quá trình học.

## Gần nhau về số có nghĩa gần nhau về ý nghĩa không?

Có lúc các biểu diễn có quan hệ thú vị theo một số thước đo. Nhưng không nên biến quan sát “hai vectơ gần nhau” thành kết luận “mô hình hiểu hai khái niệm giống hệt con người”.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> Khoảng cách số là thuộc tính của biểu diễn. Ý nghĩa là một diễn giải lớn hơn và cần thêm bằng chứng.

Đây là một chủ đề sẽ quay lại nhiều lần trong sách:

~~~text
quan sát được một cấu trúc
≠
đã giải thích được ý nghĩa của cấu trúc
~~~

## Biểu diễn ban đầu vẫn chưa biết toàn bộ ngữ cảnh

Giả sử token:

~~~text
bank
~~~

được đổi thành một vectơ ban đầu.

Bảng nhúng chủ yếu dựa vào **danh tính token**.

Nhưng câu:

~~~text
I deposited money at the bank.
~~~

và câu:

~~~text
We sat on the river bank.
~~~

có ngữ cảnh khác nhau.

Nếu biểu diễn của token chỉ giữ nguyên từ đầu tới cuối, mô hình sẽ khó phân biệt hai cách dùng.

Đây là lý do biểu diễn ban đầu chỉ là **điểm xuất phát**.

Sau khi đi vào Transformer, trạng thái sẽ tiếp tục được biến đổi dựa trên những token xung quanh.

Ta có thể vẽ:

~~~text
mã token
↓
biểu diễn ban đầu
↓
lớp 1
↓
biểu diễn đã thay đổi
↓
lớp 2
↓
biểu diễn lại thay đổi
↓
...
~~~

Thứ đi xuyên mô hình vì vậy không phải một chiếc thẻ giữ nguyên.

Nó giống một bản ghi liên tục được cập nhật.

## Một ví dụ: hồ sơ được bổ sung dần

Hãy tưởng tượng một hồ sơ ban đầu chỉ có tên “An”. Khi ta lần lượt biết thêm “An đang ở sân bay”, “An vừa kiểm tra hộ chiếu”, “An đang ở cửa khởi hành”, tên không đổi nhưng trạng thái thông tin quanh “An” đã giàu hơn.

Mô hình không hiểu theo đúng cách con người hiểu, nhưng phép so sánh giữ được trực giác:

> **danh tính có thể giữ nguyên trong khi trạng thái biểu diễn được cập nhật theo ngữ cảnh.**

## “Mảnh tri thức” bắt đầu trở nên khó nắm

Nếu ta hỏi:

> Mảnh tri thức đang nằm ở token hay ở vectơ?

câu trả lời vẫn chưa thể là một trong hai.

Bởi vectơ ban đầu còn chưa biết đầy đủ ngữ cảnh.

Sau nhiều lớp, trạng thái lại khác.

Và các trọng số của chính mô hình cũng tham gia vào mọi phép biến đổi.

Một cách nói thận trọng hơn:

~~~text
token
→ cho biết ta bắt đầu từ đơn vị nào

biểu diễn ban đầu
→ cho mô hình một trạng thái số để bắt đầu

ngữ cảnh + các lớp
→ tiếp tục biến đổi trạng thái đó
~~~

Ta đang rời xa ý tưởng “tri thức là một vật thể nằm yên”.

Thay vào đó, ta bắt đầu thấy tri thức có thể liên quan tới **một quá trình biến đổi**.

## Chương tiếp theo

Ta đã có:

~~~text
token
↓
mã token
↓
biểu diễn ban đầu
~~~

Nhưng một câu gồm nhiều token.

Làm sao biểu diễn của token hiện tại chịu ảnh hưởng từ những token còn lại?

Vì sao cùng một token trong hai câu khác nhau có thể dần mang trạng thái khác nhau?

Trước khi đi sâu vào cơ chế chú ý, ta cần hiểu một khái niệm rộng hơn:

> **ngữ cảnh làm biểu diễn thay đổi.**

Đó là Chương 4.

### Nhớ 3 điều

1. **Mã token chỉ là danh tính; mô hình tra nó thành một vectơ số để bắt đầu tính toán.**
2. **Phép nhúng (embedding) là bước tạo biểu diễn ban đầu từ mã token; các giá trị này được học trong quá trình huấn luyện.**
3. **Biểu diễn ban đầu chưa phải ý nghĩa cuối cùng của token.** Khi token đi qua các lớp và tương tác với ngữ cảnh, trạng thái của nó còn tiếp tục thay đổi.

**Tiếp theo: [Chương 4 — Cùng một token, ngữ cảnh khác, biểu diễn khác](04-cung-token-ngu-canh-khac.md)**
