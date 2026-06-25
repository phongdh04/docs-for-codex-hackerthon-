---
name: context-interview
description: Thực hiện phỏng vấn User, quét mã nguồn, tổng hợp và chốt nội dung tóm tắt tài liệu trước khi viết.
---

## Description
Kỹ năng giúp Lux thu thập bối cảnh bằng cách quét mã nguồn cục bộ (nếu có) và lập bộ câu hỏi phỏng vấn tối giản gửi trực tiếp cho User. Sau khi có phản hồi, Lux tổng hợp thông tin, lập tóm tắt nội dung tài liệu chuẩn bị viết và gửi cho User phê duyệt (confirm) trước khi soạn thảo chính thức.

## Triggers
- Kích hoạt tại Bước 3 và Bước 4 của quy trình `build-project-doc.md` và `edit-project-doc.md`.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| guideline_content | Markdown | Có | Nội dung cấu trúc mẫu chuẩn của tài liệu cần viết. |
| existing_doc_content | Markdown | Không | Nội dung tài liệu cũ (nếu là tác vụ chỉnh sửa). |
| targetKey | String | Có | Tên tài liệu cần viết/sửa (VD: `01-overview.md`). |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| summary_content | String | Bản tóm tắt nội dung tài liệu chuẩn bị viết đã được User phê duyệt. |
| collected_context | JSON | Object chứa toàn bộ thông tin nghiệp vụ/kỹ thuật đã chốt với User. |

## Steps
1. **Quét mã nguồn cục bộ (nếu có):**
   - Đọc các cấu hình chính (`package.json`, `mcp_config.json`...) để trích xuất thông tin kỹ thuật thô liên quan đến tài liệu cần biên soạn.
2. **Lập và gửi câu hỏi phỏng vấn:**
   - So sánh bối cảnh thô quét được và tài liệu cũ với guideline chuẩn để tìm khoảng trống thông tin.
   - Biên soạn một bộ câu hỏi phỏng vấn tối giản **dưới 5 câu** gửi trực tiếp cho User trong chat.
   - Nếu User trả lời chưa rõ ràng hoặc có mâu thuẫn nghiệp vụ, tiếp tục hỏi lại để làm rõ.
3. **Biên soạn tóm tắt nội dung:**
   - Sau khi nhận đầy đủ câu trả lời của User, tổng hợp thành một bản tóm tắt nội dung tài liệu ngắn gọn (Summary).
   - Gửi bản tóm tắt này lên chat cho User xác nhận (confirm).
4. **Xác nhận phê duyệt từ User:**
   - Chờ User phản hồi confirm.
   - Nếu User chưa xác nhận hoặc có comment điều chỉnh, thực hiện chỉnh sửa nội dung tóm tắt theo comment (hoặc quay lại phỏng vấn nếu cần làm rõ thêm) cho đến khi User đồng ý confirm.
5. **Bàn giao:** Trả về biến `summary_content` và `collected_context` đã được chốt duyệt.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| User không phản hồi phỏng vấn | Đang chờ thông tin từ User | Dừng tiến trình tạm thời để chờ User cung cấp thông tin, không tự động đi tiếp. |
| Ý kiến điều chỉnh của User mâu thuẫn | User yêu cầu sửa đổi trái ngược | Làm rõ trực tiếp với User bằng câu hỏi lựa chọn đơn giản (A hay B) để đi đến thống nhất. |
