# 08-qa-strategy.md (CHIẾN LƯỢC QUẢN TRỊ CHẤT LƯỢNG)

Tài liệu này quy định chiến lược kiểm soát chất lượng (QA/QC) cho toàn bộ hệ thống của dự án **Property AI (PAI)**. Nhằm đảm bảo trải nghiệm mượt mà của PWA Client và độ chính xác của trợ lý ảo AI, dự án áp dụng chiến lược kiểm thử hỗn hợp kết hợp giữa **Custom Pytest Framework (Tự động)** và **Manual Testing (Thủ công)**.

---

## 1. Cấu Trúc Kiểm Thử (Testing Pyramid)

Dự án phân bổ nguồn lực kiểm thử theo mô hình tháp kiểm thử 3 lớp tiêu chuẩn, đặc biệt chú ý tới kiểm soát luồng xử lý bất đồng bộ qua Kafka/Redis và phản hồi AI:

- **Unit Test (Chiếm 60%):**
  - **Backend (`product-service`, `background-service`, `ai-mcp-server`):** Lập trình viên viết Unit Test bằng **JUnit 5** và Mockito để kiểm tra các hàm xử lý logic, tính toán hạn mức gói cước và giải mã JWT token.
  - **Frontend (`web-client`, `pwa-client`):** Viết Unit Test bằng **Jest** và React Testing Library để kiểm tra các component UI tĩnh.
- **Integration Test (Chiếm 30%):**
  - Thực hiện kiểm thử khớp nối giữa REST API từ `product-service` với MongoDB và hàng đợi Kafka.
  - Thực hiện kiểm thử tích hợp giữa `ai-mcp-server` với các LLM Models (OpenAI/Gemini/Claude) để đo lường độ trễ và độ an toàn của Prompt.
- **E2E / UI Test & Manual Test (Chiếm 10%):**
  - Kiểm thử toàn trình các kịch bản quan trọng từ giao diện người dùng (Môi giới truy cập PWA -> AI lập chiến dịch -> Chat tương tác -> Đẩy cảnh báo lead nóng).

---

## 2. Quy Trình Báo Cáo & Xử Lý Lỗi (Bug Lifecycle & Reporting)

Mọi lỗi phát hiện được trong quá trình kiểm thử tự động hoặc kiểm thử thủ công phải được tạo Bug Report chi tiết trên hệ thống quản lý task chung (Jira/Redmine) theo cấu trúc chuẩn:

### 2.1 Định dạng tiêu đề Bug Report
- Cấu trúc: `[Component] - [Bug Type] - [Task ID] - Mô tả ngắn về lỗi`
- *Ví dụ:* `[pwa-client] - [UI] - PAI-122 - Nút "Nhận Lead" bị lệch hiển thị trên Safari Mobile`
- *Ví dụ:* `[ai-mcp-server] - [Logic AI] - PAI-130 - AI bịa thông tin giá phân khu Beverly sai 20%`

### 2.2 Các thông tin bắt buộc trong Bug Report
1. **Phân cấp độ nghiêm trọng (Severity):**
   - `Blocker`: Lỗi chết server, lỗi bảo mật JWT hỏng, lỗi bùng nổ token AI không kiểm soát.
   - `Critical`: AI chat liên tục phản hồi sai giá BĐS, lỗi không kết nối được Socket.io thời gian thực.
   - `Major`: Một số nút chức năng PWA bị đứt liên kết hoặc không thể lưu bản nháp chiến dịch.
   - `Minor`: Lỗi sai lệch nhỏ về hiển thị UI, lỗi chính tả.
2. **Môi trường xảy ra lỗi (Environment):** Dev / Staging / OS phiên bản / Trình duyệt.
3. **Các bước tái hiện (Steps to Reproduce):** Ghi rõ các bước bấm nút kèm **Prompt mẫu đã gửi cho AI** (nếu là lỗi AI).
4. **Kết quả thực tế (Actual Result):** Log lỗi hoặc ảnh chụp màn hình thực tế.
5. **Kết quả kỳ vọng (Expected Result):** Dựa trên tài liệu nghiệp vụ [06-workflow-and-process.md](./06-workflow-and-process.md).

---

