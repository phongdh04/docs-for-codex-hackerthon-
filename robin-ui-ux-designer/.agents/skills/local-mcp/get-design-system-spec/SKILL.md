---
name: get-design-system-spec
description: Gọi mcp tool `get_design_system` để lấy đặc tả Design System tương ứng của repository UI.
---

## Description
Kỹ năng giúp Robin tải tài liệu đặc tả Design System (colors, typography, components, rules) tương ứng với repository UI được chọn của dự án thông qua MCP tool `get_design_system`, đảm bảo thiết kế tuân thủ 100% nhận diện thương hiệu.

## Triggers
- Kích hoạt tại Bước 1.2 của quy trình `story-design-flow.md`.
- Điều kiện tiên quyết: Đã trích xuất thành công `projectKey` và `repository_select` từ kỹ năng `get-story-context`.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| projectKey | String | Có | Mã hiệu của dự án (VD: `PAI`). |
| name | String | Có | Tên repository (chính là `repository_select`, VD: `pwa-client`). |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| design_system_spec | Markdown | Đặc tả Design System chi tiết của repository. |

## Steps
1. **Gọi MCP Tool:** Gọi mcp tool **`get_design_system`** (thuộc server `local-mcp`) truyền vào 2 tham số `projectKey` và `name`.
2. **Nhận kết quả:** Trích xuất tài liệu thiết kế từ payload trả về.
3. **Bàn giao kết quả:** Gán vào biến đầu ra `design_system_spec` để làm cơ sở đối chiếu rà soát thiết kế ở bước tiếp theo.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Design System trống/lỗi | Repo chưa được cấu hình design system | Báo cáo User, đề xuất sử dụng Design System mặc định của dự án (`DESIGN.md` cục bộ) để tiếp tục thiết kế. |
| Tool timeout / API lỗi | Sự cố kết nối MCP Server | Retry tối đa 2 lần. Nếu vẫn thất bại, dừng luồng và báo User. |
