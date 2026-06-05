---
name: fetch-guideline-context
description: Đọc nội dung guideline cũ từ hệ thống qua `read_resource` làm bối cảnh đối chiếu.
---

## Description
Kỹ năng giúp DooDoo đọc nội dung của một guideline cũ đã tồn tại trên hệ thống qua lệnh `read_resource`. Điều này giúp DooDoo nắm bắt bối cảnh nghiệp vụ cũ, đối chiếu và bổ sung thông tin thay vì viết trùng lặp hoặc mâu thuẫn chéo.

## Triggers
- Kích hoạt tại Bước 2 của quy trình `build-guideline-flow.md`.
- Điều kiện tiên quyết: Nhận được yêu cầu xây dựng/cập nhật guideline cụ thể từ User.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| level | String | Có | Cấp độ của guideline (VD: `PROJECT`, `EPIC`, `STORY`). |
| name | String | Có | Tên của file guideline cần lấy (VD: `01-overview.md`). |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| existing_guideline_content | Markdown | Nội dung chi tiết của guideline đã tồn tại trên hệ thống. Trả về rỗng nếu chưa tồn tại guideline này. |

## Steps
1. **Chuẩn bị URI:** Xác định URI tĩnh để đọc guideline mẫu có định dạng: `guideline://[level]/[name]` (ví dụ: `guideline://PROJECT/01-overview.md`).
2. **Đọc Resource:** Thực hiện lệnh **`read_resource`** để truy xuất nội dung guideline từ hệ thống.
3. **Bàn giao kết quả:** Gán nội dung đọc được vào biến đầu ra `existing_guideline_content` phục vụ đối chiếu và merge delta.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Guideline chưa tồn tại | Đây là guideline mới hoàn toàn | Trả về chuỗi rỗng `""` và ghi nhận bối cảnh là "Xây dựng guideline mới". Không báo lỗi dừng luồng. |
| Tool timeout / API lỗi | Sự cố kết nối MCP Server | Retry tối đa 2 lần. Nếu vẫn thất bại, dừng luồng và báo User. |
