---
name: upload-epic-doc
description: Đẩy file tài liệu Epic Brief hoặc Epic Spec đã hoàn thiện lên hệ thống lưu trữ tập trung.
---

## Description
Skill này sử dụng công cụ kết nối hệ thống để upload hoặc cập nhật nội dung của một tài liệu EPIC, đảm bảo rằng file luôn nằm trên nguồn dữ liệu duy nhất của công ty (Single Source of Truth).

## Triggers
- Kích hoạt ở Bước 6 của quy trình `epic-creation.md` (Khi tạo xong tài liệu Epic mới).
- Kích hoạt ở Bước 3 của quy trình `document-revision.md` (Khi chỉnh sửa xong file Epic sau feedback).
- Điều kiện tiên quyết: File tài liệu đã được soạn thảo xong và có độ dài hợp lý.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu dự án |
| epicKey | String | Có | Mã định danh của Epic (ví dụ: EPIC-001) |
| name | String | Có | Tên tài liệu (VD: `brief.md` hoặc `spec.md`) |
| content | String | Có | Nội dung đầy đủ của tài liệu (Markdown) |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| status | String | Trạng thái upload (SUCCESS / FAILED) |

## Steps
1. **Kiểm tra dữ liệu hợp lệ:**
   - Xác minh đủ 4 tham số: `projectKey`, `epicKey`, `name`, `content`.
   - Đảm bảo `content` không trống rỗng.
2. **Thực thi gọi MCP tool:**
   - Gọi tool `upload_epic_doc` với các params trên.
3. **Ghi nhận:**
   - Báo cáo kết quả "Đã upload thành công" cho quá trình làm việc tiếp theo.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| File Rỗng | Do luồng soạn thảo bị lỗi trước đó | Dừng upload, quay lại yêu cầu hệ thống soạn thảo lại nội dung file. |
| Token Timeout | Mất kết nối mạng với hệ thống | Tự động retry tối đa 3 lần, mỗi lần cách nhau 5 giây. Nếu vẫn fail thì dừng và thông báo. |
| Thiếu Key | Quên lấy projectKey hoặc epicKey | Kiểm tra lại dữ kiện từ context hiện tại trước khi đẩy. |
