# Chương 20 — Từ phép đo tới điều ta thật sự biết

> **Mức đọc: Nâng cao**
>
> Ta bắt đầu bằng một câu hỏi nghe rất đơn giản:
>
> **Một token mang “mảnh tri thức” đi qua mô hình như thế nào?**
>
> Sau 19 chương, câu hỏi đó đã đổi hình dạng.

Ở đầu sách, cách hình dung tự nhiên là:

~~~text
token
=
một chiếc hộp nhỏ
chứa một phần ý nghĩa
~~~

Rồi chiếc hộp đó đi xuyên qua mô hình và cuối cùng tạo ra câu trả lời.

Bây giờ ta biết bức tranh thật phức tạp hơn.

## Đường thứ nhất: bên trong mô hình

Ta đã đi qua:

~~~text
văn bản
↓
token
↓
mã token
↓
biểu diễn ban đầu
↓
ngữ cảnh
↓
attention / FFN / residual
↓
nhiều lớp
↓
quỹ đạo biểu diễn
↓
trạng thái cuối
↓
logits
↓
token tiếp theo
~~~

Không có bước nào cho ta một chiếc hộp ghi `TRI THỨC`. Thứ ta thấy là **một quá trình biến đổi trạng thái**.
## Token không phải tri thức

Token là đơn vị đầu vào. Token ID là danh tính. Biểu diễn ban đầu là trạng thái số đã học. Ngữ cảnh và nhiều lớp tiếp tục biến đổi trạng thái đó, dưới tác động của trọng số mô hình.

> **Không có bằng chứng nào trong hành trình này cho phép ta coi một token đơn lẻ là một viên tri thức hoàn chỉnh.**
## Vectơ cũng không phải toàn bộ tri thức

Ta cũng không nên thay một cực đoan bằng cực đoan khác:

~~~text
token không chứa tri thức
↓
vậy một vectơ nào đó chắc chứa tri thức
~~~

Một trạng thái ở một lớp chỉ là một lát cắt của quá trình. Thông tin có thể phân tán trên nhiều chiều, phụ thuộc ngữ cảnh, được tạo qua nhiều layer và chỉ bộc lộ hành vi khi kết hợp với những phép biến đổi tiếp theo.

> **Tìm thấy một biểu diễn liên quan tới một khái niệm chưa đủ để nói tri thức “nằm trong” biểu diễn đó.**
## Một cách nói thận trọng hơn

Ta có thể mô tả ở mức kỹ thuật:

~~~text
trọng số đã học
      +
cấu trúc mô hình
      +
ngữ cảnh hiện tại
      +
trạng thái trung gian
      +
chuỗi biến đổi
      ↓
hành vi dự đoán
~~~

Sơ đồ này không phải định nghĩa triết học về tri thức. Nó chỉ giữ một ranh giới thực dụng: **những gì mô hình biểu hiện không nhất thiết nằm gọn ở một token, neuron, chiều hay layer duy nhất.**
## Đường thứ hai: bên ngoài mô hình

Từ Chương 11, ta đổi góc nhìn:

~~~text
phép tính logic
↓
kernel / dispatch
↓
bộ nhớ / thực thi
↓
dấu vết / dấu thời gian
↓
counter
↓
bằng chứng
~~~

Ở đây xuất hiện một loại hiểu biết khác: **tri thức của người quan sát về mô hình và hệ thực thi**.
## Tri thức của người quan sát không đến thẳng từ con số

~~~text
HIỆN TƯỢNG
    ↓
KÊNH ĐO
    ↓
SỐ LIỆU
    ↓
ĐỘ ĐẦY ĐỦ PHÉP ĐO
    ↓
BẰNG CHỨNG
    ↓
DIỄN GIẢI
    ↓
KẾT LUẬN TRONG PHẠM VI
~~~

Nếu kênh đo không đủ, con số không đủ. Nếu bằng chứng chỉ định vị điểm nóng, ta không được viết cơ chế. Nếu `counter = 0` nhưng độ đầy đủ phép đo FAIL, ta không được gọi đại lượng vật lý bằng 0. Nếu khuôn mẫu đẹp nhưng chưa có phép thử đủ phân biệt, ta không được viết kết luận nhân quả.
## Hai hành trình gặp nhau

> **[FIGURE F24] — Final synthesis: model trajectory + observer trajectory**
>
> ~~~text
> BÊN TRONG MÔ HÌNH          PHÍA NGƯỜI QUAN SÁT
>
> token                      hiện tượng
>   ↓                           ↓
> trạng thái                  phép đo
>   ↓                           ↓
> biến đổi                    bằng chứng
>   ↓                           ↓
> hành vi                     điều ta biết
> ~~~

Một bên là quá trình mô hình tạo và biến đổi trạng thái. Một bên là quá trình con người xây hiểu biết đáng tin về quá trình đó.

Tên **Đường đi của mảnh tri thức** vì vậy có hai nghĩa — nhưng không nghĩa nào biến token thành một chiếc hộp chứa tri thức bất biến.
## Nghĩa thứ nhất: thông tin được biến đổi

