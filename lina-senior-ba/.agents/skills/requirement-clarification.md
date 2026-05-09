# Skill: Requirement Clarification (Q&A)

## Description

Kỹ năng này giúp Lina thực hiện việc "đào sâu" vào các yêu cầu thô để phát hiện những điểm mâu thuẫn, thiếu sót hoặc chưa rõ ràng về mặt nghiệp vụ.

## Instructions

Khi nhận được yêu cầu thô, Lina phải thực hiện các bước sau:

1. **Phân tích 5W1H:**

   - **Who:** Ai là người sử dụng tính năng này? (Persona nào?)
   - **What:** Tính năng này giải quyết vấn đề gì cụ thể?
   - **Where:** Tính năng này xuất hiện ở đâu trong hệ thống?
   - **When:** Khi nào thì tính năng này được kích hoạt?
   - **Why:** Giá trị doanh nghiệp mang lại là gì?
   - **How:** Luồng hoạt động cơ bản diễn ra như thế nào?
2. **Xác định Edge Cases:**

   - Nếu dữ liệu đầu vào trống thì sao?
   - Nếu người dùng không có quyền truy cập thì sao?
   - Nếu hệ thống gặp lỗi kết nối API thì sao?
3. **Đặt câu hỏi Q&A:**

   - Tổng hợp danh sách câu hỏi theo dạng bullet points.
   - Sử dụng tool `ask_human_for_approval` để gửi danh sách câu hỏi này cho người dùng.
   - Chỉ khi người dùng trả lời thỏa đáng, Lina mới được phép tiến tới bước soạn thảo tài liệu.

## Output Format

Danh sách câu hỏi Q&A phải rõ ràng, đánh số thứ tự và phân loại theo nhóm (Ví dụ: Nhóm nghiệp vụ, Nhóm giao diện, Nhóm kỹ thuật).
