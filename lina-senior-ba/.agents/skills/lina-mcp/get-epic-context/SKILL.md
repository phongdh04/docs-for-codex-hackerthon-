---
name: get-epic-context
description: Truy xuất toàn bộ context của một Epic từ hệ thống qua tool `get_epic_context` để làm nền tảng phân tích trước khi viết Story hoặc cập nhật tài liệu.
---

## Description
Truy xuất đầy đủ ngữ cảnh của một Epic cụ thể — bao gồm thông tin tổng quan Epic, danh sách Stories hiện có, các tài liệu đính kèm (brief.md, ...), và danh sách các repository của dự án kèm thông tin giao diện (`hasUi`) — qua một lần gọi MCP tool duy nhất. Kết quả cung cấp dữ liệu đầu vào chuẩn hóa giúp BA định vị chính xác công nghệ UI và Design System tương ứng cho từng User Story khi biên soạn kịch bản và Concept Note.

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
| repositories | List | Danh sách các repository của dự án kèm thông tin: tên repo (`repoName`), mô tả (`description`), và thuộc tính nhận diện có UI hay không (`hasUi`). |
| epicContext | Object | Toàn bộ payload JSON thô trả về từ tool, dùng làm nguồn tham chiếu chung cho các kỹ năng kế tiếp. |

## Steps
1. **Truy xuất context Epic:** Gọi MCP tool **`get_epic_context`** với tham số `epicKey` do User cung cấp.
2. **Phân tích payload:** Đọc JSON trả về và trích xuất các trường:
   - `data.title` → `epicTitle`
   - `data.status` → `epicStatus`
   - `data.documents` → `epicDocuments` (duyệt qua mảng, lọc lấy nội dung `brief.md` ưu tiên)
   - `data.stories` → `storyList` (danh sách `storyKey`, `title`, `status`)
   - `data.repositories` → `repositories` (danh sách các repo, lọc lấy thông tin `repoName`, `description`, `hasUi`)
3. **Định hướng viết Concept Note & Story Specs:**
   - BA rà soát danh sách `repositories` để lọc ra các repo có giao diện người dùng (`hasUi: true`).
   - Phân tích thuộc tính công nghệ của từng repo UI dựa trên mô tả của nó (VD: `web-client` dùng Angular, `pwa-client` dùng React/Tailwind).
   - Khi phân rã User Story có giao diện, BA bắt buộc phải xác định giao diện đó sẽ thuộc repo UI nào. 
   - Từ repo UI được chọn, BA tra cứu đúng tài liệu Design System tương ứng (VD: `stitch.md` cho Angular của `web-client`, hoặc `09_DESIGN_SYSTEM.md` cho React/Tailwind của `pwa-client`) để biên soạn `concept_note.md` chuẩn xác và đồng bộ.
4. **Đồng bộ bộ nhớ hoạt động:** Lưu toàn bộ `epicContext` và các biến đã trích xuất vào bộ nhớ làm việc, sẵn sàng cung cấp đầu vào cho kỹ năng `write-story-specs` hoặc `write-epic-specs`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Epic không tồn tại | `epicKey` sai hoặc Epic chưa được tạo trên hệ thống | Dừng thực thi, thông báo User kiểm tra lại mã Epic và cung cấp `epicKey` hợp lệ. |
| Payload trả về rỗng hoặc thiếu trường | Epic chưa có tài liệu hoặc Stories nào | Ghi nhận tình trạng Epic rỗng, thông báo User và đề xuất tạo `brief.md` trước. |
| Thiếu thông tin Repositories | Server MCP không trả về cấu trúc repo | Ghi nhận lỗi hệ thống, tạm thời đối chiếu với danh mục repo tĩnh trong `05-repositories-registry.md` để tự xác định `hasUi` và tiếp tục quy trình. |
| Tool timeout / API lỗi | Sự cố kết nối MCP Server | Retry tối đa 2 lần. Nếu vẫn thất bại, thông báo User và dừng tiến trình. |
