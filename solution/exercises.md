# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> *3/4 câu trả lời là về chủ đề hang Sơn Đoòng. Câu trả lời với temperature 1.0 và 1.5 văn phong có vẻ bay bổng hơn, tuy nhiên nội dung thì gần như nhau.*

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> *Tôi sẽ đặt temperature 0.0 vì chatbot hỗ trợ khách hàng cần độ chính xác, ổn định và nhất quán thay vì tính sáng tạo, tránh việc mỗi lần trả lời lại diễn đạt hoặc suy diễn khác nhau. Temperature thấp giúp hạn chế bịa thông tin và đảm bảo cùng một câu hỏi sẽ nhận được câu trả lời gần như giống nhau.*

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> *Dựa trên bảng giá PRICING_PER_1K_TOKENS thì GPT-4o đắt hơn GPT-4o-mini ~16.7 lần với cùng số token sử dụng. GPt-4o nên dùng trong trường hợp đòi hỏi câu trả lời chất lượng, có độ chính xác cao, ví dụ như các vấn đề về pháp lý. Còn GPT-40-mini nên dùng trong các tác vụ không yêu cầu độ chính xác quá cao như tóm tắt văn bản, trò chuyện cơ bản.*

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> *Câu trả lời với system prompt thứ 2 dài hơn, đầy đủ hơn, sử dụng những từ vựng chuyên ngành, mang tính học thuật, giải thích theo hướng chuyên sâu như Distributed Ledger Technology (DLT), hash, node, Proof of Work (PoW), Proof of Stake (PoS). Còn câu trả lời với system prompt đầu tiên câu trả lời ngắn gọn, dùng ngôn ngữ đơn giản và ví dụ gần gũi. Điều này cho thấy system prompt định hướng vai trò, phong cách, mức độ chi tiết và đối tượng người đọc, từ đó ảnh hưởng trực tiếp đến cách mô hình diễn đạt và lựa chọn nội dung.*

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> *Đoạn văn gốc của tôi có khoảng 141 từ. Số token đo được theo count_tokens là 172 và số từ/0.75 là 188, chênh lệch khoảng 9%. Tiếng Việt tốn nhiều token hơn tiếng Anh cùng độ dài là vì tokenizer của các mô hình LLM được tối ưu chủ yếu trên dữ liệu tiếng Anh. Nhiều từ tiếng Việt có dấu thanh, ký tự Unicode và từ ghép nhiều âm tiết, nên chúng thường bị tách thành nhiều token hơn thay vì một token như các từ tiếng Anh phổ biến.*

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> *Streaming quan trọng khi tác vụ thường có các câu trả lời dài, xử lý lâu, để tránh việc người dùng có cảm giác chờ đợi lâu, cảm giác hệ thống bị lỗi khi mãi không thấy cập nhật. Còn non-streaming phù hợp khi câu trả lời ngắn, cần nhận toàn bộ kết quả một lần hoặc phải xử lý dữ liệu hoàn chỉnh trước khi hiển thị, chẳng hạn như trả về JSON, kết quả phân loại.*

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> *Exponential backoff giúp giảm tải cho API khi hệ thống đang quá tải bằng cách tăng dần thời gian chờ giữa các lần thử lại, tạo điều kiện để máy chủ có thời gian phục hồi. So với việc luôn chờ một khoảng thời gian cố định, cách này làm giảm số lượng yêu cầu gửi đến trong thời gian ngắn và tăng khả năng thành công của các lần retry. Nếu hàng nghìn client cùng sử dụng delay cố định (ví dụ đều retry sau 1 giây), chúng sẽ gửi yêu cầu gần như cùng lúc, gây ra hiện tượng retry storm, khiến máy chủ tiếp tục bị quá tải và khó phục hồi hơn.*

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> *Persona: "Bạn là trợ giảng cho khóa học về AI, luôn trả lời bằng tiếng Việt khi không có yêu cầu gì khác. Trả lời rõ ràng, nếu không chắc chắn về thông tin thì nói rõ ra thay vì bịa thông tin." Tôi yêu cầu trả lời bằng tiếng Việt để tiếp cận đến đa số người dùng, yêu cầu tránh bịa thông tin để câu trả lời được chính xác nhất.*

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> *Trợ lý hiện tại chỉ lưu lịch sử 3 lượt hội thoại gần nhất, khiến model bị quên thông tin khi đoạn hội thoại kéo dài. Trợ lý cũng không có bộ nhớ dài hạn để ghi nhớ sở thích, thói quen, thông tin cá nhân của người dùng. Từ đó tôi đề xuất cải tiến: lưu các cuộc hội thoại vào cơ sở dữ liệu hoặc vector database. Khi người dùng đặt câu hỏi mới, hệ thống sẽ tìm kiếm các đoạn hội thoại liên quan và đưa vào prompt cùng với lịch sử gần nhất. Cách này giúp trợ lý ghi nhớ thông tin xuyên suốt nhiều phiên làm việc, trả lời nhất quán hơn và hỗ trợ các cuộc hội thoại dài hiệu quả hơn.*

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
