---
name: upload-story-doc
description: Đẩy file tài liệu Story đã hoàn thiện lên hệ thống lưu trữ tập trung.
---

## Description
Skill này sử dụng công cụ kết nối hệ thống để upload hoặc cập nhật nội dung của một tài liệu STORY, đảm bảo rằng file được lưu vào hệ thống để Dev/Test có thể đọc được.

## Triggers
- Kích hoạt ở Bước 4 của quy trình `document-revision.md` (Khi chỉnh sửa xong file Story sau feedback).
- Kích hoạt bất cứ khi nào Lina viết xong 1 file Story.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu dự án |
| storyKey | String | Có | Mã định danh của Story |
| name | String | Có | Tên tài liệu (VD: `spec.md` hoặc `ui.md`) |
| content | String | Có | Nội dung đầy đủ của tài liệu (Markdown) |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| status | String | Trạng thái upload (SUCCESS / FAILED) |

## Steps
1. **Kiểm tra dữ liệu hợp lệ:**
   - Xác minh đủ 4 tham số: `projectKey`, `storyKey`, `name`, `content`.
   - Đảm bảo `content` không trống rỗng.
2. **Thực thi gọi MCP tool:**
   - Gọi tool `upload_story_doc` với các params trên.
3. **Ghi nhận:**
   - Báo cáo kết quả upload thành công.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Content Rỗng | Soạn thảo lỗi | Dừng ngay, soạn lại file trước khi upload. |
| Lỗi đường truyền | Do kết nối API | Tự động retry tối đa 3 lần. |
| Nhầm lẫn Key | Gửi `epicKey` vào tham số `storyKey` | Kiểm tra định dạng biến (Story thường khác Epic). Sửa lại tham số và gửi lại. |
