# Chương 1 — Token không phải là một từ

> **Mức đọc: Nền tảng**
>
> **Ta đang đứng ở đâu?**
>
> ~~~text
> văn bản
>    ↓
> [ chia thành những mảnh nhỏ ]
>    ↓
> token
> ~~~
>
> Chương này chỉ trả lời một câu hỏi: **token là gì và vì sao “một token = một từ” chỉ là cách hiểu tạm thời?**

Hãy bắt đầu bằng một câu rất bình thường:

~~~text
Tôi thích cà phê.
~~~

Nếu được yêu cầu chia câu này thành những mảnh nhỏ, có lẽ ta sẽ làm:

~~~text
Tôi | thích | cà | phê | .
~~~

Đây là một cách rất tốt để bắt đầu hiểu token.

Ở mức đầu tiên, ta có thể nói:

> **Token là một mảnh văn bản nhỏ mà mô hình xử lý.**

Cách hiểu này chưa hoàn toàn chính xác.

Nhưng nó đủ để ta đi bước đầu tiên mà không bị ngợp.

## Vì sao không nói luôn “token là một từ”?

Bởi mô hình không bắt buộc chia văn bản theo cách con người chia từ.

Ví dụ, “ChatGPT” có thể được giữ thành một token hoặc chia thành nhiều mảnh tùy tokenizer. Điều tương tự xảy ra với từ hiếm, tên riêng, số, dấu câu, khoảng trắng, tiếng Việt, tiếng Anh và mã nguồn.

Vì vậy cách nói tốt hơn là:

> **Một token có thể là một từ, một phần của từ, dấu câu hoặc một chuỗi ký tự khác.**

## Ai quyết định cách chia?

Không phải người dùng.

Không phải phần cứng đang chạy mô hình.

Cũng không phải mỗi lớp Transformer tự nghĩ ra cách chia riêng.

Có một thành phần đứng trước mô hình làm việc đó.

Ta gọi nó là **bộ tách và mã hóa văn bản (tokenizer)**.

Tokenizer làm việc với một **từ vựng token (vocabulary)**: tập những đơn vị token và mã tương ứng mà hệ thống có thể sử dụng.

> **[FIGURE F02] — Văn bản → tokenizer → token pieces → token IDs**

Có thể hình dung:

~~~text
"Tôi thích cà phê."
        ↓
bộ tách và mã hóa
        ↓
[token A] [token B] [token C] ...
~~~

Tên tiếng Anh tokenizer rất phổ biến, nên ta giữ lại trong ngoặc khi giới thiệu.

Sau này nếu bạn đọc tài liệu thấy chữ tokenizer, bạn đã biết nó đang nói tới phần nào của quá trình.

## Token không nhất thiết nhìn giống phần chữ ta tưởng tượng

Một token có thể chứa dấu cách ở đầu, một từ có thể bị chia thành nhiều token, và ranh giới token có thể khác cách ta tự chia bằng mắt.

Điều này không phải lỗi. Mục tiêu của tokenizer là tạo ra một hệ đơn vị mà mô hình có thể xử lý nhất quán, không phải những mảnh đẹp nhất cho con người.

## Một ví dụ gần gũi: bộ chữ ghép

Hãy tưởng tượng một hộp mảnh chữ không chứa mọi từ hoàn chỉnh, mà chứa những mảnh có thể ghép lại. Muốn tạo “cà phê rất ngon.”, hệ thống có thể cần nhiều mảnh nhỏ.

Phép so sánh này không mô tả đầy đủ mọi tokenizer hiện đại, nhưng giải thích được một điều quan trọng: **một từ đôi khi cần nhiều token vì hệ thống phải ghép từ những đơn vị có sẵn.**

## Token có ý nghĩa không?

Có.

Và không.

Một token như “Paris” rõ ràng gợi cho con người rất nhiều ý nghĩa.

Nhưng token trong hệ thống trước hết chỉ là **một đơn vị được bộ mã hóa nhận diện**.

Mô hình chưa trực tiếp cầm cả “Paris, nước Pháp, tháp Eiffel, lịch sử, địa lý...” trong một chiếc hộp token.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> ~~~text
> mảnh văn bản
> ≠
> toàn bộ ý nghĩa mà con người liên tưởng tới mảnh đó
> ~~~

Đây là bước đầu tiên để hiểu tên cuốn sách.

Nếu token không tự chứa nguyên một mảnh tri thức, vậy tri thức xuất hiện bằng cách nào?

Chưa vội.

Trước hết còn một bước quan trọng hơn.

## Token sẽ được đổi thành một con số

Máy tính không gửi chữ “Paris” đi xuyên qua hàng chục lớp theo đúng dạng ký tự mà ta đang nhìn.

Mỗi token được gắn với một mã số.

Ta gọi đó là **mã token (token ID)**.

> **MINH HỌA — Illustration**
>
> ~~~text
> "Paris" → 8421
> ~~~
>
> Con số 8421 chỉ là ví dụ, không phải ID được khẳng định cho một tokenizer cụ thể.

Điều cần nhớ là:

~~~text
văn bản
↓
token
↓
mã token
~~~

Nhưng mã token cũng chưa phải thứ các lớp của mô hình thực sự dùng để làm phần lớn phép tính.

Ở chương sau nữa, ta sẽ thấy nó còn phải được đổi thành một biểu diễn bằng số phong phú hơn.

## Vì sao việc chia token quan trọng?

Nếu cùng một đoạn văn được chia thành số token khác nhau, điều đó có thể ảnh hưởng tới độ dài chuỗi, lượng ngữ cảnh chiếm dụng, bộ nhớ, thời gian xử lý và cách các mảnh đầu vào được đưa vào mô hình.

Vì vậy việc chia token không chỉ là “cắt câu cho đẹp”. Nó là một phần của cách văn bản bước vào mô hình.

## Nhưng đừng suy diễn quá xa

Một lỗi khác cũng dễ mắc:

> “Nếu biết cách tokenizer chia câu, ta đã biết mô hình hiểu câu thế nào.”

Không.

Ta mới chỉ biết **cách câu được chia thành đơn vị đầu vào**. Cách những đơn vị đó được biểu diễn và biến đổi về sau là một câu chuyện khác.

~~~text
cách chia token
= cách chia đầu vào

hiểu ý nghĩa
≠ chỉ riêng cách chia đầu vào
~~~

## Câu hỏi nhỏ nhưng rất quan trọng

Quay lại câu:

~~~text
Hà Nội là thủ đô của Việt Nam.
~~~

Nếu token “Hà” hay “Hà Nội” xuất hiện, nó có mang theo tri thức “Hà Nội là thủ đô của Việt Nam” hay không?

Nếu câu trả lời là “có”, tri thức đó nằm ở đâu trong token?

Nếu câu trả lời là “không”, tại sao mô hình vẫn có thể hoàn thành câu “Hà Nội là thủ đô của ...” bằng “Việt Nam”?

Đây là câu hỏi của Chương 2.

### Nhớ 3 điều

1. **Token là một mảnh văn bản mà mô hình xử lý, nhưng không nhất thiết là một từ.**
2. **Bộ tách và mã hóa văn bản (tokenizer) quyết định cách văn bản được chia và gắn mã token.**
3. **Biết token là gì chưa có nghĩa biết “tri thức nằm ở đâu”.** Token mới chỉ là đơn vị đầu vào của câu chuyện.

**Tiếp theo: [Chương 2 — Token cũng không phải là tri thức](02-token-khong-phai-la-tri-thuc.md)**
