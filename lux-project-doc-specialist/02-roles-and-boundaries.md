# 02-roles-and-boundaries.md (SCOPE, ROLES & BOUNDARIES)

## 1. Phạm Vi Thực Thi Chi Tiết (Detailed Execution Scope)

Tài liệu này xác định ranh giới hoạt động nghiệp vụ giữa sự tự động hóa của các AI Agents và trách nhiệm xử lý trực tiếp của con người trong phễu Marketing - Bán hàng của Property AI (PAI).

- **Trong phạm vi tự động hóa (In-Scope - AI & System):**
  - Hệ thống tự động đồng bộ và lưu trữ dữ liệu Lead thu được từ các chiến dịch quảng cáo qua `background-service`.
  - AI (`ai-mcp-server` & `ai-agents`) tự động phân tích bối cảnh dự án BĐS, sinh bài viết content marketing hàng loạt và lập kế hoạch chiến dịch.
  - Trợ lý ảo chat tự động trả lời, giải đáp thắc mắc và "nuôi dưỡng" khách hàng (Nurturing) thời gian thực qua `realtime-server` cho đến khi đạt mức độ sẵn sàng gặp mặt (Nurturing Level 3).
  - Tự động chấm điểm độ nóng của khách hàng (Lead Scoring) dựa trên các từ khóa và mức độ tương tác.
  - Phân quyền và bảo mật chặt chẽ nhờ **ID-system** xác thực bằng cơ chế JWT Token nội bộ.
- **Ngoài phạm vi tự động hóa (Out-of-Scope - Humans):**
  - Broker trực tiếp tổ chức và thực hiện các buổi gặp mặt thực tế, dẫn khách đi xem nhà (Site-visit).
  - Broker trực tiếp đàm phán giá bán cuối cùng, chốt deal, ký hợp đồng và làm thủ tục pháp lý.
  - Các nghiệp vụ xử lý tài chính và kế toán dòng tiền lớn của sàn giao dịch.

---

## 2. Cơ Cấu Tác Nhân (Team & System Actors)

| Vai trò / Tác nhân | Loại | Nhiệm vụ cốt lõi trong hệ thống |
|---|---|---|
| **Quản trị viên hệ thống (Admin)** | Con người (Human) | Quản lý tài khoản môi giới, cấu hình hạn mức gói cước, duyệt kho dự án BĐS hệ thống. |
| **Nhà môi giới (Broker)** | Con người (Human) | Thiết lập dự án riêng, duyệt bài viết AI đề xuất, tiếp nhận lead "nóng" Level 3, và chat trực tiếp khi cần. |
| **Trợ lý ảo AI (AI Agent)** | Tác nhân thông minh (AI) | Lập kế hoạch tiếp thị, tạo nội dung bài viết, chạy quảng cáo và chat nurturing tự động với khách hàng. |
| **Khách hàng tiềm năng (Lead)** | Con người (Human) | Đối tượng nhận thông tin tiếp thị, trò chuyện trực tiếp với AI/Broker để tìm hiểu dự án. |
| **Hệ thống ID-system** | Hệ thống (System) | Cấp phát JWT Token, kiểm tra quyền truy cập API của từng tác nhân, chống rò rỉ dữ liệu lead. |
| **Hệ thống bên thứ 3** | Hệ thống (System) | Các nền tảng MXH (Facebook, Zalo), hệ thống SMS, Firebase Cloud Messaging để đẩy thông báo. |

---

## 3. Ma Trận Phân Quyền (Permission Matrix - RBAC)

Toàn bộ các API nghiệp vụ của hệ thống (`product-service`) bắt buộc phải kiểm tra quyền dựa trên thông tin giải mã từ JWT Token của **ID-system**:

