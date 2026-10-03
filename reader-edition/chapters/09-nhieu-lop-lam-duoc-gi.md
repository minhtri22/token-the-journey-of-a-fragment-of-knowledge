# Chương 9 — Một lớp chưa biết cả câu — vậy nhiều lớp làm được gì?

> **Mức đọc: Đi sâu**
>
> **Ta đã đi tới đâu?**
>
> ~~~text
> biểu diễn ban đầu
>      ↓
> lớp 1: cập nhật
>      ↓
> lớp 2: cập nhật
>      ↓
> lớp 3: cập nhật
>      ↓
> ...
> ~~~
>
> Chương này hỏi: **vì sao nhiều lớp nối tiếp nhau có thể làm được những điều mà một lớp đơn lẻ khó làm?**

Hãy tưởng tượng bạn cần giải một câu hỏi nhiều bước:

> Lan lớn tuổi hơn Mai. Mai lớn tuổi hơn An. Ai lớn tuổi nhất?

Một bước đọc có thể nhận ra từng quan hệ.

Nhưng để đi tới kết luận, ta cần kết hợp:

~~~text
Lan > Mai
Mai > An
↓
Lan > An
~~~

Đây là một ví dụ đời thường cho ý tưởng:

> **một kết quả trung gian có thể trở thành đầu vào hữu ích cho bước tiếp theo.**

Trong mạng nhiều lớp, điều tương tự xảy ra ở mức trạng thái số.

## Mỗi lớp nhận kết quả của lớp trước

Ta có:

~~~text
x0 = biểu diễn ban đầu

x1 = lớp 1 xử lý x0
x2 = lớp 2 xử lý x1
x3 = lớp 3 xử lý x2
...
~~~

Mỗi lớp không bắt đầu lại từ token gốc. Nó nhận trạng thái đã được xử lý bởi lớp trước và có thể xây tiếp trên những cấu trúc đã được tạo ra.

## Sự kết hợp nhiều bước

Khái niệm này thường được gọi là **sự kết hợp nhiều bước (composition)**.

Ta có thể hình dung bằng một bài toán hình học đơn giản:

~~~text
điểm
↓
đường
↓
hình
↓
cấu trúc lớn hơn
~~~

Mỗi bước tạo ra một đối tượng giàu cấu trúc hơn mà bước sau có thể sử dụng.

Trong Transformer, ta không nên gán mỗi lớp một nhiệm vụ cứng như trên.

Nhưng ý tưởng composition vẫn hữu ích:

> **nhiều phép biến đổi nối tiếp có thể tạo ra chức năng phong phú hơn một phép biến đổi duy nhất.**

## Chiều sâu là gì?

Số lớp nối tiếp nhau tạo ra **chiều sâu (depth)** của mạng.

Nhiều lớp tạo thêm cơ hội để trộn thông tin, cập nhật trạng thái, tạo đặc trưng trung gian và kết hợp kết quả của những phép biến đổi trước.

~~~text
đầu vào
↓
biến đổi 1
↓
trạng thái trung gian
↓
biến đổi 2
↓
...
↓
trạng thái cuối
~~~

Chiều sâu không tự động đảm bảo mô hình tốt hơn trong mọi trường hợp; nó chỉ mở khả năng thực hiện chuỗi biến đổi dài hơn.
## Có phải lớp thấp học cú pháp, lớp cao học ngữ nghĩa?

Bạn có thể gặp câu kiểu:

> “Lớp thấp học từ và ngữ pháp, lớp cao học ý nghĩa và suy luận.”

Đó có thể là tóm tắt một số khuynh hướng quan sát được, nhưng không nên coi là định luật.

Thông tin có thể xuất hiện ở nhiều lớp, phân tán, được tạo rồi suy yếu; cùng một lớp cũng có thể tham gia nhiều chức năng.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> Tránh sơ đồ cứng kiểu `lớp 1 = chữ`, `lớp 2 = ngữ pháp`, `lớp 3 = tri thức`. Nó quá đẹp để đại diện cho toàn bộ hệ thống.
## Cách nhìn tốt hơn: quỹ đạo biểu diễn

Thay vì hỏi:

> “Tri thức nằm ở lớp nào?”

ta có thể hỏi:

> **Biểu diễn thay đổi như thế nào khi đi qua các lớp?**

Ta gọi chuỗi trạng thái đó là một **quỹ đạo biểu diễn (representation trajectory)**.

> **[FIGURE F10 — HERO FIGURE] — Một token identity, nhiều state qua chiều sâu**
>
> ~~~text
> TOKEN ID giữ nguyên
>        │
>        ↓
> x0 → x1 → x2 → x3 → ... → xN
>      trạng thái số thay đổi
> ~~~

Đây là một ý trung tâm của cuốn sách.

Không phải:

> token mang một mẩu tri thức nguyên vẹn đi xuyên model.

Mà gần hơn với:

> **một trạng thái số được biến đổi liên tục qua một quỹ đạo.**
## Một token có “biến dạng” không?

Nếu nói bằng trực giác, có thể nói:

> “Biểu diễn của token đang biến dạng qua các lớp.”

Nhưng cần hiểu đúng: token ID và chữ trên màn hình không đổi. Thứ thay đổi là **trạng thái số gắn với vị trí token trong ngữ cảnh hiện tại**.

F10 chính là hình ta sẽ mang theo tới nửa sau cuốn sách: cùng một danh tính đầu vào, nhưng trạng thái có một đường đi qua chiều sâu.
## Làm sao biết thay đổi nào quan trọng?

Đây là câu hỏi khó hơn.

Ta có thể đo:

- khoảng cách giữa hai trạng thái;
- hướng thay đổi;
- độ lớn thay đổi;
- mức giống nhau;
- khả năng dự đoán một thuộc tính từ trạng thái.

Nhưng một con số thay đổi không tự động có nghĩa:

> “Mô hình vừa học thêm tri thức.”

Hoặc:

> “Lớp này vừa suy luận.”

Ta phải phân biệt:

~~~text
trạng thái thay đổi
≠
ta đã hiểu ý nghĩa của thay đổi
~~~

Đây là cầu nối đầu tiên giữa **quan sát biểu diễn** và **kỷ luật diễn giải**.

Nửa sau cuốn sách sẽ quay lại nguyên tắc này mạnh hơn.

## Nhiều lớp cuối cùng đi tới đâu?

Sau lớp cuối, mô hình có một trạng thái cuối cho vị trí hiện tại.

Trạng thái đó vẫn chỉ là một dãy số.

Người dùng chưa thể đọc nó như văn bản.

Vậy làm sao từ dãy số đó xuất hiện token tiếp theo?

Ta cần một bước cuối:

~~~text
trạng thái cuối
↓
điểm cho mọi token có thể chọn
↓
chọn một token
~~~

Đó là Chương 10.

### Nhớ 3 điều

1. **Nhiều lớp cho phép các phép biến đổi được kết hợp theo chuỗi; lớp sau làm việc trên trạng thái mà lớp trước đã tạo.**
2. **Không nên coi mỗi lớp là một “ngăn chức năng” cố định như cú pháp, ngữ nghĩa hay tri thức.**
3. **Quỹ đạo biểu diễn (representation trajectory) là chuỗi trạng thái số của cùng một vị trí khi đi qua nhiều lớp; đây là cách hữu ích để hỏi token “biến dạng” như thế nào.**

**Tiếp theo: [Chương 10 — Từ trạng thái cuối tới token tiếp theo](10-tu-trang-thai-cuoi-toi-token-tiep-theo.md)**
