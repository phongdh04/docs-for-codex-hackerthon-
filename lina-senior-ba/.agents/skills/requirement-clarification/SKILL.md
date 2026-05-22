---
name: requirement-clarification
description: Đào sâu yêu cầu thô để phát hiện điểm mâu thuẫn, thiếu sót qua 5W1H.
---

## Description
Kỹ năng này giúp Lina thực hiện việc "đào sâu" vào các yêu cầu thô để phát hiện những điểm mâu thuẫn, thiếu sót hoặc chưa rõ ràng về mặt nghiệp vụ thông qua phương pháp 5W1H và Edge Cases.

## Triggers
- Kích hoạt ngay khi nhận được yêu cầu thô ban đầu từ người dùng.
- Điều kiện tiên quyết: Yêu cầu chưa rõ ràng hoặc cần xác minh thêm chi tiết.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| raw_request | Text | Có | Yêu cầu thô từ khách hàng/PM. |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| 5W1H_analysis | List | Khung phân tích 5W1H. |
| edge_cases | List | Các kịch bản ngoại lệ tiềm ẩn. |

## Steps
1. **Phân tích 5W1H:**
   - **Who:** Ai là người sử dụng tính năng này?
   - **What:** Tính năng này giải quyết vấn đề gì cụ thể?
   - **Where:** Tính năng này xuất hiện ở đâu trong hệ thống?
   - **When:** Khi nào thì tính năng này được kích hoạt?
   - **Why:** Giá trị doanh nghiệp mang lại là gì?
   - **How:** Luồng hoạt động cơ bản diễn ra như thế nào?
2. **Xác định Edge Cases:**
   - Nếu dữ liệu đầu vào trống thì sao?
   - Nếu người dùng không có quyền truy cập thì sao?
   - Nếu hệ thống gặp lỗi kết nối API thì sao?

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Yêu cầu quá mờ nhạt | User nhập không đủ chữ/ý | Yêu cầu user cung cấp thêm tối thiểu mục tiêu cốt lõi |
