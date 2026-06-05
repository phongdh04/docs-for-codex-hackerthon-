---
name: upload-project-doc
description: Gọi mcp tool `upload_project_doc` từ local-mcp để đẩy tài liệu dự án lên hệ thống.
---

## Description
Kỹ năng giúp Lux gọi MCP tool thực tế **`upload_project_doc`** (thuộc server `local-mcp`) để đẩy nội dung tài liệu cấp dự án đã được User chốt duyệt và lưu local lên server lưu trữ tập trung của hệ thống.

## Triggers
- Kích hoạt tại Bước 6 của quy trình `build-project-doc.md` và `edit-project-doc.md`.
- Điều kiện tiên quyết: Tài liệu đã được ghi local và nhận được sự phê duyệt (confirm) bản thảo đầy đủ từ User.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| projectKey | String | Có | Mã hiệu dự án trên hệ thống (VD: `ECOM`). |
| name | String | Có | Tên file tài liệu (VD: `01-overview.md`). |
| description | String | Có | Mô tả ngắn gọn nội dung tài liệu (tối đa 256 ký tự). |
| content | String | Có | Chuỗi nội dung Markdown hoàn chỉnh của tài liệu. |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| upload_status | String | Kết quả tải lên hệ thống (SUCCESS hoặc FAILED). |

## Steps
1. **Kiểm tra dữ liệu đầu vào:**
   - Xác thực sự hiện diện của đầy đủ 4 tham số yêu cầu (`projectKey`, `name`, `description`, `content`).
   - Đảm bảo `description` không vượt quá 256 ký tự và `content` không bị rỗng.
2. **Gọi công cụ hệ thống:**
   - Thực hiện cuộc gọi tới tool **`upload_project_doc`** truyền đầy đủ 4 tham số.
3. **Xác nhận và báo cáo:**
   - Nhận kết quả phản hồi từ tool.
   - Trả về `upload_status` = `SUCCESS` và thông báo cho User rằng tài liệu đã được tải lên server hệ thống thành công.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Mô tả quá dài | Mô tả vượt quá 256 ký tự | Tự động truncate mô tả còn 250 ký tự kèm dấu "..." và tiếp tục gọi tool. |
| Tool timeout / API lỗi | Sự cố đường truyền hệ thống | Thực hiện Retry gọi tool tối đa 2 lần. Nếu vẫn thất bại, dừng luồng và báo User. |
