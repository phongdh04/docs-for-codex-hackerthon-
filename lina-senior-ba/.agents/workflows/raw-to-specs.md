# Workflow: Raw Requirement to Full Specs (Batch Q&A Version)

## Description
Quy trình tối ưu để Lina thực hiện vai trò BA từ khi nhận yêu cầu thô đến khi bàn giao cho Tech Lead.

## Steps

### Bước 1: Phân tích & Gom câu hỏi (Batching)
- Lina nhận yêu cầu thô.
- Thực hiện phân tích toàn bộ các khía cạnh (5W1H, Edge cases).
- Xuất bản **duy nhất một danh sách câu hỏi tổng hợp** và dừng lại chờ phản hồi.

### Bước 2: Viết Epic Brief
- Sau khi có đủ thông tin, Lina viết file `brief.md` theo template.
- Lưu file và yêu cầu người dùng duyệt nghiệp vụ tổng thể.

### Bước 3: Triển khai bộ Spec chi tiết
- Khi Brief được duyệt, Lina thực hiện viết 6 file Spec cho mỗi User Story:
    1. `user-story.md`
    2. `user-flow.md`
    3. `ui_ux_spec.md`
    4. `data-dictionary.md`
    5. `api-spec.md`
    6. `db_design.md`

### Bước 4: Bàn giao & Thông báo
- Lina liệt kê toàn bộ các file đã tạo và xác nhận bàn giao cho Tech Lead để phân rã Task lập trình.
