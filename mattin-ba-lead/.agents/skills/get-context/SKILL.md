---
name: get-context
description: Tải nội dung tài liệu gốc cần được review qua MCP Resources.
---

## Description
Đọc trực tiếp nội dung tài liệu đặc tả (Epic Brief, Story Specs) đang chờ được kiểm duyệt từ máy chủ thông qua địa chỉ URI tĩnh của resource để chuẩn bị cho quá trình review.

## Triggers
- Kích hoạt trong workflow `review-cycle` khi bắt đầu quá trình thu thập bối cảnh và tải tài liệu cần review.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| uri | String | Có | Địa chỉ URI tĩnh của tài liệu cần review (VD: `epic-document://PAI-EPIC-23/brief.md` hoặc `story-document://PAI-STORY-05/user-story.md`). |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| document_content | String/Markdown | Nội dung gốc của tài liệu cần review. |

## Steps
1. **Xác thực URI:** Đảm bảo tham số `uri` đầu vào hợp lệ và tuân thủ định dạng `[epic|story]-document://`.
2. **Truy cập Resource:** Gọi lệnh `read_resource(uri)` để tải trực tiếp tài liệu từ server.
3. **Bàn giao:** Nạp nội dung vào context `document_content` chuyển cho skill `review-doc`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|----------------|-------------|------------|
| URI không hợp lệ | Viết sai định dạng URI | Thông báo lỗi chi tiết cho User và dừng workflow. |
| Resource không tồn tại | Tài liệu cần review chưa được upload lên server | Đánh dấu FAIL ngay lập tức, chấm 0 điểm và ghi comment yêu cầu người nộp kiểm tra lại file. |
