---
name: research-project-overview
description: Tìm và lấy nội dung tài liệu tổng quan của dự án (01-overview.md).
---

## Description
Kỹ năng giúp Mattin lấy được bức tranh toàn cảnh về dự án từ hệ thống thông qua `mattin-mcp` để có ngữ cảnh ban đầu (mục tiêu dự án, luồng nghiệp vụ tổng quát) trước khi đối chiếu và review tài liệu chi tiết của BA.

## Triggers
- Được gọi ở Bước 2 của quy trình `review-cycle.md` khi bắt đầu quá trình thẩm định.

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
   - Gọi thẳng tool `get_project_doc_by_name` từ `mattin-mcp` với 2 tham số:
     - `projectKey`: theo ngữ cảnh hiện tại.
     - `name`: Tham số cứng là `"01-overview.md"`.
2. **Đọc tài liệu:**
   - Trích xuất nội dung thu được vào `overview_content` phục vụ cho việc review.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| File Not Found | Chưa setup dự án hoặc thiếu file `01-overview.md` | Kết luận review FAILED ngay lập tức vì dự án chưa được cấu hình tổng quan chuẩn chỉnh. |
| Thiếu projectKey | Không xác định được dự án | Hỏi lại User "Review request này thuộc dự án nào?" |
