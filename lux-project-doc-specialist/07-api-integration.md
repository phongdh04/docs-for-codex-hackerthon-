# 07-api-integration.md (QUY CHUẨN GIAO TIẾP API)

Tài liệu này quy định các chuẩn mực giao tiếp dữ liệu (Data Contracts) giữa các dịch vụ Frontend PWA (`web-client`, `pwa-client`), Core Backend (`product-service`), Realtime Socket.io (`realtime-server`) và Trình tích hợp AI (`ai-mcp-server`) của dự án **Property AI (PAI)**.

---

## 1. Cấu Trúc Phản Hồi Chuẩn (Base Response Format)

Hệ thống Core Backend (`product-service`) áp dụng định dạng JSON đồng bộ cho 100% các phản hồi API để Frontend dễ dàng bóc tách và xử lý hiển thị giao diện:

### 1.1 RESTful API Response Format
- **Phản hồi Thành công (Success - HTTP 200/201):**
```json
{
  "status": 1,
  "data": {
    "campaign_id": "CAMP_2026_99",
    "name": "Chiến dịch Vinhomes Grand Park",
    "status": "active"
  },
  "message": "Thao tác thành công",
  "meta": {
    "server_time": 1779842400,
    "traceId": "trace-8ee6-415f-984a-d5e6e9ccc59b"
  }
}
```

- **Phản hồi Thất bại (Failure - HTTP 4xx/5xx):**
```json
{
  "status": 0,
  "data": null,
  "message": "Không tìm thấy thông tin dự án bất động sản",
  "error_code": "VAL_4004",
  "payload": {
    "field": "project_id",
    "issue": "not_exists"
  },
  "meta": {
    "server_time": 1779842400,
    "traceId": "trace-8ee6-415f-984a-d5e6e9ccc59b"
  }
}
```

### 1.2 Real-time Socket.io Message Format
Các bản tin chat và thông báo đẩy thời gian thực gửi qua `realtime-server` bắt buộc tuân thủ cấu trúc sau:
```json
{
  "event_name": "NEW_HOT_LEAD",
  "sender": "SYSTEM_AI",
  "recipient": "BROKER_2026_007",
  "payload": {
    "lead_id": "LEAD_999",
    "score": 95,
    "intent": "WANT_TO_VIEW_SITE",
    "message": "Khách hàng mong muốn đi xem nhà thực tế vào Chủ Nhật tuần này!"
  },
  "timestamp": 1779842400
}
```

---

## 2. Từ Điển Mã Lỗi (Error Code Dictionary)

Hệ thống mã lỗi nghiệp vụ thống nhất giúp Frontend PWA hiển thị thông báo lỗi thân thiện cho người dùng cuối và chuyển đổi ngôn ngữ (i18n):

| Mã lỗi (Error Code) | HTTP Status | Mô tả chi tiết nguyên nhân | Hành động của Frontend (PWA Client) |
|---|---|---|---|
| **`AUTH_4001`** | 401 Unauthorized | JWT Token không hợp lệ hoặc không được truyền trong Header. | Chuyển hướng người dùng về trang đăng nhập `/login` lập tức. |
| **`AUTH_4002`** | 401 Unauthorized | JWT Token đã hết hạn sử dụng. | Gọi API refresh token ngầm. Nếu lỗi, chuyển về trang `/login`. |
| **`VAL_4001`** | 400 Bad Request | Thiếu các trường dữ liệu bắt buộc ở Request Body. | Hiển thị thông báo đỏ "Vui lòng nhập đầy đủ thông tin" ở các ô input. |
| **`VAL_4002`** | 403 Forbidden | Vượt quá hạn mức gói cước (Ví dụ: setup dự án thứ 2 ở gói Cơ bản). | Hiển thị thông báo pop-up "Hạn mức đã đầy, vui lòng nâng cấp gói". |
| **`AI_5001`** | 502 Bad Gateway | Không kết nối được với các mô hình AI lớn (GPT/Gemini/Claude). | Hiển thị thông báo "Hệ thống AI đang bận xử lý, vui lòng thử lại sau". |
| **`SYS_5001`** | 500 Internal Server Error | Lỗi hệ thống cơ sở dữ liệu MongoDB sập hoặc Kafka treo. | Hiển thị thông báo lỗi hệ thống chung và báo cáo log về Sentry. |

---

## 3. Quy Tắc Thiết Kế Endpoint & API Specs (RESTful Standards)

