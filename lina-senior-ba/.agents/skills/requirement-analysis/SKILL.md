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
| batched_qa_list | Markdown | Danh sách câu hỏi tổng hợp, phân nhóm rõ ràng |

## Steps
1. **Quét toàn diện (Full Scan):**
   - Rà soát các lỗ hổng thông tin, mâu thuẫn từ `raw_request` và các ngoại lệ.
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