## 3. Chấp Nhận Nghiệm Thu (User Acceptance Testing - UAT)

Quy trình UAT là chốt chặn cuối cùng trước khi đưa mã nguồn lên môi trường Production On-premise, có sự tham gia trực tiếp của PO và khách hàng chạy thử:

- **Điều kiện cần để kích hoạt UAT:**
  - 100% các Pull Requests đã được merge thành công vào nhánh `develop`.
  - Hệ thống build tự động trên GitHub Actions thành công và deploy ổn định lên môi trường Staging.
  - Bộ kiểm thử tự động (Custom Pytest Framework) đạt tỷ lệ Pass > 95%.
- **Tiêu chí phê duyệt hoàn thành UAT (Acceptance Criteria):**
  - Toàn bộ các Acceptance Criteria (AC) trong Specs của BA đã được thỏa mãn.
  - Trợ lý chat AI tuyệt đối tuân thủ các **giới hạn đỏ** quy định tại [02-roles-and-boundaries.md](./02-roles-and-boundaries.md) (Không cam kết giá sai lệch, tự động nhường quyền khi Broker nhảy vào chat).
  - Không còn tồn tại bất kỳ lỗi nào ở mức độ `Blocker` hoặc `Critical` chưa được vá.

---

## 4. Kiểm Thử Tự Động & Thủ Công (Testing Standards)

### 4.1 Quy chuẩn Kiểm thử tự động (Custom Pytest Framework)
Dự án phát triển một khung kiểm thử tự động tùy chỉnh sử dụng ngôn ngữ **Python** để kiểm soát chất lượng tích hợp và API:
- **Framework chính:** **Pytest**.
- **API Automation Testing:** Sử dụng thư viện **Requests** trong Python để giả lập các request mang theo JWT Token từ ID-system, gửi tới các API của `product-service` nhằm kiểm tra độ chịu tải và tính chính xác của dữ liệu trả về.
- **AI Regression Testing:** Viết các script Pytest gửi các bộ Prompt mẫu định kỳ tới `ai-mcp-server`, sau đó sử dụng các thuật toán so khớp ngữ nghĩa (Semantic Similarity) để đánh giá câu trả lời của AI, ngăn ngừa tình trạng suy giảm chất lượng prompt (Prompt Drift).
- **CI/CD Integration:** Bộ test Pytest tự động được đóng gói thành một Docker container riêng biệt và được trigger tự động chạy trên GitHub Actions mỗi khi có PR vào nhánh `develop`.

### 4.2 Tầm quan trọng của Manual Testing (Kiểm thử thủ công)
Mặc dù hệ thống có kiểm thử tự động, **Manual Testing** vẫn đóng vai trò cực kỳ then chốt và không thể thay thế trong dự án PAI:
- **Lý do bắt buộc:** 
  - Khả năng hội thoại của AI vô cùng biến ảo, các script tự động không thể bao quát hết mọi câu chữ, sắc thái và cảm xúc của khách hàng khi chat.
  - Kiểm thử trực quan trải nghiệm Progressive Web App (PWA) khi Broker cài đặt ứng dụng lên màn hình nền điện thoại, tắt mạng kiểm thử offline cache và nhận Push Notification qua Firebase.
- **Phương pháp thực hiện:**
  - QA thực hiện giả lập làm khách hàng khó tính, nhắn tin trực tiếp trên giao diện di động của `pwa-client` để kiểm tra độ nhạy bén, khả năng xử lý ngữ cảnh của trợ lý chat AI.
  - Kiểm tra thủ công giao diện quản trị CMS `web-client` trên nhiều trình duyệt khác nhau (Chrome, Safari, Edge) để đảm bảo độ mượt mà.

---

## Lịch Sự Cập Nhật (Changelog)

| Phiên bản | Ngày | Người cập nhật | Nội dung thay đổi |
|---|---|---|---|
| v1.0 | 2026-05-10 | Admin | Bản thảo khởi tạo ban đầu. |
| v1.1 | 2026-05-26 | Lux - Project-Level Documentation Specialist | Chuẩn hóa quy trình kiểm thử phối hợp Custom Pytest Framework và Manual Testing, bổ sung quy định cụ thể về phân loại mức độ lỗi cho AI và đồng bộ liên kết chéo. |
