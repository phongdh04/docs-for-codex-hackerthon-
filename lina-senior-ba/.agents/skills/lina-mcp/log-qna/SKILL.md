---
name: log-qna
description: Ghi chú lại toàn bộ lịch sử hỏi đáp (Q&A) đã chốt với khách hàng vào hệ thống.
---

## Description
Kỹ năng giúp Lina lưu lại biên bản/log của quá trình làm rõ yêu cầu (Q&A) lên hệ thống, đảm bảo có bằng chứng (traceability) cho các quyết định nghiệp vụ sau này.

## Triggers
- Kích hoạt ở Bước 4 của quy trình `epic-creation.md` khi khách hàng/User đã chốt xong các câu trả lời và Lina không còn gì để hỏi thêm.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu của dự án |
| content | String | Có | Đoạn văn bản chứa các câu hỏi và câu trả lời đã chốt |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| status | String | Trạng thái ghi log (Success/Fail) |

## Steps
1. **Kiểm tra Input:**
   - Xác minh `projectKey` đã có chưa.
   - Đảm bảo `content` không bị trống rỗng.
2. **Thực thi gọi tool:**
   - Gọi tool MCP `log_qna` truyền vào 2 tham số trên.
3. **Trả về kết quả:** 
   - Thông báo cho User rằng lịch sử Q&A đã được lưu thành công.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Tool lỗi hoặc Time out | Hệ thống nghẽn | Tự động retry gọi lại tool `log_qna` tối đa 3 lần. Nếu vẫn thất bại, thông báo cho User. |
| Thiếu content | Biến content truyền vào bị rỗng | Cảnh báo nội bộ, tổng hợp lại các đoạn chat Q&A trước đó để lấy content rồi mới gọi lại tool. |
