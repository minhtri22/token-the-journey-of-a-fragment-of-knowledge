# Chương 6 — Cơ chế chú ý: token nhìn những token khác như thế nào?

> **Mức đọc: Đi sâu**
>
> **Ta đang mở chiếc hộp nào?**
>
> ~~~text
> trạng thái hiện tại
>       ↓
> [ cơ chế chú ý ]
>       ↓
> trạng thái có thêm
> thông tin từ ngữ cảnh
> ~~~
>
> Chương này xây trực giác về **cơ chế chú ý (attention)**. Công thức sẽ chỉ xuất hiện ở mức tối thiểu cần thiết để hiểu ba vai trò: truy vấn, khóa và giá trị.

Hãy đọc câu:

~~~text
Nam đặt chiếc cốc lên bàn rồi nhấc nó lên.
~~~

Khi tới từ “nó”, ta tự nhiên hỏi:

> “Nó” đang chỉ cái gì?

Trong câu này, “chiếc cốc” là ứng viên hợp lý hơn “bàn”.

Một mô hình ngôn ngữ cũng cần một cơ chế để trạng thái tại vị trí hiện tại sử dụng thông tin từ những vị trí trước.

Cơ chế chú ý là một phần quan trọng của việc đó.

## “Nhìn” chỉ là cách nói

Khi ta nói:

> token này “nhìn” token kia,

đừng hiểu là có một con mắt hay một thao tác đọc chữ theo nghĩa con người.

Thực tế là những dãy số được đưa vào các phép toán để tạo ra mức độ liên quan rồi trộn thông tin.

Từ “nhìn” chỉ giúp ta xây trực giác, dễ hình dung.

## Ba vai trò: tôi đang cần gì, ai có thể phù hợp, và họ mang gì?

Hãy tưởng tượng bạn đang ở thư viện.

Bạn cần tìm tài liệu về:

~~~text
lịch sử Hà Nội
~~~

Bạn có một câu hỏi.

Các cuốn sách có nhãn.

Và mỗi cuốn chứa nội dung.

Ta có thể dùng phép so sánh:

> **[FIGURE F07] — Attention: Query / Key / Value và dòng thông tin**
>
~~~text
câu hỏi đang tìm gì?
→ truy vấn

nhãn của từng nguồn
→ khóa

nội dung có thể lấy về
→ giá trị
~~~

Trong attention, ba vai trò đó được gọi là:

- **truy vấn (query)**;
- **khóa (key)**;
- **giá trị (value)**.

Thường viết tắt là:

~~~text
Q = query
K = key
V = value
~~~

## Quay lại câu “chiếc cốc”

Ta có:

~~~text
Nam | đặt | chiếc | cốc | lên | bàn | rồi | nhấc | nó | lên
~~~

Khi xử lý vị trí “nó”, ta có thể hình dung trạng thái hiện tại tạo ra một **truy vấn**.

Các vị trí trước có những **khóa**.

Mô hình so sánh truy vấn hiện tại với các khóa để tạo ra các điểm liên quan.

~~~text
                  vị trí hiện tại
                       [nó]
                         │
                      truy vấn
                         │
        ┌────────────────┼────────────────┐
        ↓                ↓                ↓
      [cốc]            [bàn]            [Nam]
       khóa              khóa             khóa
        │                │                │
     điểm cao?        điểm thấp?       điểm khác?
~~~

Sau đó các điểm được dùng để quyết định lượng thông tin nào từ **giá trị** của từng vị trí được đưa về trạng thái hiện tại.

## Khóa không phải giá trị

Đây là chi tiết dễ nhầm.

Một vị trí không chỉ có một dãy số duy nhất làm tất cả mọi việc trong attention.

Từ trạng thái của mỗi vị trí, mô hình tạo ra những cách nhìn khác nhau phục vụ các vai trò khác nhau.

Có thể hình dung:

~~~text
trạng thái của token
      │
      ├→ truy vấn Q
      ├→ khóa K
      └→ giá trị V
~~~

Các phép biến đổi này dùng trọng số đã học.

Truy vấn và khóa chủ yếu tham gia vào câu hỏi:

> “Mức độ liên quan giữa hai vị trí là bao nhiêu?”

Giá trị tham gia vào câu hỏi:

> “Nếu vị trí này được xem là liên quan, phần thông tin nào sẽ được đưa sang?”

## Điểm chú ý

Mô hình tạo một **điểm chú ý (attention score)** giữa truy vấn hiện tại và các khóa có thể nhìn tới.

> **MINH HỌA — Illustration**
>
> ~~~text
> "nó" so với "cốc"  →  4,2
> "nó" so với "bàn"  →  1,1
> "nó" so với "Nam"  →  0,5
> ~~~

Các số chỉ để minh họa. Ý chính là: điểm cao hơn có thể làm vị trí đó đóng góp nhiều hơn. Nhưng điểm thô chưa phải trọng số cuối cùng.
## Từ điểm số sang trọng số

