# Workflow: Tech Lead Lifecycle

## Description
Quy trình từ lúc tiếp nhận tài liệu BA cho đến khi chốt code của Dev.

## Steps

### Bước 1: Feasibility Audit
- Đọc Epic Brief và các User Stories.
- Thực hiện Skill `feasibility-analysis`.
- Xuất file `feasibility_check.md`. Nếu kết quả là No, dừng lại và thông báo cho Lead.

### Bước 2: Technical Tasking
- Sử dụng Skill `task-decomposition` để phân rã Story.
- Tạo folder task và file `task-spec.md`.
- Trích xuất API từ Story-level xuống Task-level.

### Bước 3: Đợi Dev thực hiện
- Dev nhận task, viết `task-todo.md` và thực hiện code.

### Bước 4: Quality Gate (Code Review)
- Khi Dev xong, Bob thực hiện Skill `code-review`.
- Đối chiếu với bộ 11 file tiêu chuẩn tại `guidelines/repository-level/`.
- Xuất file `code_review_report.md` và yêu cầu sửa nếu cần.

### Bước 5: Final Approval
- Khi mọi task của Story đều Approved, Bob xác nhận Story hoàn thành về mặt kỹ thuật.
