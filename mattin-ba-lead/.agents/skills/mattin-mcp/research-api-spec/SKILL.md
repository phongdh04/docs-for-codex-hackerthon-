---
name: research-api-spec
description: Tra cứu và lấy cấu trúc thiết kế API cũ để đối chiếu nghiệp vụ.
---

## Description
Kỹ năng tổ hợp (Composite Skill) giúp Mattin tự động tìm kiếm các thiết kế API Spec trong hệ thống theo nghiệp vụ, dùng để thẩm định xem đặc tả API của BA viết trong tài liệu có khớp hoặc tái sử dụng chuẩn xác hay không.

## Triggers
- Kích hoạt ở Bước 2 của quy trình `review-cycle.md`.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu của dự án đang thao tác |
| text | String | Có | Từ khóa nghiệp vụ (Ví dụ: "auth", "payment") |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| api_specs | String/Markdown | Cấu trúc chi tiết các API phục vụ review đối chiếu. |

## Steps
1. **Tra cứu API Spec:**
   - Gọi tool `search_api_spec` từ `mattin-mcp` (truyền `projectKey` và `text`).
2. **Kiểm tra kết quả:**
   - Nếu không có kết quả: Trả về `api_specs` rỗng.
   - Nếu có kết quả, chuyển sang bước 3.
3. **Lấy chi tiết API:**
   - Gọi tool `get_api_spec` từ `mattin-mcp` để lấy chi tiết Request/Response.
4. **Tổng hợp:** Gộp nội dung thu được vào `api_specs`.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Không tìm thấy | API mới | Trả về rỗng, ghi nhận không có thiết kế cũ đối chiếu. |
