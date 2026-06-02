---
name: write-story-specs
description: Phân rã và viết chi tiết trọn bộ Specs cho một User Story.
---

## Description
Kỹ năng cốt lõi của Lina để biến một yêu cầu cấp độ EPIC thành các tài liệu đặc tả chi tiết (Story, Flow, Data, API, DB) ở cấp độ User Story.

## Triggers
- Kích hoạt trong giai đoạn Chi tiết hóa EPIC (Workflow 2).
- Điều kiện tiên quyết: EPIC đã được PM phê duyệt (nhận được `brief.md` hợp lệ).

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả                                      |
|-----|------|----------|--------------------------------------------|
| epic_brief | Markdown | Có | Tài liệu `brief.md` của EPIC đã được duyệt |
| epicContext | Object | Không | Payload JSON thô chứa bối cảnh Epic và danh sách `repositories` (có thuộc tính `hasUi`) trả về từ skill `get-epic-context`. |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| user_story | Markdown | Mô tả chi tiết hành vi và Acceptance Criteria |
| concept_note | Markdown | Ghi chú yêu cầu UI/UX |
| user_flow | Markdown | Luồng thao tác của người dùng |
| data_dictionary | Markdown | Từ điển dữ liệu nghiệp vụ |
| api_spec | Markdown | Đặc tả API dự kiến |
| db_design | Markdown | Thiết kế cơ sở dữ liệu cơ bản |

## Steps
1. **Phân rã Logic:**
   - Đọc `brief.md` và tách ra thành các User Story độc lập mang lại giá trị trọn vẹn.
2. **Viết Nghiệp vụ & Giao diện:**
   - Viết `user-story.md` (chuẩn As a... I want to... So that...).
   - Viết `user-flow.md` (chỉ rõ luồng rẽ nhánh, lỗi, happy path).
   - Viết `concept_note.md` làm đầu vào cho Designer: Đối chiếu với danh sách `repositories` có `hasUi: true` từ `epicContext` để xác định giao diện của story thuộc repo nào (VD: `web-client` dùng Angular hay `pwa-client` dùng React/Tailwind). Tra cứu đúng tài liệu Design System tương ứng (VD: `stitch.md` hoặc `09_DESIGN_SYSTEM.md`) để biên soạn concept note chuẩn xác.
3. **Viết Kỹ thuật (Dữ liệu):**
   - Định nghĩa `data-dictionary.md`.
   - Phác thảo `api-spec.md` và `db_design.md` mức nghiệp vụ.
4. **Đóng gói file (Tuân thủ Guideline):**
   - **BẮT BUỘC:** Đối với toàn bộ 6 file Spec trên, phải tìm và áp dụng nghiêm ngặt các biểu mẫu (template/guideline) tương ứng của dự án.
   - Sử dụng kỹ năng `[fetch-guideline](../fetch-guideline/SKILL.md)` để truy xuất biểu mẫu chuẩn trên hệ thống.
   - Tuyệt đối không tự sáng tạo ra định dạng hay thay đổi cấu trúc bảng biểu.
5. **Kiểm tra chéo (Cross-check):**
   - Đảm bảo tính nhất quán giữa các file (ví dụ API phải chứa biến định nghĩa trong Data Dictionary).

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Logic mâu thuẫn | Epic brief không bao phủ hết kịch bản | Ghi chú vào file `out_scope_thinking.md` và xin ý kiến PM |
