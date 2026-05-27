---
name: update-review-request
description: Cập nhật kết quả review tài liệu (PASSED/FAILED) lên hệ thống.
---

## Description
Kỹ năng giúp Mattin đẩy kết quả thẩm định cuối cùng lên hệ thống lưu trữ tập trung của dự án thông qua `local-mcp`, ghi nhận trạng thái duyệt (APPROVED/REJECTED) và đính kèm biên bản báo cáo lỗi chi tiết.

## Triggers
- Kích hoạt ở Bước 4 của quy trình `review-cycle.md` sau khi chấm điểm và đóng gói báo cáo xong.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| reviewKey | String | Có | Mã hiệu định danh của review request |
| status | String | Có | Trạng thái duyệt (`APPROVED` hoặc `REJECTED`) |
| comment | String | Có | Báo cáo chi tiết lỗi (Markdown) theo chuẩn định dạng |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| update_status | String | Trạng thái cập nhật (SUCCESS/FAILED) |

## Steps
1. **Kiểm tra dữ liệu hợp lệ:**
   - Đảm bảo có đầy đủ `reviewKey`, `status`, và `comment`.
   - `status` bắt buộc phải là `APPROVED` (nếu điểm >= 90) hoặc `REJECTED` (nếu điểm < 90).
2. **Gọi tool hệ thống:**
   - Gọi tool `update_review_request` từ `local-mcp` truyền vào 3 tham số trên.
3. **Báo cáo kết quả:**
   - Trả về thông tin cập nhật thành công lên màn hình cho User.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Gửi thất bại | Lỗi kết nối API hệ thống | Retry gọi lại tool tối đa 3 lần. Nếu vẫn lỗi, báo lỗi trực tiếp cho người dùng. |
| Thiếu tham số | Quên truyền `reviewKey` | Kiểm tra lại dữ liệu từ context ban đầu để trích xuất `reviewKey`. |
