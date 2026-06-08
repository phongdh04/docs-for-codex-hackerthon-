---
name: get-story-context
description: Gọi mcp tool `get_story_design_context` để lấy ngữ cảnh của Story và trích xuất các thông tin nghiệp vụ/kỹ thuật.
---

## Description
Kỹ năng giúp Robin truy xuất ngữ cảnh UI/UX của một Story cụ thể thông qua MCP tool `get_story_design_context`. Kết quả JSON được parse để trích xuất `projectKey`, nội dung `concept_note`, tên repository thực thi (`repository_select`), cùng các tài liệu đính kèm liên quan (`user-story.md`, `user-flow.md`).

## Triggers
- Kích hoạt tại Bước 1 của quy trình `story-design-flow.md`.
- Điều kiện tiên quyết: Nhận được mã hiệu story (`storyKey`) và dự án (`projectKey`) từ yêu cầu của User.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| projectKey | String | Có | Mã hiệu của dự án (VD: `PAI`, `ECOM`). |
| storyKey | String | Có | Mã hiệu của Story cần thiết kế (VD: `PAI-STORY-23`). |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| projectKey | String | Mã hiệu dự án trích xuất từ payload (VD: `PAI`). |
| concept_note | Markdown | Nội dung tài liệu concept note do BA viết. |
| repository_select | String | Tên repository được chọn để thiết kế UI (VD: `pwa-client`). |
| user_story | Markdown | Nội dung đặc tả yêu cầu và Acceptance Criteria của Story. |
| user_flow | Markdown | Nội dung sơ đồ luồng điều hướng của người dùng. |
| documents | List | Danh sách đầy đủ các tài liệu thô đính kèm của Story. |

## Steps
1. **Gọi MCP Tool:** Thực hiện gọi MCP tool **`get_story_design_context`** (thuộc server `local-mcp`) truyền vào `projectKey` và `storyKey`.
2. **Xử lý và Trích xuất Payload JSON:**
   - Trích xuất **`projectKey`** trực tiếp từ `data.projectKey` của response.
   - Duyệt qua mảng `data.documents`:
     - Lọc lấy document có `name == "concept_note.md"`, trích xuất `content` của nó làm **`concept_note`**.
     - Lọc lấy document có `name == "user-story.md"`, trích xuất `content` làm **`user_story`**.
     - Lọc lấy document có `name == "user-flow.md"`, trích xuất `content` làm **`user_flow`**.
3. **Parse thông tin Repository:**
   - Phân tích nội dung văn bản của `concept_note` để tìm kiếm dòng chứa thông tin repository thực hiện (Ví dụ: `*   **Repository thực hiện:** `pwa-client``).
   - Trích xuất tên repository đó gán vào biến đầu ra **`repository_select`** (VD: `pwa-client`).
4. **Bàn giao kết quả:** Lưu trữ các biến đầu ra để chuyển giao cho các bước kế tiếp.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Không tìm thấy `concept_note.md` | Story chưa có tài liệu concept note | Báo lỗi ngay cho User và dừng quy trình vì thiếu dữ kiện thiết kế cốt lõi. |
| Không parse được `repository_select` | File concept note không tuân thủ mẫu hoặc thiếu phần khai báo repository | Mặc định hỏi lại User trong chat để xác định rõ repo UI cần thiết kế (React, Angular, hay Mobile), ghi nhận và tiếp tục. |
| Tool timeout / API lỗi | Sự cố kết nối MCP Server | Retry tối đa 2 lần. Nếu vẫn lỗi, báo User và dừng tiến trình. |