Các điểm thường được đưa qua **softmax**, biến một nhóm điểm thành các trọng số dương có tổng bằng 1.

> **MINH HỌA — Illustration**
>
> ~~~text
> cốc   4,2                 cốc   0,90
> bàn   1,1   → softmax →   bàn   0,06
> Nam   0,5                 Nam   0,04
> ~~~

Sau đó các **giá trị (V)** được trộn theo những trọng số đó.

~~~text
0,90 × giá trị(cốc)
+
0,06 × giá trị(bàn)
+
0,04 × giá trị(Nam)
↓
thông tin đưa về vị trí "nó"
~~~
## Token hiện tại có được nhìn tương lai không?

Trong mô hình sinh văn bản kiểu chỉ dùng bộ giải mã, vị trí hiện tại thường không được phép dùng những token ở tương lai.

Nếu chuỗi hiện tại là:

~~~text
Hà Nội là thủ đô của
~~~

mô hình chưa được nhìn token “Việt” nếu “Việt” chính là thứ nó đang cần dự đoán.

Ta có thể hình dung một mặt nạ:

~~~text
vị trí hiện tại: [của]

được nhìn:
[Hà] [Nội] [là] [thủ] [đô] [của]

không được nhìn:
[Việt] [Nam] ...
~~~

Quy tắc này được gọi là **che nhân quả (causal masking)**.

Tên “nhân quả” ở đây nói về hướng thời gian trong chuỗi: token hiện tại chỉ dùng phần đã có trước đó để dự đoán phần tiếp theo.

Nó không có nghĩa mô hình đã chứng minh quan hệ nhân quả trong thế giới thật.

## Một đầu chú ý có đủ không?

Trong thực tế, attention thường có nhiều **đầu chú ý (attention heads)**.

~~~text
cùng trạng thái đầu vào
        │
        ├→ đầu 1
        ├→ đầu 2
        ├→ đầu 3
        └→ ...
              ↓
        kết hợp lại
~~~

Không nên gắn nhãn cứng kiểu “đầu 1 = ngữ pháp, đầu 2 = địa lý”. Chức năng có thể phân tán và phụ thuộc ngữ cảnh.

> **SIDEBAR — GQA và MQA**
>
> Mô hình nhiều đầu cơ bản thường được giải thích bằng các đầu Q/K/V. Nhưng không phải mọi LLM hiện đại đều có số query heads và key/value heads bằng nhau.
>
> **Multi-Query Attention (MQA)** cho nhiều query heads dùng chung key/value. **Grouped-Query Attention (GQA)** chia query heads thành nhóm và dùng ít key/value heads hơn số query heads.
>
> Mục đích thực dụng quan trọng là giảm lượng key/value cần lưu và xử lý, đặc biệt với **KV cache** khi sinh văn bản.
>
> Cuốn sách không đi sâu vào GQA/MQA; mô hình Q/K/V cơ bản vẫn đủ để hiểu cơ chế chú ý ở mức của chương này.
## Attention có phải lời giải thích cho “mô hình đang nghĩ gì” không?

Không nên nói như vậy.

Trọng số chú ý cho ta biết một phần cách thông tin được trộn trong một phép tính cụ thể. Nhưng toàn mô hình còn có nhiều đầu, nhiều lớp, FFN, residual và chuẩn hóa.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> ~~~text
> trọng số chú ý
> ≠
> toàn bộ lời giải thích cho dự đoán
> ~~~

Một bản đồ attention có thể hữu ích, nhưng không tự động trở thành bằng chứng đầy đủ về ý nghĩa, nguyên nhân hay “suy nghĩ” của mô hình.
## Attention làm xong thì sao?

Sau attention, trạng thái của mỗi vị trí đã nhận thêm thông tin từ những vị trí khác.

Nhưng lớp Transformer chưa kết thúc.

Trạng thái đó còn đi qua một nhánh biến đổi khác.

Nếu attention giống bước:

> “Tôi nên lấy thông tin nào từ những vị trí khác?”

thì nhánh tiếp theo gần với câu hỏi:

> “Sau khi có thông tin đó, tôi biến đổi trạng thái của vị trí này như thế nào?”

Đó là Chương 7.

### Nhớ 3 điều

1. **Cơ chế chú ý (attention) cho phép một vị trí trộn thông tin từ những vị trí khác thông qua truy vấn (query), khóa (key) và giá trị (value).**
2. **Điểm chú ý được biến thành trọng số để quyết định lượng thông tin được lấy từ từng vị trí; trong mô hình sinh văn bản, vị trí hiện tại không được nhìn token tương lai.**
3. **Trọng số chú ý không phải lời giải thích hoàn chỉnh cho “mô hình đang nghĩ gì”.** Nó chỉ là một phần của một chuỗi biến đổi lớn hơn.

**Tiếp theo: [Chương 7 — Nhánh biến đổi tín hiệu: điều gì xảy ra sau cơ chế chú ý?](07-nhanh-bien-doi-tin-hieu.md)**
