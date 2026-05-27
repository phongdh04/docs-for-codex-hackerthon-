# 01-overview.md (PROJECT CHARTER)

## 1. Giới Thiệu Tổng Quan (Executive Summary)

Dự án **Property AI (PAI)** là một hệ thống trợ lý Marketing thông minh được thiết kế chuyên biệt cho các nhà môi giới bất động sản. Mục tiêu cốt lõi của hệ thống là tự động hóa toàn bộ phễu tìm kiếm và nuôi dưỡng khách hàng tiềm năng, hoạt động liên tục 24/7 ngay cả khi môi giới đang nghỉ ngơi. PAI sử dụng sức mạnh của công nghệ đa mô hình ngôn ngữ lớn (Multi-LLM) để tự lập kế hoạch marketing, chạy quảng cáo và tương tác tự nhiên với khách hàng.

---

## 2. Vấn Đề & Thách Thức (Pain Points)

| Mã | Mô tả vấn đề | Hậu quả / Ảnh hưởng |
|---|---|---|
| **P1** | Môi giới tốn quá nhiều thời gian cho việc thiết lập quảng cáo và nuôi dưỡng khách hàng thủ công. | Bỏ lỡ thời điểm vàng để chăm sóc khách hàng hoặc chốt giao dịch. |
| **P2** | Chi phí chạy quảng cáo quá lớn nhưng tỷ lệ chuyển đổi thành lead thực tế rất thấp. | Lãng phí lớn ngân sách marketing của cá nhân môi giới hoặc sàn. |
| **P3** | Phản hồi khách hàng bị chậm trễ khi môi giới bận làm việc trực tiếp hoặc ngoài giờ. | Khách hàng mất kiên nhẫn, rời đi và tìm sang đối thủ cạnh tranh. |
| **P4** | Môi giới thiếu kỹ năng viết content marketing và thiết kế hình ảnh chuyên nghiệp. | Hình ảnh dự án kém thu hút, giảm uy tín thương hiệu cá nhân. |
| **P5** | E ngại kỹ thuật Digital Marketing phức tạp và sợ tối ưu hóa sai luồng. | Gặp bế tắc trong việc tiếp cận các kênh tiếp thị hiện đại. |

---

## 3. Mục Tiêu & Tiêu Chí Đo Lường (Objectives & KPIs)

| Mục tiêu chiến lược | Con số kỳ vọng | Phương pháp / Công cụ đo lường |
|---|---|---|
| **Tự động hóa phễu lead** | 80% lead được tìm thấy và nuôi dưỡng tự động qua chiến dịch marketing của AI. | Chỉ số trên hệ thống CRM Internal Dashboard. |
| **Tối ưu tốc độ phản hồi** | Thời gian phản hồi < 1 phút (AI chat phản hồi tức thì). | Chỉ số đo bằng Telemetry / Socket.io Logs. |
| **Tiết kiệm thời gian content** | Giảm 70% thời gian môi giới phải bỏ ra để viết bài marketing. | Tính năng Tracking Time trên ứng dụng. |
| **Hiệu quả chuyển đổi** | Tăng 20% tỷ lệ lead quan tâm chuyển đổi sang cuộc hẹn gặp. | Tỷ lệ chuyển đổi từ Lead sang Appointment trên CRM. |

---

## 4. Đối Tượng Thụ Hưởng (Stakeholders / Value Proposition)

| Đối tượng | Giá trị nhận được / Mục đích sử dụng |
|---|---|
| **Nhà môi giới cá nhân** | Sở hữu trợ lý ảo AI tìm khách tự động, chat chăm sóc khách hàng 24/7 và triển khai nhanh chóng/vận hành hiệu quả các chiến dịch marketing. |
| **Khách hàng tiềm năng** | Được cung cấp thông tin dự án nhanh chóng, chính xác và cá nhân hóa sâu sắc. |
| **Hệ thống AI Agents** | Tác nhân thực thi chính: tự động lập kế hoạch, soạn bài viết và tương tác thông minh. |

---

## 5. Phạm Vi Tổng Quan (High-level Scope)

