# Workflow: Review Cycle & Reporting

## Description
Quy trình thực hiện một lượt review chuyên nghiệp của BA Lead.

## Steps

### Bước 1: Thu thập dữ liệu
- Mattin đọc guideline chuẩn tại thư mục `guidelines/`.
- Mattin đọc bộ tài liệu mà Lina vừa bàn giao.

### Bước 2: Thẩm định chéo (Cross-check)
- So khớp dữ liệu giữa `user-story.md`, `api-spec.md` và `db_design.md`.
- Đảm bảo mọi field trong Spec đều đã được định nghĩa trong `data-dictionary.md`.

### Bước 3: Chấm điểm & Viết Report
- Sử dụng skill `strict-review.md` để trừ điểm.
- Tạo file `REVIEW_REPORT.md` với format:
    - **Header:** REVIEW REPORT - [Tên Component]
    - **Status:** PASS/FAIL (Kèm điểm số)
    - **Comment:** Những lời chỉ trích thẳng thắn.
    - **Defects:** Danh sách lỗi cụ thể.
    - **Interrogations:** Danh sách các câu hỏi chất vấn.

### Bước 4: Chốt kết quả
- Mattin gửi thông báo kết quả cuối cùng trong khung chat.
- Nếu FAIL, Mattin yêu cầu Lina phải phản hồi về thời gian hoàn thành bản sửa đổi.
- Mattin gửi report cho Lead (Người dùng) để đưa ra quyết định cuối cùng về việc có cho phép dự án đi tiếp hay không.
