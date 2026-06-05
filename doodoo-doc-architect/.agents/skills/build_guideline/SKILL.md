---
name: build_guideline
description: Xây dựng tài liệu guideline theo chuẩn 5 tiểu mục đệ quy (Recursive Meta-Guideline Standard).
---

## Description
Kỹ năng giúp DooDoo xây dựng, tạo mới hoặc bổ sung một tài liệu guideline trong hệ thống. Kỹ năng này áp dụng nghiêm ngặt tiêu chuẩn đệ quy: mỗi phần của guideline được tạo ra bắt buộc phải có đủ 5 tiểu mục (Mô tả, Cách viết, Nguồn thông tin, Cách thu thập, Format gợi ý) để AI và Con người dễ dàng tuân thủ.

## Triggers
- Kích hoạt tại Bước 3 của quy trình `build-guideline-flow.md`.
- Điều kiện tiên quyết: Đã làm rõ yêu cầu nghiệp vụ qua Q&A và lấy context guideline cũ (nếu có).

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| name | String | Có | Tên tài liệu guideline cần xây dựng (VD: `01-overview.md`). |
| existing_guideline_content | Markdown | Không | Nội dung guideline cũ đã có trên hệ thống (dùng khi cập nhật). |
| requirements | String | Có | Các thông tin nghiệp vụ/yêu cầu đã chốt qua bước Q&A. |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| guideline_content | Markdown | Nội dung guideline hoàn chỉnh đã được định dạng và cấu trúc theo đúng chuẩn 5 tiểu mục. |

## Steps
1. **Thiết lập Khung cấu trúc:**
   - Liệt kê các đầu mục lớn cần thiết dựa theo yêu cầu nghiệp vụ.
   - Nếu là cập nhật, xác định phần delta thay đổi để chèn vào mà không làm hỏng cấu trúc cũ.
2. **Biên soạn 5 tiểu mục bắt buộc cho từng đầu mục:**
   - **Mô tả:** Giải thích rõ mục này là gì, tại sao quan trọng.
   - **Cách viết:** Hướng dẫn chi tiết từng bước cách thực hiện.
   - **Nguồn thông tin:** Chỉ rõ dữ liệu đầu vào lấy từ đâu (file, api, DB...).
   - **Cách thu thập:** Phương pháp, câu lệnh cụ thể để lấy thông tin.
   - **Format gợi ý / Template áp dụng:** Khung mẫu định dạng Markdown Code Block cụ thể.
3. **Áp dụng định dạng chuẩn:**
   - Đánh số thứ tự cho các đầu mục chính.
   - Sử dụng định dạng Bold/Heading phân cấp rõ ràng.
   - Sử dụng sơ đồ `mermaid` cho các quy trình phức tạp và bảng Markdown cho so sánh.
4. **Bàn giao kết quả:** Trả về chuỗi Markdown `guideline_content` hoàn chỉnh.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Thiếu 1 trong 5 tiểu mục bắt buộc | Sơ suất khi biên soạn | Rà soát lại tài liệu trước khi trả về, bổ sung ngay các mục trống và điền ghi chú "Chờ cập nhật" nếu thiếu thông tin, không được bỏ trống cấu trúc. |
| Yêu cầu nghiệp vụ mâu thuẫn | Đầu vào requirements chưa đồng nhất | Quay lại bước Q&A để hỏi lại User làm rõ điểm mâu thuẫn nghiệp vụ. |
