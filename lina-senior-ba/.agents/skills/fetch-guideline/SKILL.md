---
name: fetch-guideline
description: Trích xuất biểu mẫu (guideline/template) chuẩn từ hệ thống để chuẩn bị viết tài liệu.
---

## Description
Kỹ năng giúp Lina tự động tìm kiếm, phân loại và tải về các biểu mẫu chuẩn (guidelines) từ hệ thống thông qua các tool của `lina-mcp`, đảm bảo mọi tài liệu sinh ra đều tuân thủ định dạng gốc của tổ chức.

## Triggers
- Bất cứ khi nào Agent cần xuất bản một tài liệu mới (Epic, Story, Spec, v.v.) mà chưa nắm rõ biểu mẫu.
- Điều kiện tiên quyết: Xác định được tên hoặc cấp độ (level) của tài liệu cần viết.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| doc_level | String | Có | Cấp độ tài liệu (VD: EPIC, STORY) |
| doc_name | String | Không | Tên cụ thể của tài liệu nếu cần lấy đích danh |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| guideline_content | Markdown | Nội dung định dạng chuẩn của biểu mẫu |

## Steps
1. **Tra cứu danh sách Guideline:**
   - Sử dụng `guideline_levels_list` hoặc `guidelines_list` để xem danh sách các cấp độ/biểu mẫu khả dụng trên hệ thống.
2. **Trích xuất nội dung Guideline:**
   - Sử dụng `get_guideline_by_level` nếu chỉ cần lấy theo cấp độ tài liệu chung.
   - Hoặc sử dụng `get_guideline_by_level_and_name` nếu cần lấy chính xác theo một biểu mẫu cụ thể.
3. **Bàn giao biểu mẫu:**
   - Đọc hiểu biểu mẫu và truyền lại cho quá trình soạn thảo tài liệu (write-specs).

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Không tìm thấy guideline | Sai tên level hoặc hệ thống chưa có | Dừng việc tạo file, hỏi lại User và ghi chú chờ bổ sung guideline |