- **Trong phạm vi (In-Scope):**
  - AI tự động lập kế hoạch marketing dựa trên kho dữ liệu dự án BĐS được cung cấp.
  - AI triển khai quảng bá đa kênh Google ads, facebook ads qua fanpage/website và nuôi dưỡng khách hàng.
  - AI tự động chat tương tác và nuôi dưỡng khách hàng tiềm năng đến khi đạt trạng thái Level 3 (sẵn sàng gặp mặt).
  - Ứng dụng PWA Client hoạt động mượt mà trên cả PC ([web-client](./05-repositories-registry.md)) và Thiết bị di động ([pwa-client](./05-repositories-registry.md)).
  - Bảo mật tài khoản thông qua hệ thống **ID-system** xác thực bằng cơ chế JWT Token nội bộ.
- **Ngoài phạm vi (Out-of-Scope):**
  - Các thủ tục pháp lý, đặt cọc và ký kết hợp đồng giấy tờ trực tiếp (do môi giới tự thực hiện).
  - Quản lý dòng tiền, tài chính và các nghiệp vụ kế toán chuyên sâu của sàn giao dịch.

---

## 6. Giải Pháp Công Nghệ & Kiến Trúc (Tech Stack)

Hệ thống được chia nhỏ thành 6 dịch vụ phối hợp đồng bộ qua kiến trúc hướng sự kiện (Event-driven):

| Thành phần / Layer | Công nghệ áp dụng | Vai trò trong hệ thống |
|---|---|---|
| **Frontend PC** | React + Vite + PWA (`web-client`) | Giao diện quản trị, CMS cho Admin và Sàn BĐS trên PC. |
| **Frontend Mobile** | React + Vite + PWA (`pwa-client`) | Giao diện di động mượt mà cho Broker quản lý lead và chat. |
| **Backend Core** | Spring Boot (`product-service`) | Cung cấp các RESTful API nghiệp vụ cho Web/App/CMS. |
| **AI Integration** | Spring AI 2.0.0 (Milestone) (`ai-mcp-server`) | Tích hợp và điều phối các công cụ thực thi cho AI agent. |
| **AI Agents Core** | Spring AI 2.0.0-M (`ai-agents`) | Sử dụng Spring AI làm lõi LLM engine điều phối và xử lý suy nghĩ của các tác nhân (AI Agents). |
| **Realtime Service** | Node.js + Socket.io (`realtime-server`) | Xử lý tin nhắn chat thời gian thực, thông báo lead mới. |
| **Background Tasks** | Spring Boot (`background-service`) | Đồng bộ dữ liệu ngầm, schedule tasks, listen Kafka/Redis. |
| **Database** | MySQL | Lưu trữ dữ liệu cấu trúc dự án BĐS, thông tin lead và cấu hình kịch bản. |
| **Caching & Message** | Redis & Apache Kafka | Cache dữ liệu nhanh và điều phối hàng đợi xử lý bất đồng bộ. |
| **AI Model** | Gemini models (qua Gemini API) | Mô hình ngôn ngữ lớn cốt lõi cung cấp khả năng suy luận, sinh nội dung tiếp thị và kịch bản chat tự nhiên. |

---

## 7. Lộ Trình Phát Triển (Roadmap / Milestones)

- **Phase 1 (MVP):** Hoàn thiện khung 7 dịch vụ (bao gồm `ai-agents`), AI lập kịch bản cơ bản và chạy quảng cáo thử nghiệm.
- **Phase 2:** Triển khai quy trình AI Nurturing Level 3 hoàn chỉnh, tích hợp chat thời gian thực qua PWA Client và đồng bộ hóa đa mô hình AI (GPT-4o, Gemini, Claude).
- **Phase 3:** Mở rộng tính năng tối ưu hóa quảng cáo tự động nâng cao bằng AI và tối ưu hóa hệ thống cảnh báo sớm.

---

## 8. Rủi Ro & Biện Pháp Giảm Thiểu (Risks & Mitigations)

