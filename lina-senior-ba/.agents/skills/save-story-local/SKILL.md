---
name: save-story-local
description: Lưu trữ bộ 6 files đặc tả User Story Specs cục bộ dưới dạng file offline trong workspace.
---

## Description
Kỹ năng này giúp Lina lưu toàn bộ 6 file tài liệu đặc tả chi tiết của một User Story (`user-story`, `concept_note`, `user-flow`, `data-dictionary`, `api-spec`, `db_design`) trực tiếp vào thư mục tương ứng trong workspace thay vì đẩy lên hệ thống qua MCP Tool.

## Triggers
- Kích hoạt ở bước cuối cùng của quy trình `epic-detailing.md` hoặc `document-revision.md` sau khi hoàn tất soạn thảo specs.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu dự án (VD: `PAI`). |
| epicKey | String | Có | Mã định danh Epic chứa Story (VD: `PAI-EPIC-46`). |
| storyKey | String | Có | Mã định danh Story cần lưu (VD: `US-01`). |
| specs_data | Map | Có | Cấu trúc map chứa tên file và nội dung Markdown tương ứng của 6 file đặc tả. |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| status | String | Trạng thái lưu file (SUCCESS / FAILED). |
| folder_path | String | Đường dẫn tuyệt đối đến thư mục chứa bộ Specs của Story. |

## Steps
1. **Xác định đường dẫn lưu:**
   - Dựng đường dẫn thư mục lưu trữ: `{projectKey}/epic-detailing/{epicKey}/{storyKey}/`.
2. **Lưu các file đặc tả:**
   - Tạo thư mục đích nếu chưa tồn tại.
   - Duyệt qua `specs_data` và ghi đè nội dung vào từng file Markdown tương ứng:
     - `user-story.md`
     - `concept_note.md`
     - `user-flow.md`
     - `data-dictionary.md`
     - `api-spec.md`
     - `db_design.md`
3. **Xác nhận:**
   - Báo cáo đường dẫn thư mục đã lưu thành công các tài liệu trong workspace.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Không ghi được file | Quyền ghi thư mục bị chặn | Báo cáo lỗi ghi hệ thống cho User. |
| Thiếu file đặc tả trong specs_data | BA quên soạn thảo một trong các file specs bắt buộc | Dừng lại, yêu cầu BA bổ sung đầy đủ 6 file specs rồi thực hiện ghi lại. |
