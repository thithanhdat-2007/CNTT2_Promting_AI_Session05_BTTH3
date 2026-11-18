# BÀI TẬP: THIẾT KẾ PROMPT CHAIN-OF-THOUGHT XỬ LÝ KHIẾU NẠI KHÁCH HÀNG

## PHẦN 1: PROMPT HOÀN CHỈNH (CHUẨN KỸ THUẬT CHAIN-OF-THOUGHT)

Dưới đây là câu lệnh hoàn chỉnh được thiết kế theo cấu trúc rõ ràng, tích hợp kỹ thuật suy luận từng bước (**Chain-of-Thought**) và các ràng buộc chống suy diễn vô căn cứ để nhân viên CSKH nạp trực tiếp vào AI:

```text
Vai trò (Role): Bạn là Chuyên gia Phân tích Nghiệp vụ Chăm sóc Khách hàng (CSKH) và Quản lý Rủi ro Dịch vụ Hậu mãi ngành bán lẻ công nghệ.

Bối cảnh & Dữ liệu đầu vào (Context & Input):
Một nhân viên CSKH cần xử lý khiếu nại từ khách hàng mua laptop. Dưới đây là nội dung phản ánh nguyên văn của khách hàng và thông tin chính sách hiện có:

<customer_complaint>
"Tôi mua laptop cách đây 10 ngày. Máy thường xuyên bị nóng, quạt kêu to và pin chỉ sử dụng được khoảng 2 tiếng. Tôi đã mang đến cửa hàng kiểm tra nhưng nhân viên chỉ vệ sinh máy và trả lại. Sau 2 ngày, tình trạng vẫn tiếp diễn. Tôi muốn đổi máy hoặc hoàn tiền."
</customer_complaint>

<store_policy>
[Chưa được cung cấp trong dữ liệu đầu vào]
</store_policy>

Ràng buộc kỹ thuật bắt buộc (Guardrails):
1. TUYỆT ĐỐI KHÔNG tự ý giả định hoặc bịa đặt chính sách đổi trả, hoàn tiền của cửa hàng (ví dụ: không tự suy đoán hạn đổi trả là 7 ngày, 15 ngày hay 30 ngày).
2. Phân định rạch ròi giữa: Dữ kiện thực tế (Facts), Giả thuyết chuyên môn (Hypotheses) và Thông tin chưa xác định (Unknowns).
3. Không đưa ra kết luận chắc chắn về nguyên nhân lỗi phần cứng hay cam kết đổi/hoàn tiền khi chưa có căn cứ xác minh.

Yêu cầu suy luận từng bước (Chain-of-Thought Execution):
Hãy suy nghĩ và phân tích tuần tự theo đúng 5 bước dưới đây trước khi đưa ra đề xuất cuối cùng:

- Bước 1 (Phân loại thông tin): Bóc tách nội dung khiếu nại thành 3 nhóm riêng biệt: (1) Dữ kiện đã xác thực từ lời kể của khách, (2) Giả thuyết kỹ thuật/vận hành có thể xảy ra, và (3) Thông tin còn thiếu chưa được xác định.
- Bước 2 (Đánh giá vấn đề & Mức độ nghiêm trọng): Liệt kê từng vấn đề khách hàng đang gặp phải (cả về sản phẩm lẫn quy trình dịch vụ) và đánh giá mức độ nghiêm trọng kèm lý do.
- Bước 3 (Lập danh sách thông tin cần xác minh): Liệt kê cụ thể các thông tin nhân viên CSKH cần kiểm tra ngay với khách hàng, bộ phận kỹ thuật và quản lý cửa hàng.
- Bước 4 (Đề xuất 2 phương án xử lý): Dựa trên nguyên tắc quản trị rủi ro và không giả định chính sách, đề xuất 2 phương án giải quyết khả thi, phân tích rõ Ưu điểm và Hạn chế của từng phương án.
- Bước 5 (Soạn phản hồi gửi khách hàng): Viết một mẫu tin nhắn/email phản hồi chuyên nghiệp, đồng cảm, nhận trách nhiệm về trải nghiệm chưa tốt ở lần kiểm tra trước, nhưng giữ thái độ khách quan và không hứa hẹn vượt thẩm quyền khi chưa thẩm định lại máy và đối chiếu chính sách.
```

---

## PHẦN 2: KẾT QUẢ THỰC THI MÔ PHỎNG TỪ PROMPT

Dưới đây là kết quả đầu ra chi tiết mà AI tạo ra khi thực thi đúng trình tự **Chain-of-Thought** theo các ràng buộc của đề bài:

### 1. Báo cáo phân tích theo từng bước (Chain-of-Thought Analysis)

