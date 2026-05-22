---
name: fetch-guideline
description: Trích xuất biểu mẫu (guideline/template) chuẩn từ hệ thống để làm hệ quy chiếu chấm điểm.
---

## Description
Kỹ năng giúp Mattin tự động tìm kiếm và tải về các biểu mẫu chuẩn (guidelines) từ hệ thống thông qua các tool của `mattin-mcp`. Dữ liệu này dùng làm căn cứ cứng (hệ quy chiếu) để soi lỗi tài liệu của Lina.

## Triggers
- Kích hoạt trong quá trình chạy workflow `review-cycle`, tại bước Thu thập dữ liệu.
- Điều kiện tiên quyết: Nhận được yêu cầu review một loại tài liệu cụ thể.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| doc_level | String | Có | Cấp độ tài liệu (VD: EPIC, STORY) |
| doc_name | String | Không | Tên cụ thể của tài liệu cần lấy biểu mẫu |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| guideline_content | Markdown | Nội dung định dạng chuẩn của biểu mẫu |

## Steps
1. **Truy xuất nội dung Guideline:**
   - Sử dụng tool `get_guideline_by_level_and_name` thuộc `mattin-mcp` để lấy biểu mẫu tương ứng với loại tài liệu cần review.
   - Hoặc các công cụ tìm kiếm guideline khác nếu có.
2. **Bàn giao biểu mẫu:**
   - Đọc hiểu biểu mẫu và truyền biến `guideline_content` này cho kỹ năng `review_doc` ở bước tiếp theo.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Không tìm thấy guideline | Hệ thống chưa định nghĩa form | Đánh dấu FAIL cho quá trình lấy mẫu, bắt Lina bổ sung chuẩn trước khi review. |
