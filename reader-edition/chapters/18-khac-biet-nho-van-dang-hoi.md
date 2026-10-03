# Chương 18 — Khi một khác biệt quá nhỏ vẫn đáng để hỏi

> **Mức đọc: Nghiên cứu**
>
> **Ta đang đổi câu hỏi:**
>
> ~~~text
> hai giá trị gần như giống nhau
>          ↓
>      "coi như bằng?"
>          ↓
> hay phần chênh lệch đó
>      có điều gì đáng hỏi?
> ~~~
>
> Chương này không dạy rằng mọi sai khác nhỏ đều quan trọng. Nó dạy một kỷ luật đơn giản hơn: **đừng gọi một sai khác là “vô nghĩa” trước khi biết nó nhỏ so với cái gì.**

Hãy nhìn hai số:

~~~text
A = 1,000000
B = 1,000003
~~~

Ta có thể nói:

~~~text
A ≈ B
~~~

Điều đó có thể hoàn toàn hợp lý.

Nhưng ta cũng có thể tính:

~~~text
B - A = 0,000003
~~~

Ba phần triệu — rất nhỏ.

Câu hỏi là:

> **Nhỏ đến mức có thể bỏ qua, hay nhỏ nhưng có cấu trúc?**

Hai câu này không giống nhau.

## Sai khác là gì?

Nếu có hai giá trị `A` và `B`, ta có thể tính **sai khác (difference)**:

~~~text
Δ = B - A
~~~

Nếu hai giá trị gần nhau, Δ sẽ nhỏ. Trong một số ngữ cảnh, phần còn lại sau khi trừ có thể được gọi là **phần dư / khác biệt dư (residual difference)**.

Đừng nhầm nó với **đường cộng tắt (residual connection)** ở Chương 8: một bên là kiến trúc mạng, một bên là phần chênh giữa hai đại lượng.
## Nhỏ so với cái gì?

Một khác biệt chỉ có ý nghĩa khi đặt cạnh khả năng phân biệt của phép đo.

**Độ phân giải (resolution)** là mức chi tiết nhỏ nhất mà phép đo có thể phân biệt đáng tin.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> Không quan sát thấy khác biệt không tự động nghĩa không có khác biệt. Ngược lại, quan sát thấy một khác biệt cực nhỏ cũng chưa chắc đó là tín hiệu thật.
## Nhiễu cũng tạo ra sai khác

Phép đo thực tế có thể chịu ảnh hưởng của nhiễu, làm tròn, sai số số học, nhiệt độ, trạng thái hệ thống, cách lấy mẫu hoặc giới hạn dụng cụ.

Ta gọi vùng dao động nền thường gặp là **mức nhiễu (noise floor)**.

> **[FIGURE F22] — Sai khác nhỏ → độ phân giải → nhiễu → lặp lại → cấu trúc**
>
> ~~~text
> Δ quan sát được
>      ↓
> so với độ phân giải?
>      ↓
> so với mức nhiễu?
>      ↓
> có lặp lại?
>      ↓
> có cấu trúc?
> ~~~
## Lặp lại là câu hỏi đầu tiên

Nếu Δ đổi dấu và kích thước thất thường giữa các lần chạy, nó trông giống nhiễu hơn. Nếu cùng hướng và gần cùng độ lớn qua nhiều lần, nó đáng để hỏi tiếp.

Khả năng một hiện tượng xuất hiện lại khi lặp phép đo được gọi là **khả năng lặp lại (repeatability)**.

Lặp lại chưa chứng minh nguyên nhân; nó chỉ giúp tách ứng viên tín hiệu khỏi dao động ngẫu nhiên.
## Cấu trúc quan trọng hơn chỉ riêng độ lớn

Một chuỗi sai khác nhỏ nhưng thay đổi có hệ thống theo lớp, vị trí, thời gian hay điều kiện khác với một chuỗi dao động hỗn loạn.

Ta có thể hỏi:

- sai khác có phụ thuộc lớp không?
- có giữ cùng hướng qua nhiều lần chạy không?
- khi đầu vào thay đổi nhẹ, sai khác có phản ứng theo quy luật không?

Một khuôn mẫu có cấu trúc biến khác biệt nhỏ thành **câu hỏi khoa học đáng kiểm tra**, chưa biến nó thành kết luận.
## Một sơ đồ để nghĩ về “biến dạng”

Ở Chương 9, ta đã có **quỹ đạo biểu diễn (representation trajectory)**:

~~~text
x0 → x1 → x2 → ... → xN
~~~

Nếu có hai điều kiện gần nhau:

~~~text
A: x0 → x1 → x2 → ... → xN
B: y0 → y1 → y2 → ... → yN
~~~

ta có thể theo dõi `Δi = yi - xi` qua chiều sâu.

Câu hỏi không chỉ là **Δ có nhỏ không**, mà là **Δ thay đổi thế nào trên đường đi**: biến mất, ổn định, tăng dần, đổi hướng hay lan sang nhiều chiều.
## Nhưng “có cấu trúc” vẫn chưa phải “có ý nghĩa”

Một sai khác có thể lặp lại, tăng theo lớp và phụ thuộc đầu vào mà vẫn chưa cho phép nói:

> “Đây chính là tri thức.”

hay:

> “Đây là cơ chế gây ra hành vi X.”

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> **Có cấu trúc** mạnh hơn nhiễu ngẫu nhiên, nhưng vẫn còn cách kết luận nhân quả một khoảng lớn.
## Một trình tự an toàn

~~~text
có sai khác
    ↓
so với độ phân giải?
    ↓
so với mức nhiễu?
    ↓
có lặp lại?
    ↓
có structure?
    ↓
có phản ứng khi điều kiện thay đổi?
    ↓
mới đặt giả thuyết mạnh hơn
~~~

Đừng nhảy từ `Δ ≠ 0` sang “đã tìm thấy tín hiệu”, cũng đừng nhảy từ “Δ rất nhỏ” sang “chắc chắn vô nghĩa”.
## Một con số nhỏ có thể là gì?

Khi thấy phần chênh cực nhỏ, ít nhất có nhiều khả năng:

- sai số số học;
- nhiễu phép đo;
- giới hạn độ phân giải;
- khác biệt trạng thái thật nhưng không quan trọng;
- khác biệt thật và có cấu trúc;
- dấu hiệu của một cơ chế chưa hiểu.

Không thể chọn một câu chỉ vì nó thú vị nhất.

Phải để bằng chứng loại dần các khả năng.

## Từ sai khác tới nguyên nhân

Chương này dừng ở đây.

Ta đã có thể hỏi:

> “Sai khác có thật và có cấu trúc không?”

Nhưng câu tiếp theo khó hơn:

> “Nếu có cấu trúc, cái gì gây ra nó?”

Ở đó, một khuôn mẫu đẹp không còn đủ.

Ta phải bước vào câu chuyện của **tương quan và nhân quả**.

### Nhớ 3 điều

1. **Một sai khác rất nhỏ không tự động vô nghĩa; trước hết phải so nó với độ phân giải, mức nhiễu và khả năng lặp lại của phép đo.**
2. **Cấu trúc theo lớp, vị trí, thời gian hoặc điều kiện có thể biến một sai khác nhỏ thành câu hỏi đáng nghiên cứu.**
3. **Có cấu trúc chưa có nghĩa đã biết nguyên nhân.** Sai khác là điểm bắt đầu của một giả thuyết, không phải kết luận cuối.

**Tiếp theo: [Chương 19 — Một khuôn mẫu đẹp vẫn chưa phải nguyên nhân](19-khuon-mau-dep-chua-phai-nguyen-nhan.md)**
