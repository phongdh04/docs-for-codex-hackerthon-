---
name: get-epic-context
description: Truy xuất toàn bộ context của một Epic từ hệ thống qua tool `get_epic_context` để làm nền tảng phân tích trước khi viết Story hoặc cập nhật tài liệu.
---

## Description
Truy xuất đầy đủ ngữ cảnh của một Epic cụ thể — bao gồm thông tin tổng quan Epic, danh sách Stories hiện có và các tài liệu đính kèm (brief.md, ...) — qua một lần gọi MCP tool duy nhất. Kết quả là dữ liệu đầu vào chuẩn hóa sẵn sàng cung cấp cho các kỹ năng phân rã Story hoặc biên soạn tài liệu Epic.

## Triggers
- Khi User yêu cầu phân tích hoặc bổ sung Story cho một Epic cụ thể.
- Khi cần nắm bối cảnh Epic trước khi viết hoặc cập nhật bất kỳ tài liệu nào thuộc Epic đó.
- Khi cần đối chiếu ngữ cảnh Epic để đảm bảo tính nhất quán khi soạn thảo Specs mới.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả |
|-----|--------------|----------|-------|
| epicKey | String | Có | Mã hiệu Epic cần truy xuất ngữ cảnh (VD: `PAI-EPIC-46`). |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả |
|-----|--------------|-------|
| epicTitle | String | Tiêu đề của Epic. |
| epicStatus | String | Trạng thái hiện tại của Epic (VD: `IN_PROGRESS`, `DONE`). |
| epicDocuments | Markdown | Danh sách các tài liệu đính kèm của Epic (bao gồm nội dung `brief.md` nếu có). |
| storyList | List | Danh sách các Story thuộc Epic kèm `storyKey`, tiêu đề và trạng thái. |
| epicContext | Object | Toàn bộ payload JSON thô trả về từ tool, dùng làm nguồn tham chiếu chung cho các kỹ năng kế tiếp. |

## Steps
1. **Truy xuất context Epic:** Gọi MCP tool **`get_epic_context`** với tham số `epicKey` do User cung cấp.
2. **Phân tích payload:** Đọc JSON trả về và trích xuất các trường:
   - `data.title` → `epicTitle`
   - `data.status` → `epicStatus`
   - `data.documents` → `epicDocuments` (duyệt qua mảng, lọc lấy nội dung `brief.md` ưu tiên)
   - `data.stories` → `storyList` (danh sách `storyKey`, `title`, `status`)
3. **Đồng bộ bộ nhớ hoạt động:** Lưu toàn bộ `epicContext` vào bộ nhớ làm việc, sẵn sàng làm đầu vào cho kỹ năng `write-story-specs` hoặc `write-epic-specs`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Epic không tồn tại | `epicKey` sai hoặc Epic chưa được tạo trên hệ thống | Dừng thực thi, thông báo User kiểm tra lại mã Epic và cung cấp `epicKey` hợp lệ. |
| Payload trả về rỗng hoặc thiếu trường | Epic chưa có tài liệu hoặc Stories nào | Ghi nhận tình trạng Epic rỗng, thông báo User và đề xuất tạo `brief.md` trước. |
| Tool timeout / API lỗi | Sự cố kết nối MCP Server | Retry tối đa 2 lần. Nếu vẫn thất bại, thông báo User và dừng tiến trình. |
