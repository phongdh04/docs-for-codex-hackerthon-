---
name: fetch-project-guideline
description: Tải guideline mẫu chuẩn của tài liệu cấp dự án (PROJECT) qua `read_resource`.
---

## Description
Kỹ năng giúp Lux đọc guideline mẫu chuẩn của tài liệu cụ thể ở cấp dự án từ hệ thống qua lệnh `read_resource` để nắm được cấu trúc 5 tiểu mục bắt buộc và tiêu chuẩn trình bày cần thiết.

## Triggers
- Kích hoạt tại Bước 2 của quy trình `build-project-doc.md` và `edit-project-doc.md`.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| name | String | Có | Tên file tài liệu cần lấy guideline (VD: `01-overview.md`). |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| guideline_content | Markdown | Nội dung guideline mẫu chuẩn của tài liệu cấp dự án. |

## Steps
1. **Chuẩn bị URI:** Xác định URI tĩnh để đọc guideline mẫu có định dạng: `guideline://PROJECT/[name]` (ví dụ: `guideline://PROJECT/01-overview.md`).
2. **Đọc Resource:** Thực hiện lệnh **`read_resource`** với URI đã chuẩn bị để tải guideline mẫu chuẩn cấp dự án.
3. **Bàn giao kết quả:** Gán nội dung đọc được vào biến đầu ra `guideline_content` để phục vụ đối chiếu và lập cấu trúc tài liệu.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Resource không tồn tại | Tên tài liệu `name` sai hoặc guideline chưa cấu hình | Thông báo ngay cho User và dừng tiến trình. |
| Lỗi đường truyền / API | Lỗi hệ thống MCP | Retry tối đa 2 lần. Nếu vẫn lỗi, dừng tiến trình và báo User. |