### 3.1 Quy tắc thiết kế chung
- **URL Pattern:** `/api/v1/[resources]` (Ví dụ: `/api/v1/campaigns`, `/api/v1/leads`).
- **Naming:** Sử dụng danh từ số nhiều, chữ thường (`lowercase`), từ cách nhau bằng dấu gạch ngang (`kebab-case`).
- **HTTP Methods:**
  - `GET`: Truy vấn danh sách hoặc chi tiết dữ liệu (Dự án, Lead, Lịch sử chat).
  - `POST`: Tạo mới các thực thể (Chiến dịch marketing, tin nhắn gửi đi).
  - `PUT/PATCH`: Cập nhật cấu hình hoặc thay đổi trạng thái của lead.
  - `DELETE`: Xóa các bản ghi nháp.

---

### 3.2 RESTful API Specs Chi Tiết Cho Hai API Lõi

#### 3.2.2 [API 1] Tạo mới chiến dịch Marketing tự động (`POST /api/v1/campaigns`)
- **Mô tả chức năng:** Môi giới (Broker) gửi cấu hình dự án để AI Agent tự động lập kế hoạch và sinh bài viết marketing.
- **Headers:**
  | Tên Header | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
  |---|---|---|---|
  | `Authorization` | String | Có | JWT Token định danh Broker dạng: `Bearer [token]` |
  | `Content-Type` | String | Có | Bắt buộc truyền giá trị: `application/json` |

- **Request Body (JSON):**
  | Tên trường | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
  |---|---|---|---|
  | `project_id` | String | Có | ID của dự án bất động sản lưu trong hệ thống. |
  | `campaign_name` | String | Có | Tên hiển thị của chiến dịch marketing. |
  | `target_channels` | Array | Có | Danh sách các kênh chạy marketing (Ví dụ: `["facebook", "zalo"]`). |
  | `max_budget_vnd` | Integer | Không | Hạn mức chi phí tối đa mong muốn (để AI kiểm soát token). |

- **Responses (HTTP Status Codes):**
  - `201 Created`: Tạo chiến dịch thành công và đưa vào hàng đợi AI lập kế hoạch.
  - `403 Forbidden`: Vượt quá hạn mức số lượng dự án/tin nhắn của gói cước hiện tại.
  - `400 Bad Request`: Thiếu trường dữ liệu bắt buộc hoặc dự án không khả dụng.

- **JSON Payloads Ví dụ:**
  *   *Example Request:*
      ```json
      {
        "project_id": "PROJ_VINHOMES_001",
        "campaign_name": "Khai xuân Vinhomes Grand Park - Phân khu Beverly",
        "target_channels": ["facebook", "zalo"],
        "max_budget_vnd": 500000
      }
      ```
  *   *Example Response (201 Created - Success):*
      ```json
      {
        "status": 1,
        "data": {
          "campaign_id": "CAMP_BEVERLY_2026",
          "campaign_name": "Khai xuân Vinhomes Grand Park - Phân khu Beverly",
          "status": "pending_approval",
          "created_at": 1779842400
        },
        "message": "Đã khởi tạo chiến dịch thành công. AI đang lập kế hoạch chi tiết.",
        "meta": {
          "traceId": "trace-8ee6-415f-984a-d5e6e9ccc59b"
        }
      }
      ```
  *   *Example Response (403 Forbidden - Gói cước hết hạn/vượt quota):*
      ```json
      {
        "status": 0,
        "data": null,
        "message": "Không thể tạo chiến dịch. Tài khoản gói Cơ bản đã đạt giới hạn tối đa 1 dự án.",
        "error_code": "VAL_4002",
        "payload": {
          "current_tier": "Basic",
          "limit": 1
        },
        "meta": {
          "traceId": "trace-8ee6-415f-984a-d5e6e9ccc59b"
        }
      }
      ```

---

#### 3.2.3 [API 2] Lấy danh sách khách hàng tiềm năng (`GET /api/v1/leads`)
- **Mô tả chức năng:** Truy vấn danh sách Lead của môi giới kèm các bộ lọc trạng thái và phân hạng tiềm năng.
- **Headers:**
  | Tên Header | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
  |---|---|---|---|
  | `Authorization` | String | Có | JWT Token định danh Broker dạng: `Bearer [token]` |

- **Query Parameters (URL Params):**
  | Tên tham số | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
  |---|---|---|---|
  | `status` | String | Không | Lọc theo trạng thái lead (Ví dụ: `new`, `nurturing`, `hot_lead`, `appointment`). |
  | `min_score` | Integer | Không | Lọc khách hàng có điểm tiềm năng (Lead Score) lớn hơn hoặc bằng. |
  | `page` | Integer | Không | Chỉ số trang cần lấy để phân trang (Mặc định: `0`). |
  | `size` | Integer | Không | Số lượng bản ghi trên một trang (Mặc định: `20`). |

