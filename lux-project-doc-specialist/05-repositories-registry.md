# 05-repositories-registry.md (DANH MỤC MÃ NGUỒN & CẤU TRÚC)

## 1. Danh Bạ Repositories (Repositories Registry)

Dự án Property AI (PAI) được phát triển theo kiến trúc dịch vụ chuyên biệt (Microservices/Modular Monolith), chia tách rõ ràng trách nhiệm giữa Core Business, Frontend Clients, AI Service và Background Service. Toàn bộ mã nguồn dự án được lưu trữ tại 7 repositories chính dưới đây:

| Tên Repository | Công nghệ chính | Cú pháp khởi chạy | Vai trò & Mục đích (Purpose) |
|---|---|---|---|
| **`product-service`** | Spring Boot 3.x, Maven, Java 17 | `mvn spring-boot:run` | Cung cấp RESTful API nghiệp vụ lõi (quản lý dự án BĐS, quản lý lead, tài khoản môi giới). |
| **`web-client`** | React 18 + Vite + PWA, NPM | `npm run dev` | Giao diện quản trị CMS chuyên nghiệp trên PC dành cho Sàn BĐS và Admin hệ thống. |
| **`pwa-client`** | React 18 + Vite + PWA, NPM | `npm run dev` | Ứng dụng PWA Client di động mượt mà dành riêng cho Broker quản lý lead và chat 24/7. |
| **`ai-agents`** | Spring AI 2.0.0-M, Maven, Java 17 | `mvn spring-boot:run` | Tác nhân thông minh xử lý suy nghĩ, hội thoại với khách hàng, tự động kéo sự kiện (pull-events) tin nhắn Broker gửi đến từ Kafka. |
| **`ai-mcp-server`** | Spring Boot 3.x, Maven, Java 17 | `mvn spring-boot:run` | Đóng vai trò làm cổng cung cấp công cụ thực thi (MCP tools/functions) khi LLM từ `ai-agents` thực hiện request tools; truy xuất trực tiếp DB MySQL. |
| **`realtime-server`** | Node.js v20 + Socket.io, NPM | `node server.js` | Xử lý truyền thông tin chat thời gian thực, live reload và đẩy thông báo đẩy (FCM). |
| **`background-service`** | Spring Boot 3.x, Spring Kafka | `mvn spring-boot:run` | Triển khai các tác vụ ngầm xử lý công việc ngoài AI (quét lead MXH, lập lịch, lắng nghe hàng đợi Kafka/Redis). |

---

## 2. Tiêu Chí Phân Luồng Task (Task Routing / Usage Guidelines)

Để đảm bảo quá trình phát triển mã nguồn diễn ra trôi chảy và không chồng chéo trách nhiệm giữa các lập trình viên hoặc AI Agent, quy định phân luồng task được thực hiện nghiêm ngặt như sau:

- **Khi có Task thay đổi hoặc thêm mới REST API nghiệp vụ (VD: Thêm trường cho Lead, đổi logic phân hạng):**
  - Checkout và viết code tại repository: **`product-service`**.
  - *Lưu ý:* Phải kiểm tra quyền xác thực qua ID-system JWT Token.
- **Khi có Task thiết kế giao diện CMS, bảng điều khiển admin trên PC:**
  - Checkout và phát triển tại repository: **`web-client`**.
  - *Lưu ý:* Đảm bảo tính responsive trên màn hình lớn.
- **Khi có Task phát triển giao diện Chat, quản lý Lead di động cho môi giới:**
  - Checkout và phát triển tại repository: **`pwa-client`**.
  - *Lưu ý:* Tối ưu hóa các tính năng ngoại tuyến (Offline cache) của PWA.
- **Khi có Task phát triển Prompt, logic suy nghĩ của Agent, điều phối hội thoại (AI Agent Core):**
  - Checkout và phát triển tại repository: **`ai-agents`**.
  - *Lưu ý:* Thiết kế prompt an toàn, kiểm tra tính tương thích của Spring AI v2.0.0-M.
- **Khi có Task cấu hình, thêm mới công cụ thực thi (MCP Tools) để Agent gọi (AI Tool Server):**
  - Checkout và phát triển tại repository: **`ai-mcp-server`**.
  - *Lưu ý:* Đảm bảo kiểm tra đầu vào nghiêm ngặt trước khi tương tác trực tiếp với DB MySQL.
- **Khi có Task tối ưu hóa tốc độ chat, cấu hình Socket.io hoặc cơ chế thông báo Notification:**
  - Checkout và phát triển tại repository: **`realtime-server`**.
- **Khi có Task lập lịch quét dữ liệu tự động, lắng nghe Kafka Topic hoặc đồng bộ cơ sở dữ liệu ngầm (ngoài AI):**
  - Checkout và phát triển tại repository: **`background-service`**.

---

## 3. Cấu Trúc Thư Mục Lõi (Core Folder Structure)

Dưới đây là cấu trúc thư mục tiêu chuẩn của các nhóm repository chính nhằm duy trì tính đồng nhất trong mã nguồn:

