---
name: verify-project
description: Xác thực sự tồn tại của dự án trên hệ thống thông qua MCP tool `projects_list`.
---

## Description
Kỹ năng giúp Lux kiểm tra xem mã hiệu dự án (`projectKey`) do User cung cấp có tồn tại trong danh sách dự án của hệ thống hay không trước khi bắt đầu bất kỳ quy trình xử lý tài liệu nào.

## Triggers
- Kích hoạt tại Bước 1 của quy trình `build-project-doc.md` và `edit-project-doc.md`.
- Điều kiện tiên quyết: Nhận được mã hiệu dự án từ câu lệnh của User.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| projectKey | String | Có | Mã hiệu duy nhất của dự án cần xác thực (VD: `ECOM`, `PAI`). |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| is_valid | Boolean | Trả về `true` nếu dự án có tồn tại, ngược lại trả về `false`. |

## Steps
1. **Gọi MCP Tool:** Thực hiện gọi MCP tool **`projects_list`** (thuộc server `local-mcp`) để lấy danh sách tất cả các dự án hiện có.
2. **Kiểm tra sự tồn tại:** Đối chiếu tham số `projectKey` đầu vào với danh sách các dự án trả về.
   - Nếu tìm thấy khớp, gán `is_valid` = `true`.
   - Nếu không tìm thấy, gán `is_valid` = `false`.
3. **Phản hồi:** Trả về kết quả xác thực. Nếu `is_valid` = `false`, dừng luồng và thông báo ngay cho User.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Tool timeout / API lỗi | Lỗi kết nối MCP Server | Thực hiện Retry tối đa 2 lần. Nếu vẫn lỗi, báo User và dừng tiến trình. |
| Danh sách trả về trống | Hệ thống chưa cấu hình dự án nào | Gán `is_valid` = `false`, thông báo lỗi chi tiết cho User. |
