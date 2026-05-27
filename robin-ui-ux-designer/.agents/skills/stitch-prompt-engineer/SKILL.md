---
name: stitch-prompt-engineer
description: Viết prompt Stitch có cấu trúc dựa trên DESIGN.md và quản lý cấu hình STITCH.md cục bộ.
---

## Description
Kỹ năng giúp Robin đọc và trích xuất cấu trúc ý tưởng từ `concept_note.md`, kết hợp với quy chuẩn thiết kế trong `DESIGN.md` để soạn thảo prompt kỹ thuật chi tiết. Đồng thời, quản lý và lưu trữ thông tin cấu hình dự án (`projectId`, `designSystemId`) thông qua file `STITCH.md` cục bộ để tái sử dụng.

## Triggers
- Kích hoạt ở Bước 3 của quy trình `design-creation-process.md` sau khi ý tưởng thiết kế đã được User chốt duyệt.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| concept_note | String | Có | Nội dung ý tưởng thô từ `concept_note.md`. |
| design_system | String | Có | Các quy chuẩn thiết kế từ `DESIGN.md`. |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| stitch_prompt | String | Câu prompt kỹ thuật có cấu trúc dùng cho Stitch. |
| stitch_response | JSON | Phản hồi từ tool StitchMCP chứa thông tin màn hình được tạo. |

## Steps
1. **Kiểm tra Cấu hình Dự án (STITCH.md local):**
   - Robin tiến hành đọc file `STITCH.md` cục bộ trong workspace của mình.
   - Nếu file chưa tồn tại hoặc thiếu các tham số `projectId`/`designSystemId`:
     - Tạm dừng và gửi câu hỏi trực tiếp cho User để xin thông tin: *"Để khởi tạo thiết kế, vui lòng cung cấp Project ID hoặc Design System ID của dự án Stitch của bạn."*
     - Sau khi nhận được phản hồi từ User, tiến hành ghi và lưu lại các thông tin này vào file `STITCH.md` cục bộ theo định dạng YAML/Markdown để tái sử dụng cho các lần sau.
2. **Soạn thảo Design Prompt:**
   - Hợp nhất cấu trúc đề xuất từ `concept_note` (Khu vực Top, Main, Side, Bottom) và các yêu cầu tương tác.
   - Nhúng nghiêm ngặt các quy tắc thẩm mỹ từ `DESIGN.md` (Light Theme: primary `#d83a1a`, secondary `#f5a623`, rounded corners `12px`, font `Be Vietnam Pro`, card styles).
3. **Sinh giao diện trên Stitch:**
   - Gọi tool `generate_screen_from_text` (hoặc `edit_screens` nếu là cập nhật) của **StitchMCP**, truyền vào các tham số `projectId`, `designSystemId` đã lấy từ `STITCH.md` và câu `stitch_prompt` vừa soạn thảo.
   - Nhận kết quả phản hồi từ tool lưu vào `stitch_response`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|----------------|-------------|------------|
| Thiếu tham số Stitch | Không đọc được projectId từ file local và User không cung cấp | Báo lỗi chi tiết cho User và dừng workflow. |
| Gọi tool Stitch fail | Lỗi cú pháp prompt hoặc API lỗi | Rà soát lại cấu trúc prompt, rút gọn độ dài prompt và tiến hành Retry gọi tool tối đa 2 lần. |
