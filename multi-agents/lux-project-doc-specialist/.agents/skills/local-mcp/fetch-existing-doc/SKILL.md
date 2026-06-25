---
name: fetch-existing-doc
description: Đọc nội dung tài liệu hiện tại của dự án qua `read_resource`, báo lỗi nếu không tìm thấy.
---

## Description
Kỹ năng giúp Lux truy xuất nội dung hiện có của một file tài liệu cấp dự án từ server bằng lệnh `read_resource`. Nếu tài liệu này không tồn tại, Lux sẽ báo lỗi cho User và dừng quy trình (đảm bảo tính toàn vẹn thông tin khi chỉnh sửa).

## Triggers
- Kích hoạt tại Bước 2 của quy trình `edit-project-doc.md`.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| projectKey | String | Có | Mã hiệu dự án (VD: `ECOM`). |
| name | String | Có | Tên tài liệu cần tải (VD: `01-overview.md`). |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| existing_doc_content | Markdown | Nội dung Markdown hiện tại của tài liệu trên hệ thống. |

## Steps
1. **Chuẩn bị URI:** Xác định URI tĩnh của tài liệu cấp dự án cần tải có định dạng: `project-document://[projectKey]/[name]` (ví dụ: `project-document://ECOM/01-overview.md`).
2. **Tải tài liệu:** Thực hiện lệnh **`read_resource`** để đọc nội dung file từ hệ thống.
3. **Xử lý kết quả:**
   - Nếu tìm thấy và tải thành công nội dung, gán vào biến đầu ra `existing_doc_content`.
   - Nếu tài liệu không tồn tại hoặc nội dung rỗng: Ngay lập tức dừng luồng, thông báo cho User rằng tài liệu chưa tồn tại trên hệ thống và không thể tiến hành chỉnh sửa (đối với quy trình chỉnh sửa tài liệu).

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Tài liệu không tồn tại / URI lỗi | Tài liệu chưa từng được tạo trên hệ thống | Thông báo lỗi chi tiết ra chat cho User và dừng quy trình ngay lập tức. |
| Tool timeout / API lỗi | Sự cố kết nối server | Retry tối đa 2 lần. Nếu vẫn thất bại, dừng luồng và báo User. |
