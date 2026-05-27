# 10-observability.md (HƯỚNG DẪN GIÁM SÁT & CẢNH BÁO)

Tài liệu này quy định các tiêu chuẩn kỹ thuật về ghi nhật ký (Logging), giám sát chỉ số (Metrics), cảnh báo sự cố (Alerting) và truy vết lỗi (Tracing) cho toàn bộ hệ thống 6 dịch vụ của dự án **Property AI (PAI)**.

---

## 1. Quy Chuẩn Ghi Nhật Ký (Logging Standards)

BẮT BUỘC áp dụng cấu trúc ghi log thống nhất trên 100% các services, đặc biệt là các services backend (`product-service`, `background-service`, `ai-mcp-server`) để dễ dàng truy vết và bóc tách dữ liệu bằng các hệ thống phân tích log tập trung.

### 1.1 Phân cấp Log (Log Levels)
- **`ERROR`:** Lỗi nghiêm trọng làm sập hệ thống (Không kết nối được MongoDB/Redis/Kafka, API Key của OpenAI/Gemini bị hết hạn hoặc sai mã, lỗi logic nghiệp vụ làm đứng luồng chat).
- **`WARN`:** Các sự cố không làm sập luồng nhưng cần lưu ý (Thời gian AI phản hồi chat chậm > 10 giây, API Key AI sắp hết số dư, thiết bị mô phỏng bị mất kết nối tạm thời).
- **`INFO`:** Ghi nhận các mốc sự kiện nghiệp vụ quan trọng (Môi giới đăng nhập thành công, AI bắt đầu lập chiến dịch, AI chuyển giao lead nóng đạt Level 3, tin nhắn chat mới gửi đi thành công).
- **`DEBUG`:** Ghi lại chi tiết nội dung Prompt gửi đi và câu trả lời thô của AI, kết quả tìm kiếm ngữ cảnh RAG (Chỉ bật ở môi trường Development, cấm bật trên Production).

### 1.2 Định dạng cấu trúc Log JSON chuẩn
Tất cả các log xuất ra từ container Docker bắt buộc phải ghi dưới định dạng JSON có cấu trúc rõ ràng sau để phục vụ việc parsing tự động:
```json
{
  "timestamp": "2026-05-26T13:45:00.123+07:00",
  "level": "INFO",
  "traceId": "trace-8ee6-415f-984a-d5e6e9ccc59b",
  "service": "product-service",
  "class": "com.propertyai.service.impl.LeadServiceImpl",
  "message": "AI chuyển giao thành công Lead nóng Level 3 cho Broker Broker_007",
  "context": {
    "lead_id": "LEAD_999",
    "broker_id": "Broker_007",
    "score": 95,
    "intent": "WANT_TO_VIEW_SITE"
  }
}
```

---

## 2. Giám Sát Hiệu Năng (Performance Monitoring)

Hệ thống sử dụng bộ công cụ **Prometheus** để thu thập các chỉ số hiệu năng và **Grafana** để trực quan hóa sơ đồ sức khỏe của 6 container dịch vụ triển khai On-premise.

### Bảng Chỉ Số Metrics Cốt Lõi Cần Giám Sát:
| Tên chỉ số (Metric Name) | Đơn vị | Loại (Type) | Ngưỡng cảnh báo nguy hiểm (Alarm Threshold) | Mục đích giám sát |
|---|---|---|---|---|
| **`system_cpu_usage`** | Phần trăm (%) | Gauge | CPU Usage > 85% trong 3 phút liên tục. | Theo dõi tải phần cứng của các container trên Docker Swarm. |
| **`system_memory_usage`** | Megabyte (MB) | Gauge | RAM Usage > 90% dung lượng cấp phát. | Phát hiện rò rỉ bộ nhớ (Memory Leak) trong Spring Boot và Node.js. |
| **`api_response_latency`** | Mili-giây (ms) | Histogram | Thời gian phản hồi trung bình > 500ms (loại trừ API gọi AI). | Giám sát tốc độ phản hồi REST API của `product-service`. |
| **`ai_response_time`** | Giây (s) | Histogram | Thời gian AI phản hồi chat > 15 giây cho một tin nhắn. | Theo dõi độ trễ của các dịch vụ OpenAI/Gemini qua `ai-mcp-server`. |
| **`kafka_consumer_lag`** | Số message | Counter | Consumer Lag > 100 tin nhắn chưa xử lý trong Queue. | Phát hiện tình trạng nghẽn hàng đợi quét lead ở `background-service`. |
| **`socket_active_connections`** | Số kết nối | Gauge | Giảm đột ngột > 50% số lượng kết nối trong 1 phút. | Theo dõi tình trạng kết nối WebSocket thời gian thực của `realtime-server`. |