- **Responses (HTTP Status Codes):**
  - `200 OK`: Truy vấn thành công, trả về danh sách kèm phân trang.
  - `401 Unauthorized`: JWT Token không hợp lệ hoặc đã hết hạn.

- **JSON Payloads Ví dụ:**
  *   *Example Request:*
      `GET /api/v1/leads?status=hot_lead&min_score=80&page=0&size=2`
  *   *Example Response (200 OK - Success):*
      ```json
      {
        "status": 1,
        "data": {
          "content": [
            {
              "lead_id": "LEAD_001",
              "full_name": "Trần Thị B",
              "phone": "0901234xxx",
              "score": 92,
              "status": "hot_lead",
              "last_interaction": 1779842400
            },
            {
              "lead_id": "LEAD_002",
              "full_name": "Lê Văn C",
              "phone": "0988776xxx",
              "score": 85,
              "status": "hot_lead",
              "last_interaction": 1779841800
            }
          ],
          "page_info": {
            "current_page": 0,
            "page_size": 2,
            "total_elements": 14,
            "total_pages": 7
          }
        },
        "message": "Truy vấn danh sách lead thành công",
        "meta": {
          "traceId": "trace-8ee6-415f-984a-d5e6e9ccc59b"
        }
      }
      ```

---

## 4. Quản Trị Xác Thực & Bảo Mật (Auth & Security)

### 4.1 Cơ chế xác thực hệ thống ID-system JWT
Property AI sử dụng hệ thống bảo mật đăng nhập xác thực **ID-system** nội bộ. Không sử dụng hoặc lưu trữ các mã khóa/mật khẩu cứng trong mã nguồn:
- **Nguyên lý hoạt động:** Khi Broker đăng nhập thành công bằng tài khoản/mật khẩu, ID-system sẽ cấp một cặp khóa bao gồm `Access Token (JWT)` và `Refresh Token`.
- **Cấu trúc JWT Token:** Chứa các trường thông tin mã hóa:
  - `sub`: ID tài khoản môi giới (`broker_id`).
  - `role`: Nhóm quyền sử dụng (Ví dụ: `ROLE_BROKER`).
  - `tier`: Gói dịch vụ hiện hành (Ví dụ: `STANDARD`).
  - `exp`: Thời hạn hết hạn của token.
- **Xác thực API:** Mọi API gửi từ `web-client` và `pwa-client` bắt buộc phải đính kèm JWT Token vào Header `Authorization: Bearer [token]`. Dịch vụ `product-service` sẽ trực tiếp giải mã token này để phân quyền xử lý dữ liệu.

### 4.2 Bảo mật WebSocket Handshake (`realtime-server`)
- Khi PWA Client thiết lập kết nối Socket.io tới `realtime-server`, bắt buộc phải truyền JWT Token trong tham số `auth` lúc khởi tạo handshake:
  ```javascript
  const socket = io("https://realtime.propertyai.vn", {
    auth: { token: "Bearer " + jwtToken }
  });
  ```
- `realtime-server` sẽ trích xuất token này và gọi dịch vụ xác thực nội bộ của `product-service` để validate trước khi cho phép thiết lập kết nối Socket. Nếu token không hợp lệ hoặc hết hạn, socket sẽ lập tức bị từ chối kết nối (Disconnect).

### 4.3 Tần suất yêu cầu (Rate Limiting)
Để bảo vệ hệ thống trước các cuộc tấn công DDoS và tránh bùng nổ ngân sách API AI, hạn mức request được cấu hình như sau:
- **REST API:** Giới hạn tối đa **100 request/phút** cho mỗi tài khoản Broker (kiểm soát qua địa chỉ IP và JWT Token tại `product-service`).
- **Realtime Chat API:** Giới hạn tối đa **10 tin nhắn/10 giây** cho một hội thoại chat của khách hàng để ngăn chặn spam bot.

---

## Lịch Sử Cập Nhật (Changelog)

| Phiên bản | Ngày | Người cập nhật | Nội dung thay đổi |
|---|---|---|---|
| v1.0 | 2026-05-10 | Admin | Bản thảo khởi tạo ban đầu. |
| v1.1 | 2026-05-26 | Lux - Project-Level Documentation Specialist | Tái cấu trúc thành 4 phần chuẩn mực, cập nhật cơ chế ID-system JWT nội bộ, bổ sung RESTful API Specs chi tiết cho hai API lõi và đồng bộ liên kết chéo. |
