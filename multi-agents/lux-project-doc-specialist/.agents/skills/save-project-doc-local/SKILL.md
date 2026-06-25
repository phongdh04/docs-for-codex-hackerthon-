---
name: save-project-doc-local
description: Ghi tài liệu Markdown đã chốt duyệt xuống local `{projectKey}/{name}` và thực hiện backup file cũ.
---

## Description
Kỹ năng giúp Lux ghi nội dung tài liệu Markdown hoàn chỉnh của cấp dự án xuống workspace cục bộ dưới đường dẫn `{projectKey}/{name}`. Kỹ năng này tự động phát hiện nếu file đã tồn tại và thực hiện đổi tên, sao lưu sang thư mục backup để tránh mất mát dữ liệu lịch sử.

## Triggers
- Kích hoạt tại Bước 5 của quy trình `build-project-doc.md` và `edit-project-doc.md`.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| projectKey | String | Có | Mã hiệu dự án (VD: `ECOM`). |
| name | String | Có | Tên file tài liệu (VD: `01-overview.md`). |
| content | String | Có | Nội dung Markdown hoàn chỉnh của tài liệu cần ghi. |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| save_status | String | Kết quả ghi file (`SUCCESS` hoặc `FAILED`). |
| file_path | String | Đường dẫn lưu file thực tế (VD: `ECOM/01-overview.md`). |

## Steps
1. **Chuẩn bị thư mục:**
   - Xác định đường dẫn file đích: `{projectKey}/{name}` (ví dụ: `ECOM/01-overview.md`).
   - Đảm bảo thư mục `{projectKey}` và `{projectKey}/backup` tồn tại (hoặc tự động tạo nếu chưa có).
2. **Thực hiện Backup nếu file đã tồn tại:**
   - Kiểm tra xem file `{projectKey}/{name}` đã tồn tại ở local chưa.
   - Nếu đã tồn tại:
     - Tạo timestamp hiện tại định dạng `YYYYMMDD_HHMMSS`.
     - Đổi tên file cũ và di chuyển vào thư mục backup với tên mới dạng: `{projectKey}/backup/[name].[timestamp].bak` (ví dụ: `ECOM/backup/01-overview.md.20260605_100000.bak`).
3. **Ghi file mới:**
   - Ghi chuỗi `content` Markdown mới vào file `{projectKey}/{name}`.
4. **Xác nhận và trình User:**
   - Trả về `save_status` = `SUCCESS` và đường dẫn file thực tế `file_path`.
   - Gửi bản thảo đầy đủ lên chat cho User review và chốt duyệt lần cuối.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Lỗi ghi file (Permission denied) | Quyền ghi thư mục bị chặn hoặc folder không hợp lệ | Thông báo cho User lỗi phân quyền chi tiết, yêu cầu cấp quyền hoặc kiểm tra lại workspace. |
| Không tạo được thư mục backup | Thiếu quyền tạo thư mục | Bỏ qua bước backup hoặc lưu file tạm thời sang thư mục an toàn khác trong workspace và thông báo cho User. |
