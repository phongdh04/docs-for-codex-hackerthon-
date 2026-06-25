---
name: fetch-guideline
description: Trích xuất biểu mẫu (guideline/template) chuẩn từ hệ thống qua MCP Resources làm căn cứ review.
---

## Description
Đọc trực tiếp nội dung biểu mẫu (guideline/template) chuẩn của hệ thống thông qua địa chỉ URI tĩnh của resource, làm căn cứ cứng (hệ quy chiếu) để soi lỗi và thẩm định tài liệu của BA.

## Triggers
- Kích hoạt trong workflow `review-cycle` khi bắt đầu quá trình thu thập bối cảnh và hệ quy chiếu review.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| uri | String | Có | Địa chỉ URI tĩnh của biểu mẫu cần đọc (VD: `guideline://STORY/api-spec.md`). |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| guideline_content | String/Markdown | Nội dung định dạng chuẩn của biểu mẫu guidelines. |

## Steps
1. **Xác thực URI:** Đảm bảo tham số `uri` đầu vào hợp lệ và tuân thủ định dạng `guideline://[LEVEL]/[filename]`.
2. **Truy cập Resource:** Gọi lệnh `read_resource(uri)` để tải trực tiếp nội dung biểu mẫu chuẩn từ máy chủ.
3. **Bàn giao:** Nạp nội dung biểu mẫu vào context phục vụ cho quá trình so sánh, review tài liệu của BA.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|----------------|-------------|------------|
| URI không hợp lệ | Viết sai định dạng URI | Thông báo lỗi chi tiết cho User và dừng workflow. |
| Resource không tồn tại | Hệ thống chưa định nghĩa form chuẩn | Đánh dấu FAIL cho quá trình lấy mẫu, bắt BA bổ sung chuẩn trước khi review. |
