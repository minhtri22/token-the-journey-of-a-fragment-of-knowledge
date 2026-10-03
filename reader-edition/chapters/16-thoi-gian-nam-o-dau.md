# Chương 16 — Thời gian nằm ở đâu?

> **Mức đọc: Nghiên cứu**
>
> **Ta đã có:**
>
> ~~~text
> dispatch
> + timestamp
> + danh tính logic
>       ↓
> [ cộng thời gian theo từng họ phép tính ]
>       ↓
> điểm nóng
> ~~~
>
> Chương này trả lời một câu hỏi rất cụ thể: **trong một bước sinh token được đo, phần nào đang chiếm nhiều thời gian nhất?**

Hãy tưởng tượng một ngày làm việc dài 8 giờ.

Nếu muốn tối ưu thời gian, ta có thể ghi:

~~~text
họp          3 giờ
viết báo cáo 2 giờ
đọc email    1 giờ
di chuyển    1 giờ
khác         1 giờ
~~~

Ta lập tức biết nơi thời gian tập trung.

Trong hệ thống tính toán, các vùng như vậy được gọi là **điểm nóng (hotspot)**.

## Từ 469 dispatch tới vài nhóm dễ hiểu hơn

Đọc riêng 469 dispatch sẽ rất khó. Sau quy chiếu, ta có thể cộng thời gian theo các họ phép tính logic.

> **KẾT QUẢ ĐO — Measured Result `[E-HOTSPOT-01]`**
>
> Trong dấu vết mục tiêu, bốn họ lớn nhất chiếm:
>
> ~~~text
> LM head   63.9109%
> FFN-down  16.2659%
> FFN-up     6.0136%
> FFN-gate   5.9812%
> ----------------
> top four  92.1715%
> ~~~
>
> Các tỷ lệ là phần của **tổng thời gian dispatch đã đo** trong dấu vết đó.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> Đây không phải profile chung cho mọi model, runtime, phần cứng hay lần chạy.
## Nhìn bằng một biểu đồ

> **[FIGURE F19] — Phân bố điểm nóng theo thứ hạng**
>
> ~~~text
> LM head    |████████████████████████████████| ~63.9%
> FFN-down   |████████                        | ~16.3%
> FFN-up     |███                             | ~ 6.0%
> FFN-gate   |███                             | ~ 6.0%
> khác       |████                            | ~ 7.8%
> ~~~

Hình có thể làm tròn để nhìn nhanh; Evidence Note giữ exact values.
## Đây gọi là định vị chi phí

Việc trả lời **“thời gian nằm ở đâu?”** là một dạng **định vị chi phí (localization)**.

~~~text
toàn bộ đường sinh token
        ↓
đo + quy chiếu
        ↓
nhóm theo họ phép tính
        ↓
xếp theo tỷ trọng
        ↓
điểm nóng
~~~

Định vị giúp tránh tối ưu theo trực giác khi chưa biết phần nào thật sự chiếm thời gian.
## Nhưng 63.9109% có nghĩa gì?

Nó có nghĩa chính xác rằng: **trong dấu vết mục tiêu, các dispatch được quy chiếu về LM head chiếm 63.9109% tổng thời gian dispatch đã đo**.

Nó không tự động có nghĩa LM head luôn có tỷ lệ đó, không chứng minh LM head bị memory-bound, và cũng không nói rằng một thay đổi giả định sẽ giảm tổng độ trễ đúng 63.9109%.

Con số là kết quả định vị của một phép đo cụ thể.
## Hotspot có phải nguyên nhân không?

Không.

Một con đường thường xuyên tắc cho ta biết **nơi** vấn đề xuất hiện; nguyên nhân có thể là đèn giao thông, công trường, đường hẹp hay lưu lượng.

Tương tự:

~~~text
LM head = 63.9109% thời gian dispatch đã đo
~~~

cho biết nơi thời gian tập trung, không nói cơ chế gây chi phí.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> **localization ≠ mechanism**
>
> Hotspot là vị trí ưu tiên điều tra, không phải bằng chứng nhân quả.
## Vì sao định vị vẫn rất có giá trị?

Trước khi có dấu vết, ta có hàng trăm công việc và không biết nên nhìn đâu. Sau dấu vết, bốn họ chiếm **92.1715%** thời gian dispatch đã đo.

Ta vẫn chưa có nguyên nhân, nhưng đã có một **bản đồ ưu tiên** để đặt câu hỏi tiếp theo.
## Amdahl xuất hiện ở đây

Nếu một phần chỉ chiếm tỷ trọng rất nhỏ, tối ưu riêng phần đó chỉ có dư địa giới hạn để cải thiện toàn hệ.

Ngược lại, hotspot lớn có dư địa ảnh hưởng lớn hơn. Đây là trực giác của **định luật Amdahl (Amdahl's law)**.

Ta không cần công thức ở đây. Quan trọng hơn: **tỷ trọng lớn không đồng nghĩa phần đó dễ tối ưu hoặc đã biết cơ chế.**
## Điểm nóng có thể thay đổi

Sau khi một hotspot được tối ưu, phần khác có thể trở thành nút thắt mới.

Vì vậy bản đồ điểm nóng có **thời hạn**. Sau thay đổi đáng kể, cần đo lại thay vì tiếp tục dùng bản đồ cũ như sự thật cố định.
## Từ “ở đâu” sang “vì sao”

Bây giờ ta có một mục tiêu cụ thể:

~~~text
LM head
FFN-down
...
~~~

Muốn hiểu cơ chế, ta có thể hỏi phần cứng:

- có đang đọc nhiều dữ liệu không?
- cache có trúng không?
- đơn vị tính toán có bận không?
- có đang chờ phụ thuộc không?
- tần số có ổn định không?

Một nguồn trả lời là **bộ đếm phần cứng (hardware counters)**.

Nhưng các bộ đếm này cũng có thể thất bại.

Thậm chí chúng có thể trả về số 0.

Đó là câu chuyện của Chương 17.

### Nhớ 3 điều

1. **Điểm nóng (hotspot) là nơi thời gian tập trung; trong trace cụ thể này, LM head ~63,91%, FFN-down ~16,27%, FFN-up ~6,01%, FFN-gate ~5,98%, tổng bốn họ ~92,17% thời gian dispatch.**
2. **Định vị (localization) trả lời “ở đâu”, không trả lời “vì sao”.** Một hotspot lớn chưa phải bằng chứng về cơ chế gây chậm.
3. **Bản đồ hotspot có thể trở nên lỗi thời sau khi hệ thống thay đổi; muốn tiếp tục tối ưu phải đo lại.**

**Tiếp theo: [Chương 17 — Bộ đếm phần cứng: con số đo được có nghĩa gì?](17-bo-dem-phan-cung.md)**
