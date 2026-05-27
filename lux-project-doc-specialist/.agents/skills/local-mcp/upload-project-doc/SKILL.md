---
name: upload-project-doc
description: Gọi mcp tool upload_project_doc từ local-mcp để đẩy trực tiếp tài liệu dự án lên hệ thống.
---

## Description
Kỹ năng bọc (Wrapper Skill) giúp Lux kết nối trực tiếp với máy chủ `local-mcp` và gọi tool `upload_project_doc` để tải nội dung tài liệu cấp dự án đã được hoàn thiện lên hệ thống lưu trữ tập trung. Không lưu trữ cục bộ tại local workspace.

## Triggers
- Kích hoạt ở Bước 7 của quy trình `documentation-process.md` sau khi bản thảo chuỗi đã được User trực tiếp duyệt thành công trong cuộc chat.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| projectKey | String | Có | Mã hiệu duy nhất của dự án trên hệ thống (VD: `ezisolutions/multi-agent`). |
| name | String | Có | Tên file tài liệu (VD: `01-overview.md`). |
| description | String | Có | Mô tả ngắn gọn nội dung file tài liệu (tối đa 256 ký tự). |
| content | String | Có | Chuỗi nội dung Markdown hoàn chỉnh của tài liệu cần tải lên. |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| upload_status | String | Trạng thái tải lên hệ thống (SUCCESS / FAILED). |

## Steps
1. **Kiểm tra dữ liệu đầu vào:**
   - Xác thực sự hiện diện của đầy đủ 4 tham số yêu cầu (`projectKey`, `name`, `description`, `content`).
   - Đảm bảo `description` không vượt quá 256 ký tự và `content` không bị rỗng.
2. **Gọi công cụ hệ thống:**
   - Gọi tool `upload_project_doc` từ `local-mcp` truyền vào các tham số đã xác thực.
3. **Phản hồi trạng thái:**
   - Nhận kết quả từ tool, xuất thông báo trạng thái tải lên thành công (hoặc thất bại) và gán trạng thái vào biến đầu ra `upload_status`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Thiếu projectKey | Không truyền hoặc không lấy được mã dự án | Dừng luồng upload, quay lại bước context-collector hoặc trực tiếp hỏi User trong chat để bổ sung tham số. |
| Mô tả quá dài | Tham số description vượt quá 256 ký tự | Tự động cắt ngắn (truncate) chuỗi mô tả còn đúng 250 ký tự kèm dấu "..." và tiếp tục gọi tool. |
| Tool timeout / API lỗi | Sự cố đường truyền hệ thống | Thực hiện Retry gọi tool tối đa 2 lần. Nếu vẫn thất bại, thông báo trực tiếp cho User để kiểm tra hệ thống. |
