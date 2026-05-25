---
name: research-db-spec
description: Tra cứu và lấy cấu trúc bảng DB phục vụ tính năng mới.
---

## Description
Kỹ năng tổ hợp (Composite Skill) giúp Lina tìm kiếm tự động các tài liệu thiết kế Database (Table Spec) trong dự án dựa trên từ khóa nghiệp vụ, sau đó tải về chi tiết các cột/kiểu dữ liệu để làm ngữ cảnh viết Epic.

## Triggers
- Kích hoạt khi phân tích tính năng mới, cần biết bảng DB nào sẽ liên quan hoặc đã tồn tại.
- Kích hoạt ở Bước 1 của quy trình `epic-creation.md`.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu của dự án đang thao tác |
| text | String | Có | Từ khóa nghiệp vụ (Ví dụ: "payment", "user profile") |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| db_specs | String/Markdown | Chi tiết cấu trúc các bảng DB tìm thấy. Rỗng nếu không có. |

## Steps
1. **Tra cứu Table Spec:**
   - Gọi tool `search_db_table_spec` (truyền `projectKey` và `text`).
2. **Kiểm tra kết quả:**
   - Nếu không có kết quả: Trả về `db_specs` rỗng. Có thể bảng này chưa từng được thiết kế.
   - Nếu có kết quả (nhận được danh sách tên/Key của Table Spec), chuyển sang bước 3.
3. **Lấy chi tiết DB:**
   - Ứng với mỗi Table tìm được, gọi tool `get_db_table_spec` (có thể cần truyền `projectKey` và tên/id table tương ứng tùy schema của tool).
4. **Tổng hợp:** Gộp nội dung thiết kế thu được vào `db_specs`.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Rỗng/Lỗi từ khóa | Tìm không ra do từ khóa quá chi tiết | Thử cắt bớt từ khóa (tìm từ khóa ngắn/chung chung hơn) và gọi lại `search_db_table_spec`. Tối đa 2 lần thử. |
| Thiếu tham số | Không truyền đủ `projectKey` | Kiểm tra lại context trước khi chạy công cụ. |