#### Bước 1: Phân tách Dữ kiện – Giả thuyết – Thông tin chưa xác định
* **Dữ kiện thực tế (Facts):**
  * Thời điểm mua máy: Cách đây $10$ ngày.
  * Biểu hiện khách hàng phản ánh: Máy thường xuyên nóng, quạt kêu to, thời lượng sử dụng pin khoảng $2$ tiếng.
  * Lịch sử xử lý lần $1$: Khách đã mang máy tới cửa hàng kiểm tra; nhân viên chỉ thực hiện vệ sinh máy và bàn giao lại.
  * Diễn biến tiếp theo: Sau $2$ ngày từ lần kiểm tra đầu tiên, các hiện tượng trên vẫn tiếp diễn.
  * Yêu cầu hiện tại của khách: Muốn đổi máy mới hoặc được hoàn tiền.
* **Giả thuyết chuyên môn (Hypotheses — Cần kiểm chứng, chưa phải kết luận):**
  * *Giả thuyết kỹ thuật $1$ (Lỗi phần cứng/sản xuất):* Hệ thống tản nhiệt (keo tản nhiệt, ống đồng, quạt) lắp ráp chưa chuẩn hoặc cell pin bị lỗi từ nhà sản xuất (lỗi DOA - Dead on Arrival).
  * *Giả thuyết kỹ thuật $2$ (Tác nhân phần mềm & cấu hình):* Máy đang chạy ngầm các tiến trình cập nhật hệ điều hành (Windows Update), phần mềm diệt virus, driver xung đột, hoặc người dùng đang bật chế độ hiệu năng cao (High Performance) với độ sáng màn hình tối đa.
  * *Giả thuyết vận hành dịch vụ:* Quy trình tiếp nhận ở lần kiểm tra trước của nhân viên kỹ thuật có thể đã thực hiện qua loa (chỉ vệ sinh bên ngoài mà chưa chạy phần mềm kiểm tra tải CPU/nhiệt độ/độ chai pin), dẫn đến bỏ sót nguyên nhân gốc rễ.
* **Thông tin chưa xác định (Unknowns):**
  * **Chính sách đổi trả/hoàn tiền của cửa hàng:** Chưa rõ quy định thời hạn đổi trả (ví dụ: trong vòng $7$, $14$ hay $30$ ngày) và điều kiện đi kèm (phải có xác nhận lỗi phần cứng từ hãng/kỹ thuật viên, nguyên vẹn ngoại quan, đầy đủ hộp phụ kiện).
  * **Phân khúc & Cấu hình laptop:** Chưa rõ đây là dòng Ultrabook văn phòng mỏng nhẹ hay Laptop Gaming/Workstation hiệu năng cao (với dòng Gaming chạy tác vụ nặng, máy nóng và pin kéo dài $2$ tiếng có thể là đặc tính bình thường).
  * **Bối cảnh sử dụng thực tế của khách:** Khách dùng máy để làm tác vụ gì (lướt web nhẹ hay render video, chơi game), môi trường sử dụng có bí nhiệt không.
  * **Tình trạng vật lý hiện tại & Biên bản lần $1$:** Máy có dấu hiệu va đập, vào nước hay không; phiếu tiếp nhận lần trước ghi nhận thông số kỹ thuật gì.

#### Bước 2: Xác định các vấn đề & Phân tích mức độ nghiêm trọng
1. **Vấn đề 1 — Máy thường xuyên nóng và quạt kêu to:**
   * *Mức độ nghiêm trọng:* **Trung bình đến Cao.** Nếu xảy ra khi chạy tác vụ văn phòng cơ bản, đây là dấu hiệu bất thường nghiêm trọng về hệ thống tản nhiệt hoặc xung đột phần mềm, gây khó chịu và ảnh hưởng tuổi thọ linh kiện.
2. **Vấn đề 2 — Thời lượng pin chỉ đạt khoảng $2$ tiếng:**
   * *Mức độ nghiêm trọng:* **Trung bình đến Cao.** Mức độ nghiêm trọng phụ thuộc trực tiếp vào dòng máy và tác vụ sử dụng (rất bất thường nếu là máy văn phòng dùng tác vụ nhẹ, nhưng có thể chấp nhận được nếu là máy cấu hình cao đang chạy tác vụ nặng).
