# Chương 13 — Khi nhiều phép tính được gộp thành một công việc

> **Mức đọc: Đi sâu**
>
> **Chương trước:**
>
> ~~~text
> 1 phép tính logic
> ↓
> nhiều công việc vật lý
> ~~~
>
> **Chương này hỏi chiều ngược lại:**
>
> ~~~text
> nhiều phép tính logic
> ↓
> 1 công việc vật lý?
> ~~~

Câu trả lời là: **có thể**.

Một hệ thực thi không bắt buộc giữ nguyên ranh giới của sơ đồ mô hình khi đưa công việc xuống GPU.

Đôi khi nhiều phép tính được gộp lại.

## Ví dụ đời thường: rửa và cắt rau

Trên danh sách công việc, “rửa rau” và “cắt rau” là hai bước logic. Nhưng một dây chuyền có thể tổ chức chúng sát nhau hoặc trong cùng một đường xử lý vật lý.

Ý quan trọng không nằm ở ví dụ nhà bếp, mà ở chỗ: **ranh giới logic không bắt buộc trùng ranh giới thực thi**.
## Gộp phép tính là gì?

**Gộp phép tính (fusion)** là việc kết hợp nhiều phép tính logic vào một đường thực thi vật lý chung.

> **[FIGURE F15] — Nhiều logical operations → một physical execution**
>
> ~~~text
> operation A ─┐
>              ├→ fused kernel / dispatch
> operation B ─┘
> ~~~

Fusion có thể giảm dispatch, giảm ghi/đọc dữ liệu trung gian hoặc giảm một số điểm đồng bộ. Nhưng nó không tự động làm hệ thống nhanh hơn.
## Vì sao fusion có thể giúp?

Nếu hai phép tính nối tiếp phải ghi kết quả trung gian ra bộ nhớ rồi đọc lại, chi phí di chuyển dữ liệu có thể đáng kể.

Fusion có thể giữ một phần dữ liệu trung gian gần nơi tính toán hơn. Tuy nhiên kernel hợp nhất cũng có thể dùng nhiều thanh ghi hơn, giảm song song hoặc gặp giới hạn khác.

> **Fusion là một lựa chọn thực thi có trade-off, không phải tối ưu mặc định.**
## Một dispatch có thể phục vụ nhiều danh tính logic

Nếu một dispatch thực hiện cả A và B, attribution trở nên khó hơn: thời gian vật lý đó thuộc A, B hay cả hai?

Không thể nhân đôi toàn bộ thời gian cho A và B; cũng không thể chia 50/50 nếu không có cơ sở.

Đây là lý do hệ quan sát phải tách **danh tính logic** khỏi **đơn vị thực thi vật lý**.
## Trace thực nghiệm dùng trong sách có thấy fusion không?

> **KẾT QUẢ ĐO — Measured Result `[E-TRACE-01]`**
>
> Trong trace 469 dispatch được dùng ở Part III, số trường hợp **nhiều phép tính logic cùng chia sẻ một dispatch** được ghi nhận là:
>
> ~~~text
> 0
> ~~~

> **RANH GIỚI DIỄN GIẢI — Interpretation Boundary**
>
> Trace đó **không cung cấp ví dụ trực tiếp cho fusion**. Con số 0 không chứng minh fusion không tồn tại ở hệ thống khác, phiên bản khác hay execution path khác.
## Vậy tại sao vẫn cần chương này?

Một cách quan sát chỉ đúng với `1 logic = 1 dispatch` sẽ vỡ khi runtime thay đổi.

Ta cần chấp nhận ít nhất ba quan hệ:

~~~text
1 logic → 1 physical
1 logic → nhiều physical
nhiều logic → 1 physical
~~~

và trong hệ thống phức tạp còn có thể xuất hiện quan hệ nhiều-nhiều.
## Danh tính logic phải tồn tại độc lập với cách chạy

Hôm nay FFN-gate và FFN-up có thể chạy riêng; ngày mai hệ thực thi có thể gộp chúng.

Nếu danh tính khoa học phụ thuộc vào “dispatch số mấy”, ta mất khả năng so sánh giữa hai phiên bản.

Ta cần giữ `gate`, `up`, `LM head`... như các danh tính logic tương đối ổn định rồi quan sát cách chúng ánh xạ xuống phần cứng.
## Fusion và “mảnh tri thức”

Ở nửa đầu sách ta đi theo trạng thái token. Sang tầng vật lý, ranh giới khái niệm không nhất thiết được bảo toàn nguyên vẹn.

Một công việc logic có thể bị chia nhỏ, gộp với công việc khác, di chuyển dữ liệu và chạy theo nhiều hình học thực thi.

Vì vậy câu “token đang ở kernel X” thường quá đơn giản. Chính xác hơn là: **một phần công việc phục vụ trạng thái hiện tại đang được thực thi ở đó**.
## Tiếp theo: bộ nhớ

Cho tới đây ta chủ yếu nói:

~~~text
phép tính
↓
kernel
↓
dispatch
~~~

Nhưng phép tính cần dữ liệu.

Trọng số phải được đọc.

Trạng thái trung gian phải nằm đâu đó.

Kết quả phải được ghi.

GPU có thể tính rất nhanh nhưng vẫn phải chờ dữ liệu.

Vì vậy đường đi của token không chỉ là đường của **tính toán (compute)**.

Nó còn là đường của **bộ nhớ (memory)**.

### Nhớ 3 điều

1. **Gộp phép tính (fusion) có thể đưa nhiều phép tính logic vào cùng một công việc vật lý.**
2. **Fusion có thể giảm một số chi phí như dispatch hoặc di chuyển dữ liệu trung gian, nhưng không tự động làm hệ thống nhanh hơn.**
3. **Trace 469/451 được dùng trong sách không trực tiếp chứng minh fusion vì trace đó có 0 phép tính logic dùng chung một dispatch; chương này dạy khả năng tổng quát, không gán sai bằng chứng cho trace.**

**Tiếp theo: [Chương 14 — Bộ nhớ cũng nằm trên đường đi của token](14-bo-nho-tren-duong-di-token.md)**
