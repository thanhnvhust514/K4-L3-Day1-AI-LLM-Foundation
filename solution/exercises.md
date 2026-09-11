# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature tăng từ 0.0 lên 1.5, câu trả lời trở nên đa dạng, sáng tạo và đôi khi kỳ quặc hơn. Ở 0.0, mô hình đưa ra cùng một sự thật rất chuẩn xác và lặp lại; ở 1.5, văn phong phóng khoáng, đôi khi bịa ra thông tin (ảo giác) hoặc cấu trúc câu lộn xộn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Cho chatbot hỗ trợ khách hàng, tôi sẽ đặt temperature ở mức thấp (khoảng 0.0 đến 0.3). Vì khách hàng cần thông tin chính xác, nhất quán và đáng tin cậy về sản phẩm/dịch vụ, chứ không cần chatbot sáng tạo ra chính sách hay thông tin sai lệch.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Dựa trên bảng giá, GPT-4o đắt hơn GPT-4o-mini khoảng 16.6 lần ở cả input và output. GPT-4o xứng đáng dùng cho các tác vụ suy luận phức tạp, phân tích dữ liệu chuyên sâu hoặc lập trình. GPT-4o-mini thích hợp cho các tác vụ đơn giản, lặp lại nhiều như trích xuất thực thể, phân loại văn bản, hoặc tóm tắt.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với vai giáo viên tiểu học, phản hồi dùng từ ngữ đơn giản, gần gũi, thường có ví dụ như "cuốn sổ cái lớp học" hay "trò chơi ghép hình", câu chữ ngắn gọn và vui nhộn. Với vai chuyên gia tài chính, phản hồi dùng thuật ngữ như "sổ cái phân tán", "mã hóa", "đồng thuận", "nút mạng" mang tính học thuật và chính xác cao. System prompt thực sự đã điều hướng phong cách, từ vựng và đối tượng hướng đến của model một cách mạnh mẽ.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Khi đếm bằng tiktoken, số lượng token tiếng Việt thường cao hơn rất nhiều so với ước lượng theo số từ (có thể chênh lệch 50-100%). Lý do là vì bộ mã hóa tokenizer của các model (như o200k_base hay cl100k_base) được tối ưu chủ yếu cho tiếng Anh. Tiếng Anh một từ thường là 1 token, nhưng tiếng Việt với nhiều dấu câu, ký tự có dấu thường bị xé lẻ thành 2-3 token cho một từ.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất trong các giao diện chat trực tiếp (UI như ChatGPT) để người dùng có thể đọc ngay lập tức, giảm cảm giác chờ đợi, mang lại trải nghiệm tương tác tự nhiên. Non-streaming phù hợp hơn cho các tác vụ xử lý hàng loạt ở dưới nền (batch processing), hoặc khi cần lấy toàn bộ dữ liệu JSON có cấu trúc để hệ thống phía sau xử lý tiếp.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm dần áp lực lên server khi nó đang bị quá tải, cho server thời gian phục hồi. Nếu hàng nghìn client cùng retry với một delay cố định (ví dụ luôn là 1 giây), thì cứ mỗi 1 giây server lại bị "dội bom" bởi hàng nghìn request cùng lúc (gọi là hiệu ứng Thundering Herd), khiến server càng dễ bị sập hơn.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona: "Bạn là một trợ lý lập trình chuyên nghiệp. Hãy trả lời ngắn gọn, cung cấp đoạn code Python minh họa và luôn giải thích bình luận bằng tiếng Việt." Lựa chọn "trả lời ngắn gọn" giúp tiết kiệm token output (và chi phí), còn "giải thích bằng tiếng Việt" để đảm bảo dù code là tiếng Anh nhưng người học vẫn đọc hiểu được logic dễ dàng.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất hiện tại là lịch sử chỉ lưu 3 lượt (6 tin nhắn), khiến chatbot dễ "quên" thông tin ở đầu phiên chat. Cải thiện: Triển khai cơ chế tóm tắt lịch sử (summarize history). Khi history quá dài, thay vì cắt bỏ hoàn toàn, ta gọi API để tóm tắt các đoạn chat cũ thành một đoạn văn ngắn và dán nó vào system prompt hoặc message đầu tiên, giúp chatbot giữ được ngữ cảnh dài hạn mà không tốn nhiều token.

---

## Danh Sách Kiểm Tra Nộp Bài

- [x] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [x] Cả 4 checkpoint pytest đều pass
- [x] Tất cả 9 câu trong file này đã được trả lời
- [x] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
