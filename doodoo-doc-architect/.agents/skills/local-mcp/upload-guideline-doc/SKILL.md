---
name: upload-guideline-doc
description: Gọi mcp tool `upload_guideline_doc` từ local-mcp để đẩy tài liệu guideline lên hệ thống.
---

## Description
Kỹ năng giúp DooDoo gọi MCP tool thực tế **`upload_guideline_doc`** (thuộc server `local-mcp`) để tải nội dung tài liệu guideline mới biên soạn hoặc cập nhật lên server lưu trữ tập trung.

## Triggers
- Kích hoạt tại Bước 4 của quy trình `build-guideline-flow.md`.
- Điều kiện tiên quyết: Guideline đã được viết và hoàn thiện thông qua skill `build_guideline`.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| level | String | Có | Cấp độ của guideline (VD: `PROJECT`, `EPIC`, `STORY`). |
| name | String | Có | Tên guideline (VD: `01-overview.md`). |
| description | String | Có | Mô tả ngắn về tài liệu guideline (tối đa 256 ký tự). |
| content | String | Có | Nội dung chi tiết của guideline cần tải lên. |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| upload_status | String | Kết quả tải lên hệ thống (SUCCESS hoặc FAILED). |

## Steps
1. **Kiểm tra dữ liệu đầu vào:**
   - Xác thực sự hiện diện của đầy đủ 4 tham số yêu cầu (`level`, `name`, `description`, `content`).
   - Đảm bảo `description` không vượt quá 256 ký tự và `content` không bị rỗng.
2. **Gọi công cụ hệ thống:**
   - Gọi tool **`upload_guideline_doc`** truyền đầy đủ các tham số.
3. **Phản hồi trạng thái:**
   - Nhận kết quả từ tool, xuất thông báo trạng thái tải lên thành công (hoặc thất bại) và gán trạng thái vào biến đầu ra `upload_status`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Mô tả quá dài | Mô tả vượt quá 256 ký tự | Tự động truncate mô tả còn 250 ký tự kèm dấu "..." và tiếp tục gọi tool. |
| Tool timeout / API lỗi | Sự cố đường truyền hệ thống | Thực hiện Retry gọi tool tối đa 2 lần. Nếu vẫn thất bại, thông báo trực tiếp cho User. |
