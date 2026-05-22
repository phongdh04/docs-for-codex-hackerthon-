---
name: get-document
description: Tải nội dung tài liệu gốc (Epic, Story, Task) từ hệ thống cần được review.
---

## Description
Kỹ năng giúp Mattin trích xuất nội dung của các tài liệu kỹ thuật (Epic, Story, Spec) đang chờ được kiểm duyệt từ hệ thống thông qua `mattin-mcp`.

## Triggers
- Kích hoạt trong quá trình chạy workflow `review-cycle`, tại bước Thu thập dữ liệu.
- Điều kiện tiên quyết: Biết được tên tài liệu hoặc mã của tài liệu thông qua `get_review_request`.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| doc_type | String | Có | Loại tài liệu (VD: epic, story, task) |
| doc_name | String | Có | Tên hoặc Key của tài liệu cần review |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| document_content | Markdown | Nội dung gốc của tài liệu cần review |

## Steps
1. **Xác định loại tài liệu:** 
   - Dựa vào `doc_type` để quyết định gọi công cụ MCP nào.
2. **Trích xuất nội dung:**
   - Nếu là Epic: Dùng `get_epic_doc_by_name`.
   - Nếu là Story/Spec: Dùng `get_story_doc_by_name`.
   - Nếu là Task: Dùng `get_task_doc_by_name`.
   - Các tài liệu chung khác có thể dùng `search_semantic` nếu cần.
3. **Bàn giao nội dung:**
   - Truyền biến `document_content` này cho kỹ năng `review_doc` ở bước tiếp theo để thẩm định.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Không tải được file | Sai tên file, file không tồn tại | Chấm FAIL ngay lập tức, ghi comment yêu cầu người nộp kiểm tra lại file. |