3. **Vấn đề 3 — Quy trình xử lý dịch vụ lần $1$ thiếu triệt để (Chỉ vệ sinh máy mới mua $8$ ngày):**
   * *Mức độ nghiêm trọng:* **Rất Nghiêm Trọng (Critical về trải nghiệm khách hàng).** Một chiếc laptop mới mua chưa đầy $2$ tuần ít có khả năng hỏng do bụi bẩn; việc chỉ vệ sinh qua loa khiến lỗi tái diễn sau $2$ ngày là nguyên nhân trực tiếp làm mất niềm tin và đẩy khách hàng tới tâm lý đòi hoàn tiền.
4. **Vấn đề 4 — Nguy cơ tranh chấp từ yêu cầu đổi máy hoặc hoàn tiền:**
   * *Mức độ nghiêm trọng:* **Cao (Rủi ro tài chính & uy tín).** Cần đối chiếu ngay với quy định nội bộ của cửa hàng để tránh xử lý sai thẩm quyền hoặc tạo tiền lệ khiếu nại phức tạp.

---

### 2. Danh sách thông tin còn thiếu cần xác minh

Nhân viên CSKH cần rà soát và thu thập đầy đủ $3$ nhóm thông tin sau trước khi đưa ra quyết định cuối cùng:

* **Nhóm A — Xác minh nội bộ & Chính sách cửa hàng (Ưu tiên số 1):**
  1. Quy định cụ thể về thời hạn và điều kiện áp dụng chính sách **đổi máy mới 1-1** hoặc **hoàn tiền** đối với đơn hàng đã mua $10$ ngày.
  2. Quy trình thẩm định lỗi: Cửa hàng có quyền tự xác nhận lỗi đổi trả hay bắt buộc phải có biên bản xác nhận lỗi phần cứng từ Trung tâm Bảo hành chính hãng?
  3. Tra cứu phiếu tiếp nhận/lịch sử xử lý cách đây $2$ ngày: Nhân viên nào đã tiếp nhận, đã thực hiện những bước kiểm tra nào?
* **Nhóm B — Xác minh về sản phẩm & Tình trạng vật lý:**
  4. Mã sản phẩm (Model), thông số cấu hình chi tiết và thông số pin tiêu chuẩn theo công bố của nhà sản xuất.
  5. Tình trạng ngoại quan hiện tại của máy (có trầy xước, cấn móp, vào chất lỏng, rách tem bảo hành không) và tình trạng vỏ hộp, phụ kiện đi kèm.
* **Nhóm C — Xác minh từ phía khách hàng:**
  6. Các phần mềm/tác vụ khách thường xuyên sử dụng khi máy xuất hiện tình trạng nóng, quạt kêu to và sụt pin.
  7. Tình trạng máy có xảy ra khi cắm sạc hay chỉ khi dùng pin, ở chế độ nguồn (Power Mode) nào?

---

### 3. Đề xuất Hai phương án xử lý (Kèm Ưu điểm & Hạn chế)

Vì chưa có dữ liệu chính sách đổi trả của cửa hàng và chưa có kết quả đo đạc kỹ thuật thực tế, dưới đây là $2$ phương án tiếp cận phù hợp để CSKH tham mưu cho quản lý:

#### Phương án 1: Kiểm tra kỹ thuật chuyên sâu có giám sát (Express Diagnostics) & Xử lý theo đúng kết quả thẩm định
* **Nội dung triển khai:** Mời khách hàng mang máy quay lại cửa hàng (hoặc hỗ trợ giao nhận tận nơi) để Kỹ thuật viên trưởng trực tiếp kiểm tra bằng công cụ chuyên dụng (đo nhiệt độ CPU/GPU, kiểm tra Battery Report, kiểm tra tiến trình chạy ngầm) một cách minh bạch trước sự chứng kiến của khách.
  * *Nếu xác định lỗi phần cứng:* Áp dụng ngay quyền lợi đổi máy hoặc hoàn tiền theo đúng quy định hiện hành của cửa hàng.
  * *Nếu do xung đột phần mềm/cấu hình:* Tiến hành tối ưu lại hệ thống, cập nhật driver chuẩn và giải thích rõ bằng thông số đo đạc thực tế.
* **Ưu điểm:**
  * Đảm bảo tuân thủ chặt chẽ quy định nội bộ và nguyên tắc quản trị rủi ro tài chính (chỉ đổi/hoàn khi có căn cứ xác thực).
  * Minh bạch hóa nguyên nhân kỹ thuật, tránh việc đổi máy mới nhưng khách vẫn gặp tình trạng tương tự do thói quen sử dụng phần mềm nặng.
* **Hạn chế:**
  * Yêu cầu khách hàng phải chờ đợi thêm thời gian thẩm định; nếu kỹ năng giao tiếp của nhân viên không khéo léo, khách hàng đang bức xúc có thể cảm thấy cửa hàng đang "câu giờ" hoặc né tránh trách nhiệm.

