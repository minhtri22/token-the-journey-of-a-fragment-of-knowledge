# Chương 17 — Bộ đếm phần cứng: con số đo được có nghĩa gì?

> **Mức đọc: Nghiên cứu**
>
> **Ta đã biết nơi thời gian tập trung.**
>
> Bây giờ ta muốn hỏi:
>
> ~~~text
> hotspot
>    ↓
> phần cứng đang làm gì?
>    ↓
> [ bộ đếm phần cứng ]
> ~~~

Hãy tưởng tượng bảng đồng hồ trên ô tô.

Nó có thể cho bạn:

- tốc độ;
- vòng tua;
- nhiệt độ;
- mức nhiên liệu.

Những con số đó giúp hiểu chiếc xe đang hoạt động thế nào.

GPU cũng có những phép đo nội bộ tương tự.

Ta thường gọi chúng là **bộ đếm phần cứng (hardware counters)**.

## Bộ đếm có thể đo gì?

Tùy phần cứng và trình điều khiển, **bộ đếm phần cứng (hardware counter)** có thể phản ánh truy cập bộ nhớ, cache, mức sử dụng đơn vị tính toán, trạng thái chờ, tần số, số chu kỳ và nhiều đại lượng khác.

Nhưng có hotspot rồi đọc counter chưa đủ để nói nguyên nhân. Trước hết phải biết con số được tạo ra qua kênh đo nào và kênh đó có đủ đáng tin cho câu hỏi đang hỏi hay không.
## Con số đi qua một kênh đo

> **[FIGURE F20] — Measurement channel: physical event → number we see**
>
> ~~~text
> hiện tượng vật lý
>       ↓
> counter trong phần cứng
>       ↓
> khả năng phần cứng cung cấp
>       ↓
> driver / API
>       ↓
> công cụ thu thập
>       ↓
> con số ta nhìn thấy
> ~~~

Chuỗi này là **kênh đo (measurement channel)**.

Counter không nhảy thẳng từ thực tại vật lý vào trang sách. Nếu một mắt xích có giới hạn, số cuối có thể không đủ để trả lời câu hỏi cơ chế.
## 268 bộ đếm không có nghĩa có 268 câu trả lời

> **KẾT QUẢ ĐO — Measured Result `[E-COUNTER-01]`**
>
> Hệ thống mục tiêu công bố **268 counters** và cần **12 measurement passes** để thu inventory đầy đủ.

Không phải mọi counter đều có thể thu cùng lúc; instrumentation cũng có thể tạo overhead. Vì vậy nguyên tắc đúng không phải “đo mọi thứ”, mà là **chọn counter theo giả thuyết và kiểm tra measurement adequacy**.
## Đo có thể làm thay đổi hệ đang đo

Instrumentation có thể tăng thời gian, thay lịch chạy, thêm lượt đo hoặc làm thay đổi trạng thái cache.

Vì vậy nên tách vai trò bằng chứng:

~~~text
run bình thường
→ đo hiệu năng / localization

run có counter
→ chẩn đoán cơ chế
~~~

Hai loại run phục vụ câu hỏi khác nhau và không tự động thay thế nhau.
## Khi counter trả về 0

Đây là ranh giới quan trọng nhất của chương.

> **KẾT QUẢ ĐO — Measured Result `[E-COUNTER-01]`**
>
> Trong dữ liệu mục tiêu, `GpuTime = 0` ở cả ba nhóm counter được dùng.

GPU rõ ràng đã thực thi công việc, nên không thể đọc con số đó như “GPU time vật lý bằng 0”.

> **[FIGURE F21] — Counter-zero interpretation boundary**
>
> ~~~text
> counter = 0
> ≠
> physical quantity = 0
> ~~~

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> **counter = 0 ≠ physical quantity = 0**
>
> Khi kênh đo không đủ đáng tin, giá trị 0 chỉ cho biết điều công cụ trả về trong kênh đó; nó không tự động xác nhận đại lượng vật lý bằng 0.
## Cùng một chỉ số còn có thể bất nhất

> **KẾT QUẢ ĐO — Measured Result `[E-COUNTER-01]`**
>
> Trong dữ liệu mục tiêu:
>
> ~~~text
> XVE_STALL
> execution/occupancy group → 0
> stall-cause group         → khác 0
>
> GPU_MEMORY_REQUEST_QUEUE_FULL
> memory/cache group → có mẫu khác 0
> stall-cause group  → 0
> ~~~

Những bất nhất cross-group như vậy làm suy yếu khả năng dùng các counter đó để xác nhận một cơ chế cụ thể.
## Độ đầy đủ của bằng chứng

Ta gọi câu hỏi “phép đo này có đủ đáng tin và đủ phân biệt cho cơ chế đang kiểm tra không?” là **độ đầy đủ của phép đo (measurement adequacy)**.

Một counter có thể tồn tại, đọc được và trả số nhưng vẫn **không đủ** cho câu hỏi khoa học đang kiểm tra.
## FAIL của phép đo không phải FAIL của phần cứng

Việc thu thập dữ liệu mục tiêu đã chạy đủ theo kế hoạch, nhưng counter channel không đạt mức cần thiết để xác nhận cơ chế.

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> ~~~text
> measurement adequacy: FAIL
> mechanism: UNRESOLVED
> ~~~
>
> Điều này **không** có nghĩa phần cứng không có cơ chế. Nó có nghĩa bằng chứng hiện tại chưa đủ để chọn một cơ chế làm kết luận nhân quả.

Biết chính xác phép đo không cho phép nói gì cũng là một kết quả khoa học.
## Vậy các khuôn mẫu khác có bỏ đi không?

Không. Các khác biệt về DRAM, cache, stall và utilization vẫn có thể dùng làm **bằng chứng định hướng** để tạo giả thuyết.

Nhưng chúng không được phép vượt qua verdict của measurement adequacy:

~~~text
pattern
↓
hypothesis
↓
KHÔNG tự động
↓
causal conclusion
~~~

Chương 19 sẽ quay lại đúng ranh giới này.
## Và nếu hai số chỉ khác nhau một chút?

Counter không phải trường hợp duy nhất ta phải cẩn thận.

Có lúc hai trạng thái gần như giống nhau.

Có lúc phép đo chỉ chênh rất nhỏ.

Bản năng của ta thường là:

> “Nhỏ thế thì bỏ qua.”

Nhưng trước khi bỏ qua, cần hỏi:

> Nhỏ so với độ phân giải nào?

> Có lặp lại không?

> Có cấu trúc không?

Đó là Chương 18.

### Nhớ 3 điều

1. **Bộ đếm phần cứng (hardware counter) đi qua một kênh đo; con số cuối không phải sự thật vật lý trực tiếp không qua trung gian.**
2. **Trong hệ thống được đo có 268 counter và cần 12 lượt để lấy toàn bộ inventory, nhưng một số counter quan trọng vẫn không đủ đáng tin cho câu hỏi nhân quả.**
3. **Counter bằng 0 không tự động nghĩa đại lượng vật lý bằng 0.** Khi kênh đo không đủ, kết luận đúng có thể là **CHƯA GIẢI QUYẾT**, không phải bịa một nguyên nhân.

**Tiếp theo: [Chương 18 — Khi một khác biệt quá nhỏ vẫn đáng để hỏi](18-khac-biet-nho-van-dang-hoi.md)**
