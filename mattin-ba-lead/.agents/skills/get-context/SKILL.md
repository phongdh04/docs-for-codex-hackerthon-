---
name: get-context
description: Tải nội dung tài liệu gốc (Epic, Story, Task) từ hệ thống cần được review.
---

## Description
Kỹ năng giúp Mattin truy xuất nội dung của các tài liệu đặc tả (Epic, Story, Spec) đang chờ được kiểm duyệt từ hệ thống thông qua `mattin-mcp`.

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
| context_content | Markdown | Dữ liệu ngữ cảnh (Requirements, Q&A batch log, Project docs) dùng để đối chiếu |

## Steps
1. **Xác định và trích xuất tài liệu chính:** 
   - Nếu là Epic: Dùng `epic_docs_list`, `get_epic_doc_by_name`  (từ `mattin-mcp`) để lấy các tài liệu gốc của EPIC.
   - Nếu là Story: Dùng `story_docs_list`, `get_story_doc_by_name` (từ `mattin-mcp`) để lấy các tài liệu gốc của STORY.
2. **Trích xuất tài liệu ngữ cảnh đối chiếu:**
   - *Requirement khởi phát*: Dùng `get_ticket` (từ `mattin-mcp`) để lấy các nội dung yêu cầu, nhật ký Q&A của ticket
   - *Tài liệu tổng quan dự án*: Dùng `get_project_doc_by_name` (từ `mattin-mcp`) để lấy tài liệu `01-overview.md` mô tả chung của dự án.
   - *Tài liệu EPIC phụ thuộc (nếu review STORY)*: Dùng `get_epic_doc_by_name` (từ `mattin-mcp`) để lấy tài liệu `brief.md`.
3. **Bàn giao nội dung:**
   - Truyền biến `document_content` và `context_content` cho kỹ năng `review-doc` ở bước tiếp theo để thẩm định xem tài liệu có bám sát yêu cầu gốc hay không.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Không tải được file chính | Sai tên file, file không tồn tại | Chấm FAIL ngay lập tức, ghi comment yêu cầu người nộp kiểm tra lại file. |
| Thiếu tài liệu ngữ cảnh | Chưa có requirement hoặc Q&A | Vẫn tiến hành review cấu trúc nội tại của file chính, nhưng note warning thiếu thông tin đối chiếu nghiệp vụ. |