| Rủi ro | Hậu quả | Biện pháp phòng ngừa | Biện pháp xử lý |
|---|---|---|---|
| **AI Hallucination** (AI bịa thông tin) | Cung cấp sai giá bán hoặc thông tin pháp lý dự án cho khách. | Ràng buộc AI bằng Prompt chặt chẽ và Vector DB (RAG) được Admin duyệt. | Gắn nhãn "AI Generated", cho phép Broker chỉnh sửa nhanh tin nhắn trên app. |
| **Khóa tài khoản quảng cáo** | Vi phạm chính sách quảng cáo của Google/Facebook dẫn đến khóa tài khoản quảng cáo. | Tuân thủ nghiêm ngặt chính sách quảng cáo của từng nền tảng, thiết lập nội dung an toàn. | Tích hợp cơ chế tự động theo dõi trạng thái tài khoản quảng cáo và tối ưu hóa ngân sách trong `background-service`. |
| **Bảo mật dữ liệu Lead** | Rò rỉ thông tin số điện thoại, nhu cầu của khách hàng. | Mã hóa dữ liệu nhạy cảm trong CSDL MySQL, bắt buộc xác thực qua JWT. | Phân quyền truy cập nghiêm ngặt dựa trên ID-system của hệ thống. |

---

## 9. Thuật Ngữ & Quy Định Chung (Glossary & Global Rules)

### 9.1 Thuật ngữ cốt lõi (Glossary)
| Thuật ngữ | Tên mã nguồn (Code Name) | Ý nghĩa / Giải thích |
|---|---|---|
| **Lead / Khách hàng tiềm năng** | `Lead` | Thông tin định danh của khách hàng quan tâm BĐS thu được từ các chiến dịch quảng cáo. |
| **Nurturing Level 3** | `NurturingLevel3` | Trạng thái AI đã tương tác thành công tối thiểu 3 lượt chat và khách hàng đồng ý để lại SĐT hoặc hẹn lịch gặp Broker. |
| **PWA Client** | `PWAClient` | Ứng dụng web dạng Progressive Web App chạy trên PC (`web-client`) và Mobile (`pwa-client`). |
| **Hệ thống ID-system** | `IDSystem` | Hệ thống xác thực nội bộ của dự án sử dụng cơ chế JWT Token an toàn. |

### 9.2 Ràng buộc nghiệp vụ theo Gói dịch vụ (Global Business Rules)
Hệ thống Property AI phân cấp giới hạn tài nguyên và chi phí sử dụng của nhà môi giới theo 3 gói dịch vụ sau:

*   **Gói Cơ bản (200k/tháng):**
    *   Tối đa setup **1 dự án** bất động sản đồng thời.
    *   Giới hạn tối đa **500 tin nhắn chat tự động** của AI mỗi tháng.
    *   Chỉ cho phép chạy **1 chiến dịch** marketing tại một thời điểm.
*   **Gói Tiêu chuẩn (500k/tháng):**
    *   Tối đa setup **1 dự án** bất động sản đồng thời.
    *   Giới hạn tối đa **2,000 tin nhắn chat tự động** của AI mỗi tháng.
    *   Cho phép chạy tối đa **3 chiến dịch** marketing đồng thời.
    *   Hỗ trợ công cụ quảng bá đa kênh cơ bản.
*   **Gói Chuyên nghiệp (1.5M/tháng):**
    *   Tối đa setup **5 dự án** bất động sản đồng thời.
    *   Giới hạn tối đa **10,000 tin nhắn chat tự động** của AI mỗi tháng.
    *   Không giới hạn số lượng chiến dịch marketing chạy đồng thời.
    *   Hỗ trợ tối ưu quảng cáo bằng AI nâng cao và phân hạng tiềm năng (Lead Scoring).

---

## Lịch Sử Cập Nhật (Changelog)

| Phiên bản | Ngày | Người cập nhật | Nội dung thay đổi |
|---|---|---|---|
| v1.0 | 2026-05-10 | Admin | Bản thảo khởi tạo ban đầu. |
| v1.1 | 2026-05-26 | Lux - Project-Level Documentation Specialist | Cập nhật cấu trúc 6 dịch vụ thực tế, cơ chế xác thực JWT nội bộ, bảng quy tắc hạn mức gói cước môi giới và liên kết chéo hệ thống. |