#### Phương án 2: Xử lý linh hoạt ưu tiên giữ chân khách hàng (Hỗ trợ máy dùng tạm / Đề xuất cấp quản lý duyệt đổi ngang nếu đủ điều kiện ngoại quan)
* **Nội dung triển khai:** Chủ động xin lỗi chân thành về sự thiếu sót trong khâu kiểm tra lần đầu. Cung cấp ngay cho khách một chiếc laptop tương đương để **mượn dùng tạm** trong thời gian cửa hàng gửi máy thẩm định kỹ thuật khẩn cấp. Đồng thời, nếu máy đảm bảo nguyên vẹn ngoại quan, đầy đủ phụ kiện và nằm trong khung thẩm quyền linh hoạt của Quản lý cửa hàng, có thể đề xuất hỗ trợ đổi sang sản phẩm khác phù hợp hơn với nhu cầu của khách.
* **Ưu điểm:**
  * Xoa dịu sự bức xúc ngay lập tức, cho thấy cửa hàng đặt trải nghiệm khách hàng lên hàng đầu và dám nhận trách nhiệm về khâu phục vụ lần trước.
  * Không làm gián đoạn công việc của khách hàng trong thời gian chờ kiểm tra, giúp khôi phục niềm tin và giữ chân khách hàng trung thành.
* **Hạn chế:**
  * Phát sinh chi phí vận hành (quản lý máy cho mượn) hoặc rủi ro tồn kho/giảm giá trị hàng hóa nếu sau đó thẩm định cho thấy máy không hề bị lỗi phần cứng.
  * Bắt buộc phải xin phê duyệt ngoại lệ từ Quản lý cửa hàng trước khi đề xuất với khách.

---

### 4. Mẫu phản hồi chuyên nghiệp gửi khách hàng

> **Kính gửi Anh/Chị [Tên khách hàng],**
>
> Lời đầu tiên, cửa hàng xin chân thành cảm ơn Anh/Chị đã tin tưởng lựa chọn sản phẩm của chúng tôi và rất xin lỗi Anh/Chị vì trải nghiệm chưa trọn vẹn trong $10$ ngày sử dụng vừa qua. Đặc biệt, chúng tôi xin nhận trách nhiệm vì trong lần kiểm tra cách đây $2$ ngày, bộ phận kỹ thuật đã chưa kiểm tra chuyên sâu triệt để dẫn đến việc tình trạng máy nóng, quạt kêu to và pin sụt nhanh vẫn tiếp diễn, làm mất thời gian của Anh/Chị.
>
> Chúng tôi hoàn toàn thấu hiểu mong muốn được đổi máy hoặc hoàn tiền của Anh/Chị lúc này. Để đảm bảo quyền lợi cao nhất cho Anh/Chị theo đúng quy định bảo hành – đổi trả của cửa hàng, chúng tôi xin phép được hỗ trợ Anh/Chị theo quy trình ưu tiên như sau:
>
> 1. **Kiểm tra kỹ thuật chuyên sâu & minh bạch:** Kính mời Anh/Chị mang máy (kèm đầy đủ hộp và phụ kiện nếu có) đến cửa hàng trong thời gian thuận tiện nhất để **Kỹ thuật viên trưởng** trực tiếp đo đạc các thông số nhiệt độ, hiệu năng và tình trạng pin ngay trước sự chứng kiến của Anh/Chị (không chỉ vệ sinh thông thường như lần trước). Trong trường hợp Anh/Chị bận rộn, cửa hàng xin phép hỗ trợ nhân viên đến nhận máy tận nơi.
> 2. **Cam kết phương án xử lý rõ ràng:** Ngay khi có kết quả kiểm tra chính xác, nếu xác định máy phát sinh lỗi kỹ thuật từ nhà sản xuất, cửa hàng sẽ lập tức tiến hành thủ tục hỗ trợ đổi máy hoặc giải quyết quyền lợi theo đúng chính sách áp dụng cho sản phẩm mua trong $10$ ngày. Trong trường hợp cần thêm thời gian thẩm định với hãng, chúng tôi sẵn sàng chuẩn bị một máy tính tương đương để Anh/Chị mượn sử dụng tạm thời nhằm không làm gián đoạn công việc.
>
> Anh/Chị vui lòng phản hồi lại khung giờ thuận tiện hoặc số điện thoại liên hệ để Quản lý cửa hàng trực tiếp đón tiếp và hỗ trợ Anh/Chị chu đáo nhất.
>
> Trân trọng,  
> **[Tên nhân viên CSKH]**  
> Bộ phận Chăm sóc Khách hàng – [Tên Cửa hàng]