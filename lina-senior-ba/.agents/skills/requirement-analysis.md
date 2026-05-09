# Skill: Batch Requirement Analysis

## Description
Kỹ năng giúp Lina tổng hợp và cấu trúc bộ câu hỏi Q&A một cách chuyên nghiệp, tránh việc hỏi nhiều lần gây phiền nhiễu cho người dùng.

## Instructions
1. **Quét toàn diện (Full Scan):**
   - Đọc yêu cầu thô và đối chiếu với guideline để tìm các lỗ hổng thông tin.
   - Suy nghĩ về các kịch bản lỗi, quyền hạn người dùng và luồng dữ liệu.

2. **Gom nhóm câu hỏi (Categorization):**
   - **Nhóm 1 - Nghiệp vụ (Business):** Các quy tắc, điều kiện logic, mục tiêu tính năng.
   - **Nhóm 2 - Trải nghiệm (UX/UI):** Luồng người dùng, các thông báo hiển thị, logic giao diện.
   - **Nhóm 3 - Dữ liệu (Data/API):** Các trường thông tin cần lưu, các endpoint cần trao đổi.

3. **Cấu trúc Output:**
   - Sử dụng tiêu đề cho từng nhóm.
   - Sử dụng bullet points để liệt kê câu hỏi.
   - Cuối danh sách, đưa ra một câu chốt: *"Đây là toàn bộ các câu hỏi cần làm rõ để tôi có thể bắt đầu viết Epic Brief. Vui lòng phản hồi để tôi tiếp tục."*

## Guardrails
- **KHÔNG** đặt câu hỏi đơn lẻ.
- **KHÔNG** vừa giải thích vừa đặt câu hỏi rải rác.
- Đảm bảo danh sách câu hỏi đã bao hàm đủ thông tin để viết file `brief.md`.
