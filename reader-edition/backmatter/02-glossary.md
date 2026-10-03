# Glossary / Thuật ngữ

> Glossary của Vietnamese Reader Edition v1. Định nghĩa được giữ ngắn và theo đúng cách các thuật ngữ được dùng trong thân sách; đây không phải một bách khoa toàn thư AI.

## Token và mô hình

### Token
Một mảnh văn bản nhỏ mà mô hình xử lý. Token có thể là một từ, một phần của từ, dấu câu hoặc chuỗi ký tự khác.

### Bộ tách và mã hóa văn bản (tokenizer)
Thành phần chia văn bản thành các token và ánh xạ chúng sang mã token mà mô hình có thể nhận.

### Mã token (token ID)
Số nguyên dùng làm danh tính của một token trong hệ mã hóa. Token ID không phải toàn bộ ý nghĩa hay tri thức của token.

### Từ vựng token (vocabulary)
Tập các đơn vị token và mã tương ứng mà một tokenizer có thể sử dụng.

### Phép nhúng (embedding)
Phép dùng token ID để lấy một biểu diễn số ban đầu đã được học, thường từ một bảng embedding.

### Biểu diễn (representation)
Dạng số mà mô hình dùng để mang và biến đổi trạng thái của một token hoặc vị trí tại một thời điểm.

### Vectơ (vector)
Một dãy số có thứ tự. Trong sách, vectơ thường được dùng để hình dung một biểu diễn hoặc trạng thái.

### Khối số (tensor)
Một nhóm các con số được sắp theo một hình dạng để máy tính xử lý; vectơ và ma trận là các trường hợp quen thuộc.

### Tham số (parameter)
Một con số có thể được điều chỉnh trong quá trình huấn luyện và trở thành một phần của mô hình đã học.

### Trọng số (weight)
Một loại tham số phổ biến, tham gia trực tiếp vào các phép biến đổi số của mô hình.

### Ngữ cảnh (context)
Phần thông tin xung quanh mà mô hình có thể sử dụng khi xử lý một vị trí trong chuỗi.

### Trạng thái ẩn (hidden state)
Trạng thái số nằm bên trong mô hình tại một vị trí; nó không phải trực tiếp văn bản đầu vào hay token đầu ra.

### Biểu diễn theo ngữ cảnh (contextual representation)
Biểu diễn của một vị trí sau khi đã chịu ảnh hưởng của thông tin từ ngữ cảnh xung quanh.

### Vị trí (position)
Vị trí của token trong chuỗi. Thứ tự này tham gia vào cách mô hình tạo và cập nhật trạng thái.

### Transformer
Kiến trúc xử lý chuỗi bằng nhiều lớp lặp lại, trong đó attention, FFN, chuẩn hóa và residual cùng tham gia cập nhật trạng thái.

### Mô hình chỉ dùng bộ giải mã (decoder-only model)
Một kiểu Transformer dùng ngữ cảnh đã có để dự đoán token tiếp theo, với cơ chế che không cho vị trí hiện tại nhìn thông tin tương lai.

### Lớp (layer)
Một khối xử lý trong Transformer. Nhiều lớp nối tiếp nhau làm trạng thái được cập nhật nhiều lần.

### Chuẩn hóa (normalization) / RMSNorm
Bước đưa tín hiệu về dạng thuận lợi hơn cho phép tính tiếp theo. RMSNorm là một dạng chuẩn hóa thường gặp trong LLM.

### Cơ chế chú ý (attention)
Cơ chế cho phép một vị trí trộn thông tin từ những vị trí khác mà nó được phép nhìn tới.

### Truy vấn / khóa / giá trị (query / key / value)
Ba vai trò trong attention: query dùng để tìm mức liên quan, key dùng để so khớp với query, còn value mang thông tin được trộn vào đầu ra.

### Điểm chú ý (attention score)
Điểm biểu thị mức phù hợp giữa một query và một key trước khi các điểm được chuẩn hóa thành trọng số.

### Softmax
Phép biến một nhóm điểm thành các giá trị dương có tổng bằng 1, thường dùng để tạo trọng số attention hoặc phân bố xác suất từ logits.

### Đầu chú ý (attention head)
Một nhánh attention song song có các vai trò Q/K/V riêng trong cách mô hình tổ chức việc trộn thông tin.

### Che nhân quả (causal masking)
Quy tắc ngăn một vị trí trong mô hình sinh tự hồi quy sử dụng thông tin từ các vị trí tương lai.

### MHA / MQA / GQA
Ba cách tổ chức nhiều đầu attention. MHA dùng nhiều đầu Q/K/V; MQA cho nhiều query heads dùng chung key/value; GQA nhóm các query heads để dùng ít key/value heads hơn.