Token không mang nguyên một mảnh tri thức bất biến. Nó tham gia vào chuỗi biến đổi nơi danh tính token, vị trí, ngữ cảnh và trọng số đã học cùng ảnh hưởng tới trạng thái.

Cuốn sách theo dấu sự biến đổi đó.
## Nghĩa thứ hai: bằng chứng trở thành hiểu biết

Một phép đo cũng không phải tri thức hoàn chỉnh.

~~~text
quan sát
↓
kiểm tra
↓
đối chứng
↓
giới hạn
↓
diễn giải
~~~

Chỉ khi nguồn gốc và ranh giới của bằng chứng được giữ rõ, kết luận mới đáng tin hơn.
## “Không biết” cũng là một trạng thái tri thức

Khoa học không chỉ có `đúng` và `sai`. Nó còn có:

~~~text
CHƯA ĐỦ BẰNG CHỨNG
CHƯA GIẢI QUYẾT
NẰM NGOÀI PHẠM VI ĐÃ ĐO
~~~

Những trạng thái này ngăn ta biến lỗ hổng hiểu biết thành một câu chuyện tròn trịa nhưng chưa được chứng minh.

Part IV đã có một ví dụ thật:

> **UNRESOLVED / no confirmed mechanism**

Đó là nơi bằng chứng hiện tại dừng lại.
## Quay lại ví dụ đầu tiên

Ta bắt đầu với:

~~~text
Hà Nội là thủ đô của ...
~~~

rồi mô hình sinh `Việt`, sau đó `Nam`.

Lúc đầu ta có thể tưởng token “Hà Nội” mang sẵn mẩu tri thức “thủ đô Việt Nam”. Sau hành trình này, cách hỏi thận trọng hơn là:

> Những trọng số đã học, ngữ cảnh và quỹ đạo biểu diễn đã tương tác thế nào để trạng thái cuối ưu tiên `Việt`, rồi `Nam`?

Đó là câu hỏi khó hơn, nhưng phù hợp hơn với những gì ta đã quan sát.
## Cuốn sách dừng ở đâu?

Cuốn sách này dừng tại ranh giới mà bằng chứng hiện có cho phép.

Ta đã học cách theo dấu biểu diễn, thực thi và phép đo; học cách giữ **định vị** tách khỏi **cơ chế**; và học rằng một khuôn mẫu mạnh vẫn có thể kết thúc ở `UNRESOLVED`.

Những câu hỏi xa hơn có thể tồn tại, nhưng chúng **không được viết thành kết luận của cuốn sách này khi chuỗi bằng chứng chưa hội tụ**.

Điều cuốn sách giữ lại là một kỷ luật:

> **Theo dấu trước. Đo trước. Phân biệt bằng chứng với diễn giải. Chỉ nói tới nơi dữ liệu cho phép.**
## Bản đồ cuối cùng

F24 là bản đồ cuối:

~~~text
                 BÊN TRONG MÔ HÌNH

văn bản
  ↓
token
  ↓
biểu diễn
  ↓
ngữ cảnh + trọng số
  ↓
biến đổi trạng thái
  ↓
token tiếp theo


                 PHÍA NGƯỜI QUAN SÁT

hiện tượng
  ↓
dấu vết / phép đo
  ↓
độ đầy đủ của phép đo
  ↓
bằng chứng
  ↓
diễn giải
  ↓
điều ta thật sự biết
~~~

Hai đường gặp nhau ở một nguyên tắc:

> **Đừng nhầm biểu diễn với bản chất. Đừng nhầm phép đo với thực tại.**

### Nhớ 3 điều

1. **Token không phải một viên tri thức.** Điều mô hình biểu hiện xuất hiện từ tương tác giữa trọng số đã học, ngữ cảnh và chuỗi biến đổi trạng thái.
2. **Một phép đo cũng không phải tri thức hoàn chỉnh.** Nó chỉ trở thành bằng chứng hữu ích khi nguồn gốc, độ đầy đủ và ranh giới diễn giải được giữ rõ.
3. **UNRESOLVED cũng là một kết quả có giá trị.** Biết chính xác mình chưa biết gì tốt hơn một lời giải thích đẹp nhưng vượt quá bằng chứng.

---

Ta bắt đầu bằng cách đi theo một token để tìm “mảnh tri thức”.

Cuối cùng, điều ta tìm thấy không phải một vật thể nằm yên trong token, mà là **một chuỗi biến đổi**.

Và song song với nó là một chuỗi khác:

~~~text
quan sát
↓
đo
↓
kiểm tra
↓
bằng chứng
↓
hiểu biết
~~~

Có lẽ đó mới là đường đi quan trọng nhất của “mảnh tri thức” trong cuốn sách này:

> **không chỉ là cách thông tin thay đổi bên trong mô hình, mà còn là cách con người học để biết tới đâu mình thật sự hiểu sự thay đổi ấy.**
