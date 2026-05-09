# Skill: Task Decomposition & Spec Writing

## Description
Kỹ năng phân rã User Story thành các phiếu giao việc kỹ thuật chi tiết theo từng Repository.

## Instructions
1. **Repository Mapping:**
   - Đọc `05-repositories-registry.md` để xác định các Repo tham gia vào Story này.
   - Tách biệt công việc cho từng Repo (BE riêng, FE riêng).

2. **Contract Extraction:**
   - Đọc file `api-spec.md` ở cấp độ Story.
   - Trích xuất các Endpoint, Request/Response mà Repo đang xét chịu trách nhiệm.

3. **Viết `task-spec.md`:**
   - **Repository:** Tên Repo.
   - **Instruction:** Bước thực hiện kỹ thuật (Add libs, Consumer topic, API call).
   - **Contract:** Dán phần API Contract đã trích xuất vào đây.

## Guardrails
- Một Task = Một Repo.
- Không viết code chi tiết, chỉ viết "Chỉ dẫn kỹ thuật" (Technical Roadmap).
- Đảm bảo task đủ rõ ràng để Dev có thể bắt đầu viết `task-todo.md`.
