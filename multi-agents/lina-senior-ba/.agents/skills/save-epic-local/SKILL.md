---
name: save-epic-local
description: Lưu trữ tài liệu Epic Brief (brief.md) cục bộ dưới dạng file offline trong workspace.
---

## Description
Kỹ năng này giúp Lina lưu tài liệu Epic Brief (`brief.md`) trực tiếp vào thư mục phiên làm việc tương ứng trong workspace thay vì đẩy lên hệ thống qua MCP Tool, giúp quản lý tài liệu ngoại tuyến hoàn toàn.

## Triggers
- Kích hoạt ở bước cuối cùng của quy trình `epic-creation.md` hoặc `document-revision.md`.
- Điều kiện tiên quyết: Yêu cầu và phương án giải pháp đã được User phê duyệt.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu dự án (VD: `PAI`). |
| epicNo | String/Number | Có | Số hiệu phiên làm việc / số thứ tự Epic (VD: `EPIC-46`). |
| content | String | Có | Nội dung Markdown đầy đủ của tài liệu Epic Brief. |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| status | String | Trạng thái lưu file (SUCCESS / FAILED). |
| file_path | String | Đường dẫn tuyệt đối đến file tài liệu được lưu. |

## Steps
1. **Xác định đường dẫn lưu:**
   - Dựng đường dẫn tương đối: `{projectKey}/epic-creation/new-session/{epicNo}/brief.md`.
2. **Lưu file cục bộ:**
   - Tạo thư mục cha nếu chưa tồn tại.
   - Ghi đè trực tiếp nội dung `content` vào file `brief.md` (không thực hiện sao lưu/backup file cũ).
3. **Xác nhận:**
   - Đảm bảo file được lưu thành công vào workspace và sẵn sàng cho việc kiểm tra.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Không tạo được thư mục | Lỗi phân quyền ghi file hệ thống | Báo cáo lỗi ghi file cho User và dừng tiến trình. |
| Nội dung content rỗng | Quá trình soạn thảo spec bị lỗi | Dừng lại, yêu cầu Lina soạn thảo lại nội dung tài liệu trước khi ghi file. |
