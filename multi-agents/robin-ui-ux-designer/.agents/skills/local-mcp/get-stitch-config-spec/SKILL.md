---
name: get-stitch-config-spec
description: Gọi mcp tool `get_stitch_config` để lấy cấu hình Stitch (`stitchProjectId`, `stitchDesignSystemId`) của repository.
---

## Description
Kỹ năng giúp Robin tải thông tin Project ID và Design System ID của Stitch tương ứng với repository UI của dự án thông qua MCP tool `get_stitch_config`. Hai tham số cấu hình này sẽ được sử dụng làm đầu vào bắt buộc để sinh giao diện qua StitchMCP.

## Triggers
- Kích hoạt tại Bước 1.3 của quy trình `story-design-flow.md`.
- Điều kiện tiên quyết: Đã trích xuất thành công `projectKey` và `repository_select`.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| projectKey | String | Có | Mã hiệu dự án (VD: `PAI`). |
| name | String | Có | Tên repository (VD: `pwa-client`). |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| stitchProjectId | String | Project ID của Stitch dành cho repo UI được chọn. |
| stitchDesignSystemId | String | Design System ID của Stitch dành cho repo UI được chọn. |

## Steps
1. **Gọi MCP Tool:** Gọi mcp tool **`get_stitch_config`** (thuộc server `local-mcp`) truyền vào 2 tham số `projectKey` và `name`.
2. **Trích xuất thông số:** Parse response trả về và trích xuất hai tham số:
   - `stitchProjectId` (hoặc `projectId` từ response, map sang `stitchProjectId`).
   - `stitchDesignSystemId` (hoặc `designSystemId` từ response, map sang `stitchDesignSystemId`).
3. **Bàn giao kết quả:** Lưu trữ `stitchProjectId` và `stitchDesignSystemId` vào bộ nhớ hoạt động để sẵn sàng gọi StitchMCP.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Không tìm thấy cấu hình Stitch | Repo chưa được liên kết Stitch | Tạm dừng, báo cáo User và yêu cầu cung cấp ID thủ công (hoặc đọc từ file `STITCH.md` backup cục bộ) để ghi cấu hình và đi tiếp. |
| Tool timeout / API lỗi | Sự cố kết nối MCP Server | Retry tối đa 2 lần. Nếu vẫn thất bại, dừng luồng và báo User. |
