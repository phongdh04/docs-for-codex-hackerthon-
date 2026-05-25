---
name: research-api-spec
description: Tra cứu và lấy cấu trúc thiết kế API cũ để tái sử dụng.
---

## Description
Kỹ năng tổ hợp (Composite Skill) giúp Lina tìm kiếm tự động các tài liệu thiết kế API Spec trong dự án dựa trên từ khóa nghiệp vụ, sau đó tải về chi tiết Request/Response để làm bối cảnh kỹ thuật khi viết Epic.

## Triggers
- Kích hoạt ở Bước 1 của quy trình `epic-creation.md` nhằm thu thập các Endpoint đã có sẵn để tái sử dụng hoặc mở rộng.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu của dự án đang thao tác |
| text | String | Có | Từ khóa nghiệp vụ (Ví dụ: "get user", "checkout") |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| api_specs | String/Markdown | Cấu trúc chi tiết của các API tìm thấy. Rỗng nếu không có API nào. |

## Steps
1. **Tra cứu API Spec:**
   - Gọi tool `search_api_spec` (truyền `projectKey` và `text`).
2. **Kiểm tra kết quả:**
   - Nếu không có kết quả: Tính năng chưa có API nào liên quan, trả về rỗng.
   - Nếu có kết quả (nhận được danh sách API Spec), chuyển sang bước 3.
3. **Lấy chi tiết API:**
   - Gọi tool `get_api_spec` với các API lấy được để tải chi tiết cấu trúc Request/Response.
4. **Tổng hợp:** Gom nội dung vào biến `api_specs`.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Không có API nào liên quan | API chưa được thiết kế | Không báo lỗi, xem như đây là tính năng mới hoàn toàn cần Tech Lead thiết kế thêm API sau này. |
