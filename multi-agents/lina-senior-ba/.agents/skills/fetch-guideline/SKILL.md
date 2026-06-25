---
name: fetch-guideline
description: Trích xuất biểu mẫu (guideline/template) chuẩn từ hệ thống sử dụng MCP Resources.
---

## Description
Đọc trực tiếp nội dung biểu mẫu (guideline/template) chuẩn của hệ thống thông qua địa chỉ URI tĩnh của resource, đảm bảo tài liệu viết ra tuân thủ tuyệt đối định dạng chuẩn của dự án.

## Triggers
- Kích hoạt khi Agent chuẩn bị biên soạn tài liệu mới (Epic, Story, Spec) mà cần tham chiếu biểu mẫu chuẩn.

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
3. **Bàn giao tri thức:** Nạp nội dung biểu mẫu vào context để chuẩn bị cho bước soạn thảo tài liệu.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|----------------|-------------|------------|
| URI không hợp lệ | Viết sai định dạng URI | Thông báo lỗi chi tiết cho User và dừng workflow. |
| Resource không tồn tại | Trình duyệt/Server chưa định nghĩa file guideline này | Báo cáo User để kiểm tra và bổ sung guideline chuẩn lên hệ thống. |
