---
name: markdown-specialist
description: Biên soạn tài liệu kỹ thuật chuẩn Markdown AI-native, tích hợp bộ khuôn mẫu kỹ thuật cứng (RESTful API, Observability JSON, relative link, Changelog).
---

## Description
Kỹ năng giúp Lux tổng hợp dữ liệu kỹ thuật từ mã nguồn và câu trả lời của User, biên dịch thành các tài liệu Markdown chuyên nghiệp cấp dự án (level PROJECT từ `01-overview.md` đến `10-observability.md`). Kỹ năng này áp dụng nghiêm ngặt bộ khuôn mẫu kỹ thuật cứng (RESTful API, Observability JSON, relative link) và cơ chế merge delta kèm Changelog lịch sử để đảm bảo chất lượng tài liệu cao nhất.

## Triggers
- Kích hoạt ở Bước 4 của quy trình `documentation-process.md`.
- Điều kiện tiên quyết: Đã thu thập đủ bối cảnh từ `context-collector` và phản hồi của User.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| doc_name | String | Có | Tên file tài liệu cần biên soạn (VD: `01-overview.md`, `10-observability.md`). |
| project_guidelines | JSON | Có | Hướng dẫn cấu trúc 5 tiểu mục chuẩn của tài liệu. |
| initial_context | JSON | Có | Bối cảnh thô thu thập từ mã nguồn và câu trả lời của User. |
| existing_doc_content | String | Không | Nội dung hiện tại của file tài liệu (nếu là tác vụ cập nhật). |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| document_content | String | Chuỗi nội dung Markdown hoàn chỉnh của tài liệu đã được biên soạn/cập nhật. |

## Steps
1. **Dàn dựng Khung tài liệu:**
   - Tạo cấu trúc Markdown dựa trên 5 tiểu mục chuẩn được quy định trong `project_guidelines` cho file tương ứng.
   - Nếu là tác vụ cập nhật (`existing_doc_content` không rỗng), tiến hành so sánh để xác định các phần thay đổi (delta) cần merge vào các tiểu mục cũ mà không làm mất thông tin tri thức lịch sử.
2. **Áp dụng Bộ khuôn mẫu kỹ thuật cứng (Hard Rules):**
   - **RESTful API Specs:** Nếu tài liệu có mô tả API, bắt buộc tuân thủ cấu trúc:
     - Method, Path, mô tả chức năng.
     - Bảng Headers (tên header, kiểu dữ liệu, bắt buộc, mô tả).
     - Bảng Request Body (tên trường, kiểu dữ liệu, bắt buộc, mô tả).
     - Bảng Responses (mã trạng thái như 200, 400, 401..., mô tả).
     - Ví dụ mẫu JSON payload (Success & Error).
   - **Observability Specs (dành cho file `10-observability.md`):** Bắt buộc phải mô tả chi tiết:
     - Quy chuẩn Log Level: Định nghĩa rõ khi nào dùng `DEBUG`, `INFO`, `WARN`, `ERROR`.
     - Cấu trúc Log JSON mẫu: Gồm các trường bắt buộc `timestamp`, `level`, `traceId`, `message`, `context`.
     - Bảng Metrics cốt lõi cần giám sát (tên chỉ số, đơn vị, loại metrics, ngưỡng cảnh báo).
   - **Quy tắc liên kết chéo (Interlinking):** Sử dụng relative link tiêu chuẩn dạng `[Tên liên kết](./[tên_file].md)` để trỏ sang các tài liệu PROJECT khác liên quan. Tuyệt đối không dùng absolute path.
3. **Tích hợp Changelog lịch sử (chỉ khi Cập nhật tài liệu):**
   - Nếu là cập nhật tài liệu đã có, bắt buộc phải thêm mục **Lịch sử Cập nhật (Changelog)** ở cuối file tài liệu dưới dạng bảng Markdown chuẩn:
     `| Phiên bản | Ngày | Người cập nhật | Nội dung thay đổi |`
     - Phiên bản tăng tiến logic (VD: 1.0 -> 1.1).
     - Người cập nhật ghi rõ "Lux - Project-Level Documentation Specialist".
4. **Bàn giao kết quả:** Trả về chuỗi nội dung Markdown hoàn chỉnh `document_content`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Sai lệch cấu trúc | Thiếu tiểu mục chuẩn theo guideline | Đối chiếu lại `project_guidelines`, bổ sung ngay các mục trống và điền ghi chú "Chờ cập nhật" nếu thiếu dữ liệu, không được bỏ trống cấu trúc. |
| Mâu thuẫn chéo | Nội dung biên soạn mâu thuẫn với file khác | Thực hiện điều chỉnh nội dung cho nhất quán hoặc ghi chú vào phần `out_scope_thinking.md` để hỏi User duyệt. |