---

## 3. Cơ Chế Cảnh Báo Sự Cố (Alerting System)

Hệ thống cấu hình cảnh báo tự động thông qua **Telegram Alerting Bot** để đẩy thông tin sự cố trực tiếp về Group chat của đội ngũ vận hành On-premise:

- **Cảnh báo Cấp độ P0 (Khẩn cấp - Blocker):**
  - *Sự kiện:* Server chính On-premise chết, cơ sở dữ liệu MongoDB sập hoàn toàn, hoặc API Key OpenAI bị block/hết hạn mức tài chính (HTTP 401/429).
  - *Hành động:* Gửi tin nhắn Telegram kèm tag `@all` + Gọi điện thoại tự động cho Tech Lead.
- **Cảnh báo Cấp độ P1 (Nghiêm trọng - Major):**
  - *Sự kiện:* Tỷ lệ lỗi API AI (`ai-mcp-server` trả về lỗi) > 10% trong vòng 5 phút liên tục, hoặc thời gian phản hồi API chat > 20 giây.
  - *Hành động:* Đẩy thông báo Telegram đỏ tới Group Dev/QA để kiểm tra log.
- **Cảnh báo Cấp độ P2 (Cảnh báo - Minor):**
  - *Sự kiện:* Một trong các container `background-service` hoặc `realtime-server` bị restart đột ngột, hoặc CPU/RAM đạt ngưỡng cảnh báo.
  - *Hành động:* Gửi thông báo Telegram thông thường tới Group vận hành để theo dõi.

---

## 4. Truy Vết Lỗi Hệ Thống (Error Tracing)

Hệ thống tích hợp SDK **Sentry** để tự động hóa việc thu thập lỗi runtime và ngoại lệ (Exceptions) chưa được bắt ở cả Frontend PWA và Backend Spring Boot:

- **Nguyên tắc đồng bộ Trace ID (`Correlation-ID`):**
  - Khi PWA Client (`pwa-client` hoặc `web-client`) gửi một request đi, hệ thống sẽ tự động tạo một mã định danh duy nhất là `traceId` và đính kèm vào Header của request.
  - `product-service` tiếp nhận, truyền mã `traceId` này vào Header gửi đi qua Kafka để `background-service` và `ai-mcp-server` cùng ghi nhận trong Log.
- **Truy vết trên Sentry:**
  - Mọi lỗi 5xx xuất hiện ở Backend hoặc lỗi Crash ở Frontend PWA sẽ được Sentry tự động bắt lại, đính kèm mã `traceId`.
  - Dev/QC có thể lấy mã `traceId` từ màn hình lỗi của người dùng, tra cứu trên Sentry để xem chính xác toàn bộ luồng đi của Request qua 6 services và bối cảnh dẫn đến lỗi (Stack Trace).

---

## Lịch Sử Cập Nhật (Changelog)

| Phiên bản | Ngày | Người cập nhật | Nội dung thay đổi |
|---|---|---|---|
| v1.0 | 2026-05-10 | Admin | Bản thảo khởi tạo ban đầu. |
| v1.1 | 2026-05-26 | Lux - Project-Level Documentation Specialist | Chuẩn hóa cấu trúc 4 phần của Guideline, tích hợp cấu trúc Log JSON chuẩn chứa traceId, bảng metrics hiệu năng chi tiết kèm ngưỡng cảnh báo, và đồng bộ liên kết chéo. |
