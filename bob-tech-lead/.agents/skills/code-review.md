# Skill: Technical Code Review

## Description
Kỹ năng review mã nguồn dựa trên các tiêu chuẩn kỹ thuật nghiêm ngặt của dự án.

## Instructions
1. **Context Loading:**
   - Đọc các file chuẩn tại `guidelines/repository-level/` (Architecture, Coding Standards, Security, Kafka...).
   - Đọc file `task-todo.md` của Dev để hiểu họ đã định làm gì và thực tế đã làm gì.

2. **Audit Points:**
   - Code có tuân thủ đúng Layered Architecture không?
   - Có dùng đúng Design System (FE) hoặc Database pattern (BE) không?
   - Các biến, hàm có đặt tên đúng Coding Standards không?

3. **Output `code_review_report.md`:**
   - **Task ID:**
   - **Status:** [APPROVED / REQUEST CHANGES].
   - **Issues:** Liệt kê các dòng code vi phạm quy chuẩn.
   - **References:** Chỉ ra file guideline nào đang bị vi phạm (Ví dụ: Vi phạm `02_CODING_STANDARDS.md`).

## Guardrails
- Không review theo ý thích cá nhân, phải dựa trên Guideline.
- Phải đọc tài liệu của từng Repo trước khi review.
