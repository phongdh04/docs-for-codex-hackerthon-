---
name: get-reviewed-request
description: Lấy thông tin về yêu cầu chỉnh sửa (feedback) từ hệ thống.
---

## Description
Kỹ năng giúp Lina tự động lấy về các comment, điểm cần lưu ý do Mattin, David hoặc Leader đánh giá sau khi họ đã review tài liệu. Đây là đầu vào cốt lõi để Lina tiến hành sửa file.

## Triggers
- Kích hoạt đầu tiên ở Bước 1 của quy trình `document-revision.md`.

## Inputs
Không có Input bắt buộc từ người dùng. Tool tự động fetch request đang gán cho Lina.

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| review_feedback | Object/String | Chi tiết các nội dung bị reject, mã tài liệu cần sửa (epicKey/storyKey). |

## Steps
1. **Lấy dữ liệu Review:**
   - Gọi tool `get_reviewed_request` (Không cần truyền parameter).
2. **Kiểm tra dữ liệu:**
   - Trích xuất thông tin lỗi (comment) và các id tài liệu (epicKey, storyKey, projectKey) đang dính lỗi.
   - Gán vào biến `review_feedback` truyền cho luồng sau.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Dữ liệu rỗng | Không có task review nào đang assign | Dừng workflow, báo với User: "Hiện không có tài liệu nào cần sửa từ Reviewer." |
| Thiếu mã tài liệu | Dữ liệu trả về bị hỏng form | Nhắc nhở người dùng cung cấp thêm mã tài liệu bị lỗi để tiến hành sửa chữa. |
