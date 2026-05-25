---
name: get-review-request
description: Lấy thông tin yêu cầu review tài liệu từ hệ thống.
---

## Description
Kỹ năng giúp Mattin tự động truy xuất các yêu cầu thẩm định tài liệu (Review Request) đang được gán cho anh từ hệ thống thông qua `mattin-mcp`.

## Triggers
- Kích hoạt đầu tiên ở Bước 1 của quy trình `review-cycle.md` khi có lệnh review từ người dùng.

## Inputs
Không có Input bắt buộc từ người dùng. Hệ thống tự động fetch các review request tương ứng của Mattin.

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| review_requests | Array / JSON | Danh sách các yêu cầu review, chứa `reviewKey`, `projectKey`, `epicKey`/`storyKey`, `status`... |

## Steps
1. **Lấy danh sách yêu cầu review:**
   - Gọi tool `get_review_request` từ `mattin-mcp` (Không cần truyền parameter).
2. **Trả về kết quả:**
   - Lưu trữ và phân tích danh sách các yêu cầu trong `review_requests`.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Không có yêu cầu review | Không có task nào được giao | Báo lại cho User: "Không tìm thấy yêu cầu review nào được gán cho tôi." và DỪNG workflow. |