### 3.1 Cấu trúc Backend (`product-service` & `background-service`)
```text
src/
├── main/
│   ├── java/com/propertyai/
│   │   ├── config/          ← Cấu hình hệ thống (JWT, Security, Redis, Kafka)
│   │   ├── controller/      ← Định nghĩa các endpoints RESTful API
│   │   ├── service/        ├── interface và Service implementations (Logic nghiệp vụ)
│   │   ├── repository/      ← Tương tác với MySQL (Spring Data JPA / Hibernate)
│   │   ├── model/           ← Các Entity, Document và DTO models
│   │   ├── consumer/        ← Lắng nghe Kafka messages (chỉ có trong background-service)
│   │   └── PropertyAiApp.java
│   └── resources/
│       ├── application.yml  ← File cấu hình môi trường
│       └── logback-spring.xml ← Cấu hình log hệ thống
```

### 3.2 Cấu trúc AI Agent Core (`ai-agents`)
```text
src/
├── main/
│   ├── java/com/propertyai/
│   │   ├── config/          ← Cấu hình Spring AI, Kafka, Redis
│   │   ├── agent/           ← Định nghĩa Prompt, logic suy nghĩ của các tác nhân (AI Agents)
│   │   ├── consumer/        ← Lắng nghe Kafka messages (Chat events từ Broker/Khách hàng)
│   │   ├── model/           ← Các DTO, Message models và Logs
│   │   └── PropertyAiAgentApp.java
│   └── resources/
│       ├── application.yml  ← File cấu hình môi trường và API Keys
│       └── prompts/         ← Chứa các file prompt template (.txt, .md)
```

### 3.3 Cấu trúc Frontend PWA Client (`web-client` & `pwa-client`)
```text
src/
├── assets/                  ← Hình ảnh, logo, icon tĩnh
├── components/              ← Các UI Components dùng chung (Button, Card, Input)
├── contexts/                ← Quản lý State toàn cục (AuthContext, ChatContext)
├── hooks/                   ← Custom hooks xử lý logic UI
├── pages/                   ← Các màn hình chính (Dashboard, Leads, Chat, Project)
├── services/                ← Gọi REST API tới product-service (sử dụng Axios)
├── utils/                   ← Các helper functions (Format tiền tệ, thời gian)
├── App.jsx
├── main.jsx
└── registerServiceWorker.js ← Đăng ký Service Worker phục vụ tính năng PWA
```

### 3.4 Cấu trúc Realtime Server (`realtime-server`)
```text
realtime-server/
├── config/                  ← Cấu hình Socket.io và kết nối Redis Pub-Sub
├── handlers/                ← Xử lý sự kiện kết nối tin nhắn (ChatHandler, NotifyHandler)
├── middleware/              ← Validate JWT Token qua ID-system trước khi handshake
├── package.json
└── server.js                ← Điểm khởi chạy Socket.io server
```

---

## 4. Quy Chuẩn Nhánh & Commit (Branching & Commit Convention)

### 4.1 Quy chuẩn Nhánh (Branching Strategy)
Dự án áp dụng chặt chẽ quy trình **GitFlow Standard**:
- Nhánh **`main`**: Nhánh bảo vệ nghiêm ngặt, chỉ chứa mã nguồn cực kỳ ổn định phục vụ triển khai lên môi trường Production.
- Nhánh **`develop`**: Nhánh tích hợp chính. Mọi tính năng sau khi code xong phải merge vào nhánh này để QA kiểm thử trên môi trường Staging.
- Nhánh **`feature/[TaskID]-[slug]`**: Nhánh phát triển tính năng mới (Ví dụ: `feature/PAI-102-chat-pwa`).
- Nhánh **`hotfix/[TaskID]-[slug]`**: Nhánh sửa lỗi khẩn cấp trực tiếp từ `main`.

### 4.2 Quy chuẩn Commit (Commit Message Standards)
Commit Message phải tuân thủ nghiêm ngặt chuẩn **Conventional Commits**:
- Cấu trúc: `<type>(scope): [TaskID] - <subject>`
- Danh sách các `type` được phép sử dụng:
  - `feat`: Thêm tính năng mới (Ví dụ: `feat(chat): PAI-104 - add offline message queue for pwa-client`).
  - `fix`: Sửa lỗi (Ví dụ: `fix(auth): PAI-99 - fix JWT token validation exception`).
  - `refactor`: Tối ưu hóa cấu trúc code cũ nhưng không thay đổi tính năng.
  - `docs`: Cập nhật tài liệu (Ví dụ: `docs(guideline): PAI-100 - update repo structure spec`).
  - `chore`: Các tác vụ nhỏ khác liên quan đến cấu hình build, package.

---

## Lịch Sử Cập Nhật (Changelog)

| Phiên bản | Ngày | Người cập nhật | Nội dung thay đổi |
|---|---|---|---|
| v1.0 | 2026-05-10 | Admin | Bản thảo khởi tạo ban đầu. |
| v1.1 | 2026-05-26 | Lux - Project-Level Documentation Specialist | Cập nhật cấu trúc danh bạ 7 repositories thực tế (bổ sung `ai-agents` độc lập), tiêu chí phân luồng task chi tiết, thay đổi DB sang MySQL, sơ đồ thư mục lõi Backend/AI-Agents/Frontend/Realtime và đồng bộ liên kết chéo. |