### Mạng truyền thẳng (Feed-Forward Network, FFN)
Nhánh biến đổi trạng thái tại từng vị trí bằng cùng một bộ trọng số đã học, thường sau khi attention đã trộn thông tin ngữ cảnh.

### Phi tuyến (nonlinearity)
Bước biến đổi không tuyến tính giúp chuỗi phép tính biểu diễn được những quan hệ mà chỉ ghép các phép tuyến tính không thể tạo ra.

### Gating
Cơ chế dùng một nhánh hoặc tín hiệu như cổng để điều tiết phần tín hiệu được truyền qua trong một phép biến đổi.

### Đường cộng tắt (residual connection)
Đường cho trạng thái trước đi trực tiếp tới phép cộng với phần cập nhật mới, thay vì bắt mỗi lớp thay thế toàn bộ trạng thái cũ.

### Dòng residual (residual stream)
Cách nhìn trạng thái số đang được cập nhật xuyên qua nhiều lớp nhờ các phép cộng residual nối tiếp.

### Chiều sâu (depth)
Số lớp hoặc số bước biến đổi nối tiếp mà tín hiệu đi qua trong mạng.

### Sự kết hợp nhiều bước (composition)
Việc nhiều phép biến đổi nối tiếp cùng tạo nên hành vi cuối; kết quả không nhất thiết thuộc riêng một lớp hay một bước.

### Quỹ đạo biểu diễn (representation trajectory)
Chuỗi các trạng thái số của cùng một vị trí khi nó đi qua nhiều lớp, ví dụ `x0 → x1 → … → xN`.

### Lớp tạo điểm đầu ra (language-model head, LM head)
Phép biến trạng thái cuối thành các logits cho những token có thể xuất hiện tiếp theo.

### Điểm dự đoán (logit)
Điểm thô mà mô hình gán cho một token ứng viên trước khi chuyển sang xác suất hoặc áp dụng chiến lược giải mã.

### Chọn tham lam (greedy decoding)
Chiến lược luôn chọn token có điểm hoặc xác suất cao nhất ở mỗi bước sinh.

### Lấy mẫu (sampling)
Chiến lược chọn token theo một phân bố xác suất thay vì luôn lấy ứng viên đứng đầu.

## Runtime và phần cứng

### Phép tính logic (logical/model-level operation)
Danh tính của một công việc theo vai trò của nó trong đồ thị mô hình. Một phép tính logic không bắt buộc tương ứng một-một với kernel hay dispatch.

### Chương trình GPU (kernel)
Đoạn mã tính toán chạy trên GPU. Cùng một kernel có thể được gọi nhiều lần.

### Lần giao việc (dispatch)
Một lần cụ thể hệ thực thi gửi một kernel đi chạy với cấu hình công việc xác định.

### Quy chiếu (attribution)
Việc gắn một quan sát vật lý như dispatch hoặc thời gian đo được trở lại phép tính logic mà nó phục vụ.

### Hình học thực thi (execution geometry)
Cách một bài toán được chia thành các nhóm công việc để phần cứng xử lý.

### Phân rã vật lý (physical decomposition)
Việc một phép tính logic được triển khai thành nhiều công việc thực thi vật lý, chẳng hạn nhiều dispatch.

### Gộp phép tính (fusion)
Việc kết hợp nhiều phép tính logic vào một đường thực thi vật lý chung.

### Phân cấp bộ nhớ (memory hierarchy)
Cách hệ thống tổ chức nhiều tầng lưu trữ có tốc độ, dung lượng và khoảng cách tới đơn vị tính toán khác nhau.

### DRAM
Tầng bộ nhớ dung lượng lớn dùng để chứa dữ liệu mà GPU hoặc CPU cần truy cập; thường xa đơn vị tính toán hơn cache và có chi phí truy cập khác.

### Bộ nhớ đệm (cache)
Vùng nhớ nhỏ hơn và nhanh hơn, cố gắng giữ dữ liệu có khả năng sớm được dùng lại.

### Lần trúng bộ nhớ đệm (cache hit)
Trường hợp dữ liệu cần dùng đã có trong cache nên không phải lấy lại từ tầng xa hơn.

### Bị giới hạn bởi tính toán (compute-bound)
Trạng thái trong đó năng lực tính toán là giới hạn chính đối với hiệu năng trong phạm vi đang xét.

### Bị giới hạn bởi bộ nhớ (memory-bound)
Trạng thái trong đó việc cung cấp hoặc di chuyển dữ liệu là giới hạn chính đối với hiệu năng trong phạm vi đang xét. Nhãn này cần bằng chứng, không suy ra chỉ từ việc dữ liệu lớn.

