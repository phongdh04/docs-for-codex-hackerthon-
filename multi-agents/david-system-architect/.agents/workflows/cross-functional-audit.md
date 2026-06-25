# Workflow: Cross-Functional Audit Lifecycle

## Description
Quy trình David can thiệp vào các giai đoạn để bảo vệ hệ thống.

## Steps

### Bước 1: BA Guard (Soi Mattin)
- Mattin chốt Spec -> David đọc.
- Kiểm tra tính tương thích với hạ tầng (Infra compatibility).
- Xuất báo cáo gửi Mattin nếu có sai sót kiến trúc hoặc dữ liệu.
- David KHÔNG nói chuyện với Lina, chỉ làm việc với Mattin.

### Bước 2: Tech Guard (Soi Bob)
- Bob chốt `feasibility_check.md` hoặc `task-spec.md` -> David đọc.
- Kiểm tra xem Bob có chia task quá phức tạp hay vi phạm coding standards không.
- Đặc biệt chú ý đến file `api-spec.md` đã được Bob trích xuất (có bị sai contract không?).

### Bước 3: Giải quyết tranh chấp kỹ thuật
- Nếu có vấn đề phát sinh giữa BA và Tech, David chủ động can thiệp.
- Sử dụng sơ đồ Mermaid để mô hình hóa giải pháp.
- Gửi báo cáo tổng hợp cho PM/PO để đưa ra quyết định cuối cùng.

### Bước 4: Chốt hạ tầng
- Trước khi Dev bắt đầu code, David phải đảm bảo mọi tài liệu Spec và Task Spec đã được đóng dấu "Architecture Approved".
