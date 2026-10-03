# Chương 12 — Một phép tính có thể trở thành nhiều lần giao việc cho phần cứng

> **Mức đọc: Đi sâu**
>
> **Ta đang mở quan hệ này:**
>
> ~~~text
> 1 phép tính logic
>         ↓
> [ chia nhỏ để thực thi ]
>         ↓
> nhiều lần giao việc vật lý
> ~~~
>
> Chương này không hỏi phép tính “nghĩa là gì”. Nó hỏi một câu thấp hơn: **vì sao một công việc logic có thể phải được chia thành nhiều phần khi chạy thật?**

Hãy tưởng tượng bạn cần sơn một bức tường rất lớn.

Trong kế hoạch, đó là một công việc:

~~~text
Sơn bức tường phía đông.
~~~

Nhưng khi làm thật, bạn có thể chia:

~~~text
ô 1 | ô 2 | ô 3 | ô 4 | ...
~~~

Nhiều người cùng làm.

Hoặc một người làm từng phần.

Công việc logic vẫn là “sơn bức tường”.

Cách thực thi lại được chia thành nhiều phần vật lý.

## GPU cũng cần chia công việc

GPU có nhiều đơn vị tính toán hoạt động song song. Để tận dụng chúng, một phép tính lớn thường được chia theo một **hình học thực thi (execution geometry)**.

> **Hình học thực thi là cách một bài toán được chia thành các nhóm công việc để phần cứng xử lý.**

> **[FIGURE F14] — Một logical operation → nhiều physical dispatches**
>
> ~~~text
> PHÉP TÍNH LOGIC
>       ↓
>  phân rã vật lý
>   ↙   ↓   ↘
> d1   d2   ... dN
> ~~~

Cách chia cụ thể phụ thuộc thuật toán, phần cứng, kích thước dữ liệu và cách hệ thực thi được viết.
## Lớp đầu ra là một ví dụ rất rõ

Ở Chương 10, **LM head** tạo điểm cho toàn bộ từ vựng. Ở mức logic, đó là một công việc.

> **KẾT QUẢ ĐO — Measured Result `[E-DECOMP-01]`**
>
> Trong trace mục tiêu:
>
> ~~~text
> 1 LM-head logical operation
> ↓
> 19 dispatch vật lý
> ~~~

Đây là bằng chứng trực tiếp cho quan hệ **một logic → nhiều physical dispatches** trong hệ thống đã đo.
## Có phải vì từ vựng bị chia thành 19 phần?

Không nên tự suy ra như vậy chỉ từ con số 19.

Việc phân rã có thể phụ thuộc giới hạn kích thước công việc, cách chia hàng/cột, thuật toán, tổ chức bộ nhớ, kernel hoặc lựa chọn tối ưu cho phần cứng cụ thể.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> Quan sát `1 logical operation → 19 dispatches` cho ta **cấu trúc ánh xạ đã đo**. Nó chưa tự giải thích **nguyên nhân thiết kế** tạo ra ánh xạ đó.
## Chia nhỏ có phải lúc nào cũng xấu?

Không. Nhiều dispatch hơn không tự động nghĩa chậm hơn.

Chia nhỏ có thể giúp tăng song song, phù hợp giới hạn phần cứng hoặc xử lý dữ liệu lớn hơn. Nhưng chia quá nhỏ cũng có thể làm tăng chi phí gửi việc, đồng bộ và di chuyển dữ liệu.

Vì vậy:

> **Số dispatch là một đặc điểm thực thi, không phải phán quyết hiệu năng.**
## Một ví dụ bằng vận chuyển

Giả sử cần chuyển 10 tấn hàng. Một xe lớn chạy một chuyến chưa chắc nhanh hơn nhiều xe nhỏ chạy song song.

Chỉ nhìn số chuyến không đủ; còn phụ thuộc tải trọng, thời gian xếp hàng, khả năng chạy song song và chi phí chờ.

GPU cũng vậy: số dispatch chỉ có nghĩa khi đặt trong toàn bộ cấu trúc thực thi.
## Phân rã vật lý

Ta gọi việc biến một phép tính logic thành nhiều công việc thực thi là **phân rã vật lý (physical decomposition)**.

~~~text
PHÉP TÍNH LOGIC
      │
      ↓
phân rã để thực thi
      │
      ├→ dispatch 1
      ├→ dispatch 2
      └→ ...
~~~

Nếu không giữ riêng danh tính logic, ta có thể nhìn 19 dispatch và tưởng đó là 19 phép tính logic khác nhau.
## Khi cộng thời gian thì cộng thế nào?

Nếu một phép tính logic có nhiều dispatch, ta chỉ được cộng thời gian khi quy tắc đo và quy chiếu đã xác định rõ dispatch nào thuộc về phép tính đó.

Trong hệ thống thật còn có thể có chồng lấn, khoảng trống và đồng bộ. Vì vậy tổng thời gian dispatch được quy chiếu **không tự động bằng** toàn bộ thời gian hệ thống xung quanh phép tính.

Phép đo dùng trong sách khóa attribution theo trace, nên ta có thể nói thời gian dispatch được quy về operation nào; ta không gán mọi khoảng trống cho operation đó.
## Và chiều ngược lại?

Ta vừa thấy:

~~~text
1 phép tính logic
→
nhiều dispatch
~~~

Nhưng liệu có thể có:

~~~text
nhiều phép tính logic
→
1 dispatch
~~~

Có.

Khi hệ thống **gộp phép tính (fusion)**, nhiều công việc logic có thể được thực hiện trong cùng một chương trình GPU.

Đó là Chương 13.

### Nhớ 3 điều

1. **Một phép tính logic lớn có thể được phân rã thành nhiều lần giao việc vật lý để phù hợp với cách GPU thực thi.**
2. **Trong một phép đo thật, một LM head logic được thực thi bằng 19 dispatch; con số này chứng minh quan hệ một-nhiều trong hệ thống đó, không phải quy luật cho mọi mô hình.**
3. **Nhiều dispatch hơn không tự động nghĩa chậm hơn, và thấy một phép tính bị chia chưa tự động giải thích vì sao hệ thực thi chọn cách chia đó.**

**Tiếp theo: [Chương 13 — Khi nhiều phép tính được gộp thành một công việc](13-khi-nhieu-phep-tinh-duoc-gop.md)**
