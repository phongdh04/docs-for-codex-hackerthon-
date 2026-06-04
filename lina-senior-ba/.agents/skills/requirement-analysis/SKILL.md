---
name: requirement-analysis
description: Tổng hợp và cấu trúc bộ câu hỏi Q&A chuyên nghiệp (Batching).
---

## Description
Kỹ năng giúp Lina tổng hợp và cấu trúc toàn bộ câu hỏi (từ bước Clarification) thành một bộ câu hỏi duy nhất, chia theo nhóm logic, tránh hỏi rải rác.

## Triggers
- Kích hoạt ngay sau khi đã áp dụng `requirement-clarification` và phát hiện ra các điểm chưa rõ cần hỏi.
- Điều kiện tiên quyết: Đã có sẵn danh sách các lỗ hổng thông tin.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| 5W1H_analysis | List | Có | Kết quả phân tích từ bước trước |
| edge_cases | List | Có | Các ngoại lệ cần hỏi thêm |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| impact_matrix | Markdown | Bảng Ma trận đánh giá tác động 4 chiều (N nghiệp vụ, UI/UX, Dịch vụ & Dữ liệu, Quyền hạn) |
| batched_qa_list | Markdown | Danh sách câu hỏi tổng hợp, phân nhóm rõ ràng |

## Steps
1. **Quét toàn diện & Đánh giá tác động (Full Scan & Impact Matrix):**
   - Rà soát các lỗ hổng thông tin, mâu thuẫn từ `raw_request` và các ngoại lệ.
   - Thực hiện Đánh giá tác động (Impact Matrix) 4 chiều để xác định tính năng mới/nâng cấp này sẽ tác động đến những gì:
     - *Tác động Nghiệp vụ (Business Impact):* Ảnh hưởng đến các luồng nghiệp vụ hiện tại hoặc làm thay đổi các Ràng buộc nghiệp vụ toàn cục (Global Rules) không?
     - *Tác động Giao diện (UI/UX Impact):* Cần chỉnh sửa/thêm màn hình nào trên client (`web-client` Angular hoặc `pwa-client` React)?
     - *Tác động Dịch vụ & Dữ liệu (Service & Data Impact):* Repository nào bị ảnh hưởng (`product-service`, `background-service`, `ai-mcp-server`...)? Có làm thay đổi Database Schema hay API Endpoints không?
     - *Tác động Quyền hạn (Actor & Permission Impact):* Persona nào bị ảnh hưởng? Có thay đổi phân quyền (RBAC) không?
   - Kết quả phân tích được lập thành một bảng Markdown `impact_matrix`.
2. **Gom nhóm câu hỏi (Categorization):**
   - **Nhóm 1 - Nghiệp vụ (Business):** Các quy tắc, điều kiện logic, mục tiêu tính năng.
   - **Nhóm 2 - Trải nghiệm (UX/UI):** Luồng người dùng, thông báo, giao diện.
   - **Nhóm 3 - Dữ liệu (Data/API):** Trường thông tin, endpoints.
3. **Cấu trúc Output:**
   - Sử dụng tiêu đề cho từng nhóm, bullet points cho câu hỏi.
   - Chốt lại bằng một câu báo hiệu dừng chờ phản hồi.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Không có gì để hỏi | Yêu cầu đã quá rõ | Bỏ qua bước xuất danh sách và đi tiếp vào quá trình viết Brief |