### Đồng bộ (synchronization)
Cơ chế kiểm soát thứ tự hoặc phụ thuộc giữa các công việc thực thi.

### Hàng rào (barrier)
Một cơ chế đồng bộ buộc các công việc hoặc truy cập liên quan tuân theo một ranh giới thứ tự. Có barrier không đồng nghĩa toàn bộ khoảng thời gian xung quanh là barrier cost.

### Dấu vết thực thi (execution trace)
Bản ghi công việc vật lý theo thời gian, có thể gồm dispatch, thứ tự, timestamp, điểm đồng bộ và thông tin quy chiếu.

### Dấu thời gian (timestamp)
Mốc thời gian do cơ chế đo cung cấp để đánh dấu vị trí hoặc khoảng thực thi của một công việc.

### Điểm nóng (hotspot)
Vùng hoặc họ công việc chiếm phần đáng kể của thời gian đo được trong một trace hay workload cụ thể.

### Định vị chi phí (localization)
Việc xác định thời gian hoặc chi phí đang tập trung ở đâu. Localization trả lời nơi cần điều tra, không tự trả lời nguyên nhân.

### Định luật Amdahl (Amdahl's law)
Nguyên lý cho biết mức tăng tốc toàn hệ thống bị giới hạn bởi phần thời gian mà thay đổi tối ưu thực sự tác động tới.

## Phép đo và bằng chứng

### Bộ đếm phần cứng (hardware counter)
Đại lượng do phần cứng, driver hoặc API đo và báo về các sự kiện hay mức hoạt động. Giá trị counter là kết quả của một kênh đo, không phải sự thật vật lý không qua trung gian.

### Kênh đo (measurement channel)
Chuỗi từ hiện tượng vật lý qua phần cứng, driver, API và công cụ tới con số mà người quan sát nhìn thấy.

### Instrumentation
Phần mã hoặc cơ chế được thêm vào để quan sát và đo hệ thống. Instrumentation có thể tạo overhead hoặc làm thay đổi chính hệ thống đang đo.

### Độ đầy đủ của phép đo (measurement adequacy)
Mức phép đo đủ đáng tin và đủ khả năng phân biệt các giải thích cạnh tranh cho câu hỏi đang kiểm tra.

### Khác biệt / delta (Δ)
Độ chênh giữa hai giá trị, trạng thái hoặc phép đo được đem ra so sánh.

### Khác biệt dư / phần dư (residual difference / residual)
Phần chênh còn lại sau một phép trừ hoặc so sánh. Một residual nhỏ chỉ có ý nghĩa khi đặt cạnh độ phân giải, mức nhiễu và khả năng lặp lại.

### Độ phân giải (resolution)
Mức chi tiết nhỏ nhất mà một phép đo có thể phân biệt đáng tin.

### Mức nhiễu (noise floor)
Mức dao động nền thường gặp của phép đo hoặc hệ thống, dùng làm mốc để đánh giá một khác biệt nhỏ.

### Khả năng lặp lại (repeatability)
Khả năng một hiện tượng hoặc pattern xuất hiện lại khi phép đo được lặp trong điều kiện phù hợp.

### Tương quan (correlation)
Quan hệ trong đó hai đại lượng thay đổi cùng nhau. Tương quan tự nó không chứng minh quan hệ nhân quả.

### Biến gây nhiễu (confounder)
Yếu tố khác có thể tạo, che hoặc làm sai cách ta diễn giải mối liên hệ giữa những đại lượng đang quan sát.

### Bằng chứng định hướng (directional evidence)
Bằng chứng cho thấy một pattern hoặc hướng khác biệt đáng để hình thành giả thuyết, nhưng chưa đủ để xác nhận cơ chế nhân quả.

### Can thiệp (intervention)
Thay đổi có kiểm soát một yếu tố rồi quan sát phản ứng so với điều kiện đối chứng nhằm phân biệt các giải thích cạnh tranh.

### Kết luận nhân quả (causal claim)
Khẳng định rằng một yếu tố gây ra một kết quả. Loại kết luận này cần thiết kế phép thử đủ phân biệt và kênh đo đủ đáng tin.

### Chưa giải quyết / chưa đủ bằng chứng (unresolved / insufficient evidence)
Trạng thái trong đó bằng chứng hiện có chưa đủ để chọn một cơ chế hoặc kết luận. Đây là một kết quả khoa học hợp lệ, không phải giấy phép để điền vào bằng một lời giải thích đẹp.

---

Glossary này chỉ khóa nghĩa đọc sách. Khi một thuật ngữ liên quan tới số đo hoặc verdict nghiên cứu, Evidence Notes và nguồn provenance mới là nơi có thẩm quyền về giá trị đo và phạm vi claim.