| Tính năng / Hành động | Admin hệ thống | Nhà môi giới (Broker) | Trợ lý ảo AI (Agent) | Khách hàng (Guest) |
|---|:---:|:---:|:---:|:---:|
| **Quản lý tài khoản & Gói cước** | ✅ | ❌ | ❌ | ❌ |
| **Thêm/Sửa/Xóa dự án BĐS hệ thống** | ✅ | ❌ | ❌ | ❌ |
| **Đăng ký / Setup dự án cá nhân** | ✅ | ✅ | ❌ | ❌ |
| **Duyệt bài viết / Kế hoạch AI tạo** | ✅ | ✅ | ❌ | ❌ |
| **Đề xuất content & Kế hoạch chiến dịch** | ❌ | ❌ | ✅ (Đề xuất) | ❌ |
| **Chat tự động với khách hàng** | ❌ | ❌ | ✅ (Tự động) | ❌ |
| **Xem danh sách & Chi tiết Lead** | ✅ | ✅ (Chỉ lead của mình) | ✅ (Khi được giao) | ❌ |
| **Chuyển trạng thái Lead (Chốt/Hủy)** | ❌ | ✅ | ❌ | ❌ |
| **Nhận thông báo Lead nóng tức thời** | ❌ | ✅ | ❌ | ❌ |

---

## 4. Giới Hạn Hệ Thống & Tự Động Hóa (System Boundaries & Constraints)

Hệ thống tự động hóa và AI hoạt động dựa trên các nguyên tắc "giới hạn đỏ" và cơ chế bảo mật nghiêm ngặt sau:

- **CẤM (Giới hạn đỏ đối với AI):**
  - AI không được tự ý cam kết các mức chiết khấu, giảm giá hoặc quà tặng nằm ngoài chính sách giá được cấu hình cứng trong DB.
  - AI tuyệt đối không được tự ý xóa dữ liệu Lead hoặc thay đổi trạng thái phân hạng Lead thành "Chốt thành công" mà không có sự phê duyệt thủ công của Broker.
  - Nghiêm cấm cứng mã khóa bảo mật hoặc API Key trong bất kỳ repository nào. Mọi hoạt động xác thực phải đi qua JWT Token được cấp bởi ID-system.
- **BẮT BUỘC (Cơ chế kiểm soát):**
  - Mọi bài viết marketing hoặc kế hoạch chiến dịch do AI đề xuất phải hiển thị ở trạng thái "Chờ duyệt" (Draft/Pending) trên `web-client`/`pwa-client` cho đến khi Broker bấm nút "Duyệt" (Approve) mới được đưa vào hàng đợi thực thi ngầm ở `background-service`.
  - Hệ thống bắt buộc phải tự động ngắt chat tự động của AI và gửi cảnh báo "Human Takeover" đến Broker khi khách hàng có hành vi chửi bới, đòi gặp trực tiếp, hoặc nhập thông tin nằm ngoài phạm vi hiểu biết của AI.
  - Bắt buộc kiểm tra hạn mức sử dụng gói cước (Basic/Standard/Premium) trước khi kích hoạt chiến dịch quảng cáo hoặc chạy AI chat tự động.

---

## 5. Quy Tắc Đặc Biệt & Ngoại Lệ (Special Rules & Edge Cases)

| Tên quy tắc / Ngoại lệ | Mô tả chi tiết quy trình xử lý | Nơi xử lý (Component) |
|---|---|---|
| **Human Takeover** (Broker tiếp quản) | Khi Broker bấm nút chat trực tiếp hoặc chủ động trả lời khách hàng trên ứng dụng di động, hệ thống ngay lập tức tắt trạng thái tự động trả lời của AI đối với khách hàng đó để tránh xung đột kịch bản. | `realtime-server` & `product-service` |
| **JWT Expiration** | Token JWT của Broker và PWA Client có thời gian sống (TTL) là 7 ngày. Hệ thống không hỗ trợ refresh token; sau khi hết thời hạn người dùng phải đăng nhập lại để cấp token mới. | ID-system & PWA Client |
| **API Fallback** (Lỗi kết nối AI) | Nếu mô hình AI chính gặp sự cố kết nối hoặc phản hồi lỗi > 3 lần liên tiếp, `ai-mcp-server` bắt buộc phải tự động chuyển hướng (fallback) sang mô hình dự phòng đã cấu hình để duy trì cuộc hội thoại mượt mà với khách hàng. | `ai-mcp-server` (Spring AI) |

---

## Lịch Sử Cập Nhật (Changelog)

| Phiên bản | Ngày | Người cập nhật | Nội dung thay đổi |
|---|---|---|---|
| v1.0 | 2026-05-10 | Admin | Bản thảo khởi tạo ban đầu. |
| v1.1 | 2026-05-26 | Lux - Project-Level Documentation Specialist | Cập nhật cấu trúc ranh giới hệ thống tích hợp ID-system JWT, ma trận phân quyền dựa trên 6 services thực tế và liên kết chéo. |
