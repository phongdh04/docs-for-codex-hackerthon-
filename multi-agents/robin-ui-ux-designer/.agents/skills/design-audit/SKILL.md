---
name: design-audit
description: Rà soát chéo concept_note với Design System, giải quyết xung đột với User và cập nhật lên server qua tool `edit_concept_note`.
---

## Description
Kỹ năng giúp Robin đối chiếu ý tưởng thiết kế trong `concept_note` với đặc tả Design System (`design_system_spec`). Nếu phát hiện xung đột hoặc thiếu dữ kiện, Robin sẽ phỏng vấn trực tiếp User (tối giản dưới 5 câu). Khi User đã chốt phương án và nội dung, Robin hiệu chỉnh lại tài liệu `concept_note.md` cục bộ và gọi MCP tool `edit_concept_note` để đồng bộ lên server.

## Triggers
- Kích hoạt tại Bước 2 và Bước 3 của quy trình `story-design-flow.md`.
- Điều kiện tiên quyết: Đã tải xong bối cảnh Story và đặc tả Design System.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| projectKey | String | Có | Mã hiệu dự án (VD: `PAI`). |
| storyKey | String | Có | Mã hiệu Story (VD: `PAI-STORY-23`). |
| concept_note | Markdown | Có | Nội dung concept note thô nhận được từ BA. |
| design_system_spec | Markdown | Có | Đặc tả Design System quy chuẩn của repo. |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| chot_concept_note | Markdown | Nội dung concept note hoàn chỉnh đã được User chốt duyệt và lưu local/server. |

## Steps
1. **Rà soát & Đối chiếu chéo:**
   - So sánh các mô tả giao diện trong `concept_note` (colors, typography, spacing, rounded corners) với các token quy chuẩn của Design System.
   - Ghi nhận các điểm mâu thuẫn (Ví dụ: Concept note yêu cầu dùng màu khác với primary color thương hiệu, hoặc cỡ chữ không chuẩn).
2. **Thảo luận và phỏng vấn User:**
   - Lập bộ câu hỏi phỏng vấn tối giản **dưới 5 câu** chỉ ra các mâu thuẫn và đề xuất phương án sửa đổi gửi User.
   - Tiếp tục trao đổi chốt phương án nếu User có ý kiến đóng góp hoặc mâu thuẫn chưa được giải quyết.
3. **Hiệu chỉnh tài liệu và Ghi local:**
   - Khi User đã chốt duyệt phương án thiết kế, Robin hiệu chỉnh lại nội dung tài liệu concept note cho đồng bộ và lưu cục bộ dưới tên `concept_note.md` trong workspace.
4. **Cập nhật lên Server:**
   - Gọi MCP tool **`edit_concept_note`** truyền vào các tham số: `projectKey`, `storyKey`, và `conceptNote` (nội dung concept note đã hiệu chỉnh) để đồng bộ dữ liệu lên server.
5. **Bàn giao kết quả:** Gán nội dung đã chốt vào biến đầu ra `chot_concept_note`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| User chốt phương án phá vỡ Design System | User yêu cầu tùy biến đặc thù có chủ đích | Chấp thuận chỉ đạo của User (User chốt là tối cao), ghi chú bối cảnh quyết định kiến trúc và tiếp tục quy trình. |
| Gọi tool `edit_concept_note` fail | Sự cố kết nối hoặc lỗi server | Retry tối đa 2 lần. Nếu vẫn lỗi, thông báo lỗi cho User để kiểm tra kết nối MCP server. |
