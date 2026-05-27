---
name: fetch-existing-docs
description: Tải toàn bộ nội dung các file tài liệu hiện có của dự án từ máy chủ làm cơ sở đối chiếu nhất quán và cập nhật.
---

## Description
Kỹ năng tổ hợp (Composite Skill) giúp Lux tự động kết nối với máy chủ `local-mcp`, truy vấn danh mục các file tài liệu hiện tại của một dự án cụ thể, sau đó tải về toàn bộ nội dung chi tiết của từng file. Dữ liệu này dùng làm cơ sở đối chiếu nhất quán và xác định phần nội dung thay đổi (delta) khi cần cập nhật.

## Triggers
- Kích hoạt ở Bước 1 của quy trình `documentation-process.md` song song với việc fetch guideline, hoặc khi có yêu cầu cập nhật tài liệu đã tồn tại.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết                                        |
|-----|--------------|----------|-------------------------------------------------------|
| projectKey | String | Có | Mã hiệu của dự án cần tải tài liệu (VD: `PAI`,`ECOM`). |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| existing_docs | JSON | Object chứa key là tên file tài liệu (VD: `01-overview.md`) và value là nội dung Markdown chi tiết của file đó. |

## Steps
1. **Truy vấn danh sách tài liệu hiện có:**
   - Gọi tool `project_docs_list` từ `local-mcp` với tham số `projectKey` đã nhận từ đầu vào.
   - Thu về danh sách các file tài liệu đang được lưu trữ trên server cho dự án này.
2. **Kéo chi tiết nội dung từng file:**
   - Duyệt qua danh sách file tài liệu thu được ở Bước 1.
   - Với mỗi file, gọi tool `get_project_doc_by_name` từ `local-mcp` truyền vào `projectKey` và `name` (tên file tương ứng).
3. **Đóng gói dữ liệu:**
   - Hợp nhất toàn bộ tên file và nội dung chi tiết của chúng thành một đối tượng JSON có cấu trúc rõ ràng: `{ "tên_file.md": "nội_dung_markdown" }`.
   - Lưu trữ dữ liệu này vào biến đầu ra `existing_docs` để chuyển giao cho các bước đối chiếu nhất quán (Bước 5) và xác định delta cập nhật trong workflow.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| ProjectKey không tồn tại | Input bị sai hoặc dự án chưa khởi tạo | Thông báo lỗi chi tiết cho User và dừng workflow. |
| Danh sách trống | Dự án mới hoàn toàn, chưa có tài liệu nào | Trả về object rỗng `{}` và ghi nhận bối cảnh là "Viết mới hoàn toàn". Không báo lỗi ra màn hình. |
| Tool timeout / API lỗi | Lỗi kết nối hệ thống | Retry gọi tool tối đa 2 lần. Nếu vẫn thất bại, thông báo trực tiếp cho User để hỗ trợ kiểm tra đường truyền. |
