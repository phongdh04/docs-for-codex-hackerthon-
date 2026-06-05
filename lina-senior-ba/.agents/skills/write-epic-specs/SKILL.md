---
name: write-epic-specs
description: Viết tài liệu tổng quan (Brief) cho EPIC bao gồm bối cảnh, mục tiêu và danh sách Story dự kiến.
---

## Description
Kỹ năng giúp Lina viết tài liệu `brief.md` cho một EPIC dựa trên các yêu cầu thô đã được phân tích và làm rõ. Tài liệu này đóng vai trò kim chỉ nam trước khi tiến hành phân rã chi tiết.

## Triggers
- Kích hoạt trong giai đoạn Khởi tạo EPIC (Workflow 1).
- Điều kiện tiên quyết: Yêu cầu thô đã được làm rõ thông qua quá trình Q&A và rà soát hệ thống hoàn tất.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| requirement_info | Text | Có | Nội dung Q&A đã được làm rõ sau bước lấy yêu cầu. |
| approved_scope_table | Markdown Table | Có | Bảng danh sách Epic/Story sơ bộ đã được User phê duyệt ở bước trước. |
| semantic_search_result | Text | Không | Thông tin EPIC/Story cũ nếu có tái sử dụng |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| epic_brief | Markdown | File `brief.md` hoàn chỉnh chứa bảng danh sách Epic/Story đã duyệt và tuân thủ định dạng guideline |

## Steps
1. **Phân tích thông tin đầu vào:**
   - Đối chiếu nội dung đã chốt từ Q&A.
   - Xác định rõ mục tiêu cốt lõi của EPIC (Business Goal).
2. **Soạn thảo Nội dung:**
   - Viết phần giới thiệu chung.
   - Nhúng trực tiếp bảng danh sách các Epic/Story đã được phê duyệt (`approved_scope_table`) vào tài liệu làm phần cốt lõi của tài liệu Epic Brief (`brief.md`).
   - Xác định quyền hạn và người dùng mục tiêu (Persona).
3. **Đóng gói file (Tuân thủ Guideline):**
   - **BẮT BUỘC:** Tham chiếu và áp dụng nghiêm ngặt định dạng Markdown chuẩn theo các biểu mẫu (template/guideline) của dự án trên hệ thống. 
   - Sử dụng kỹ năng `[fetch-guideline](../fetch-guideline/SKILL.md)` để lấy chính xác định dạng quy định cho file EPIC brief.
   - Tuyệt đối không tự ý bịa ra cấu trúc mới.
   - Bàn giao lại file `brief.md` chuẩn bị upload.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Thiếu thông tin trọng yếu | Kết quả Q&A chưa đủ chi tiết | Quay lại dùng skill `requirement-clarification` để làm rõ |
