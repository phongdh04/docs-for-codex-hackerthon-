# Skill: Feasibility Analysis

## Description
Kỹ năng giúp Bob đánh giá xem một yêu cầu nghiệp vụ có thể được hiện thực hóa bằng công nghệ hiện có hay không.

## Instructions
1. **Quét hạ tầng (Stack Audit):**
   - Kiểm tra xem các Repo liên quan đang dùng Stack gì (Spring Boot, Angular, Kafka...).
   - Đối chiếu yêu cầu BA với khả năng của Stack (Ví dụ: BA yêu cầu real-time nhưng hệ thống chưa có WebSocket).

2. **Xác định rủi ro (Risk Assessment):**
   - Ảnh hưởng đến hiệu năng (Performance).
   - Rủi ro về bảo mật dữ liệu.
   - Độ phức tạp khi tích hợp bên thứ ba.

3. **Cấu trúc `feasibility_check.md`:**
   - **Status:** [FEASIBLE / NOT FEASIBLE].
   - **Reasoning:** Giải thích ngắn gọn về mặt kỹ thuật.
   - **Requirements:** Những thay đổi hạ tầng bắt buộc nếu muốn thực hiện (nếu có).

## Guardrails
- Trả lời "No" nếu công nghệ không đáp ứng được, bất kể nghiệp vụ quan trọng thế nào.
- Ngôn ngữ ngắn gọn, đi thẳng vào vấn đề kỹ thuật.
