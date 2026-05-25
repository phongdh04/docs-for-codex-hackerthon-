---
name: research-db-spec
description: Tra cứu và lấy cấu trúc bảng DB để đối chiếu nghiệp vụ.
---

## Description
Kỹ năng tổ hợp (Composite Skill) giúp Mattin tìm kiếm các tài liệu thiết kế Database (Table Spec) trong dự án dựa trên từ khóa nghiệp vụ, dùng làm bối cảnh đối chiếu xem thiết kế trong Epic/Story của BA có khớp cấu trúc dữ liệu thực tế hay không.

## Triggers
- Kích hoạt ở Bước 2 của quy trình `review-cycle.md`.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu của dự án đang thao tác |
| text | String | Có | Từ khóa nghiệp vụ liên quan (Ví dụ: "payment", "user") |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| db_specs | String/Markdown | Cấu trúc chi tiết các bảng DB tìm thấy để làm căn cứ thẩm định. |

## Steps
1. **Tra cứu Table Spec:**
   - Gọi tool `search_db_table_spec` từ `mattin-mcp` (truyền `projectKey` và `text`).
2. **Kiểm tra kết quả:**
   - Nếu không có kết quả: Trả về `db_specs` rỗng.
   - Nếu có kết quả, chuyển sang bước 3.
3. **Lấy chi tiết DB:**
   - Gọi tool `get_db_table_spec` từ `mattin-mcp` để lấy cấu trúc chi tiết các cột.
4. **Tổng hợp:** Gộp nội dung thu được vào `db_specs`.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Không tìm ra bảng | Từ khóa quá sâu | Thử dùng từ khóa ngắn gọn hơn và tìm lại. Tối đa 2 lần. |
