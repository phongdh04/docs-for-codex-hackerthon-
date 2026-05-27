---
name: research-project-overview
description: Truy xuất nội dung tài liệu tổng quan dự án (01-overview.md) qua MCP Resources.
---

## Description
Đọc trực tiếp nội dung tài liệu Project Charter (`01-overview.md`) từ máy chủ thông qua địa chỉ URI tĩnh để lấy ngữ cảnh đối chiếu khi review.

## Triggers
- Kích hoạt trong workflow `review-cycle` khi bắt đầu quá trình đối chiếu tài liệu với yêu cầu vĩ mô của dự án.

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
| Resource không tồn tại | Chưa khởi tạo tài liệu `01-overview.md` cho dự án | Thông báo User và dừng quá trình review. |
