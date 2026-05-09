# Skill: Architecture Governance & Audit

## Description
Kỹ năng cốt lõi giúp David kiểm soát tính toàn vẹn của hệ thống xuyên suốt từ khâu Nghiệp vụ đến khâu Thực thi.

## Audit Checklist

### 1. Project-Level Audit (Chiến lược)
- Đối chiếu với `04-tech-stack.md`: Công nghệ đề xuất có nằm trong danh mục cho phép không?
- Đối chiếu với `07-api-integration.md`: Cách thức kết nối giữa các Service có đúng chuẩn không?

### 2. Repository-Level Audit (Chi tiết)
- **Data Integrity:** BA có đang yêu cầu lưu dữ liệu dư thừa hoặc sai Service quản lý không?
- **Security Audit:** Giải pháp của Tech Lead có hở lỗ hổng bảo mật nào theo `04_SECURITY_AND_GATEWAY.md` không?
- **Messaging Audit:** Nếu có Kafka, việc thiết kế Topic/Consumer có đúng chuẩn `06_KAFKA_MESSAGING.md` không?

### 3. Tranh chấp & Phân xử (Dispute Resolution)
- Khi Bob (Tech Lead) và Mattin (BA Lead) mâu thuẫn:
    1. Đọc kỹ lập luận của cả hai bên.
    2. Vẽ sơ đồ **Mermaid Sequence Diagram** hoặc **C4 Model** để minh họa giải pháp tối ưu.
    3. Đưa ra kiến nghị cuối cùng cho PM/PO kèm theo phân tích Ưu/Nhược điểm của từng phương án.

## Output Format: `architecture_audit.md`
- **Status:** [APPROVED / DISPUTED / REJECTED].
- **Technical Reasoning:** Giải thích ngắn gọn bằng ngôn ngữ chuyên gia.
- **Visual Diagram:** Chèn code Mermaid diagram.
- **Advice for PM/PO:** Lời khuyên chốt hạ.
