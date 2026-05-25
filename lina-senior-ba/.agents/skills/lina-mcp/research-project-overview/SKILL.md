---
name: research-project-overview
description: Tìm và lấy nội dung tài liệu tổng quan của dự án (01-overview.md).
---

## Description
Kỹ năng giúp Lina lấy được bức tranh toàn cảnh về dự án để lấy ngữ cảnh ban đầu (mục tiêu dự án, luồng nghiệp vụ tổng quát) trước khi tiến hành viết các tính năng chi tiết.

## Triggers
- Luôn được gọi ưu tiên đầu tiên ở Bước 1 của Workflow `epic-creation.md` khi bắt đầu phân tích bất kỳ tính năng mới nào.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu của dự án đang thao tác |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| overview_content | Markdown | Nội dung chi tiết tài liệu tổng quan dự án |

## Steps
1. **Lấy tài liệu Overview trực tiếp:**
   - Gọi thẳng tool `get_project_doc_by_name` với 2 tham số:
     - `projectKey`: theo ngữ cảnh hiện tại.
     - `name`: Tham số cứng là `"01-overview.md"`.
2. **Đọc tài liệu:**
   - Trích xuất nội dung thu được vào `overview_content` để phục vụ cho các bước phân tích sau.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| File Not Found | Hệ thống chưa setup dự án hoặc thiếu file `01-overview.md` | Báo cáo trực tiếp lên màn hình cho User: "Dự án này chưa được thiết lập tài liệu tổng quan (01-overview.md). Vui lòng setup dự án trước khi tôi có thể viết Epic." và DỪNG Workflow. |
| Thiếu projectKey | Không xác định được dự án | Hỏi lại User "Bạn đang muốn làm tính năng này cho dự án nào?" |
