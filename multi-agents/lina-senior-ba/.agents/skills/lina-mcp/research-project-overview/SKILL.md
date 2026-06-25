---
name: research-project-overview
description: Truy xuất nội dung tài liệu tổng quan dự án (01-overview.md) qua MCP Resources.
---

## Description
Đọc trực tiếp nội dung tài liệu Project Charter (`01-overview.md`) từ máy chủ thông qua địa chỉ URI tĩnh của resource để nắm bắt bức tranh tổng quan của dự án.

## Triggers
- Kích hoạt ở Bước 1 của Workflow `epic-creation.md` để lấy ngữ cảnh ban đầu khi phân tích tính năng mới.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu dự án (VD: `PAI`). |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| overview_content | String/Markdown | Nội dung chi tiết tài liệu tổng quan dự án. |

## Steps
1. **Xác định địa chỉ URI:** Thiết lập URI tĩnh: `project-document://[projectKey]/01-overview.md`.
2. **Truy cập Resource:** Gọi lệnh `read_resource(uri)` để tải tài liệu `01-overview.md` từ server.
3. **Bàn giao:** Nạp nội dung tài liệu vào context `overview_content`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|----------------|-------------|------------|
| Resource không tồn tại | Chưa khởi tạo tài liệu `01-overview.md` cho dự án | Thông báo User: *"Dự án này chưa được thiết lập tài liệu 01-overview.md. Vui lòng tạo tài liệu này trước."* và dừng workflow. |
