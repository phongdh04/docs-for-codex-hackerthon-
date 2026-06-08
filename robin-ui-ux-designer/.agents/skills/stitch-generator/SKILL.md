---
name: stitch-generator
description: Sinh và cập nhật giao diện trên StitchMCP, trích xuất Stitch URL, và lưu trữ kết quả lên server qua tool `save_design_screen`.
---

## Description
Kỹ năng giúp Robin soạn thảo prompt thiết kế, lập danh sách các màn hình cần tạo, gọi StitchMCP để sinh và cập nhật giao diện dựa trên feedback của User. Khi User chốt duyệt hoàn toàn thiết kế, Robin gọi MCP tool `save_design_screen` để lưu trữ kết quả từng màn hình lên server và tổng hợp bàn giao.

## Triggers
- Kích hoạt tại Bước 4, Bước 5, và Bước 6 của quy trình `story-design-flow.md`.
- Điều kiện tiên quyết: User đã chốt duyệt tài liệu `concept_note.md` hiệu chỉnh và Stitch config đã sẵn sàng.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| storyKey | String | Có | Mã hiệu Story (VD: `PAI-STORY-23`). |
| stitchProjectId | String | Có | Project ID của Stitch. |
| stitchDesignSystemId | String | Có | Design System ID của Stitch. |
| chot_concept_note | Markdown | Có | Nội dung concept note đã được User chốt duyệt. |
| design_system_spec | Markdown | Có | Đặc tả Design System của repo. |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| screens_list | List | Danh sách các màn hình đã thiết kế và lưu server thành công, cấu trúc: `[{ "name": "tên màn hình", "url": "Stitch URL" }]`. |

## Steps
1. **Lập danh sách màn hình & Soạn Prompt:**
   - Dựa trên `chot_concept_note`, lên danh sách các màn hình và trạng thái giao diện cần thiết kế (VD: màn hình tĩnh, loading, lỗi).
   - Với mỗi màn hình, soạn thảo một prompt kỹ thuật chi tiết tích hợp cấu trúc layout và các tokens thẩm mỹ từ `design_system_spec`.
2. **Sinh giao diện trên Stitch (Bước 4):**
   - Gọi tool `generate_screen_from_text` (hoặc `edit_screens` nếu là cập nhật) của **StitchMCP** truyền vào `stitchProjectId`, `stitchDesignSystemId` và câu prompt đã soạn.
   - Trích xuất đường dẫn liên kết thiết kế (Stitch URL) từ response. Bàn giao link cho User xem xét.
3. **Hiệu chỉnh theo phản hồi (Bước 5):**
   - Nhận feedback chỉnh sửa từ User.
   - Thực hiện cập nhật thiết kế thông qua tool `edit_screens` của StitchMCP cho đến khi User hoàn toàn chốt duyệt và đồng ý với tất cả màn hình.
4. **Lưu trữ kết quả lên Server (Bước 6 - Khi đã chốt hoàn toàn):**
   - Với mỗi màn hình đã hoàn thiện trong danh sách thiết kế:
     - Gọi MCP tool **`save_design_screen`** (thuộc server `local-mcp`) truyền vào các tham số: `storyKey`, `name` (Tên màn hình), và `stitchUrl` (Stitch URL tương ứng) để lưu trữ kết quả thiết kế lên hệ thống.
5. **Đóng gói kết quả:** Tổng hợp và trả về danh sách `screens_list` chứa thông tin các màn hình đã lưu thành công.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Không trích xuất được Stitch URL | Thay đổi cấu trúc response từ StitchMCP | Gọi tool `get_project` từ StitchMCP để lấy danh sách screens của dự án Stitch, định vị đúng screen name để trích xuất link URL tương ứng. |
| Gọi tool `save_design_screen` thất bại | Sự cố kết nối hoặc trùng lặp dữ liệu trên server | Retry gọi tool tối đa 2 lần. Nếu vẫn lỗi, thông báo chi tiết lỗi cho User. |
