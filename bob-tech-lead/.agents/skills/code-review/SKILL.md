---
name: code-review
description: Đánh giá Pull Request của DEV, kiểm duyệt theo 11 tiêu chuẩn repository-level và upload báo cáo review.
---

## Description
Kỹ năng đóng vai trò gác cổng chất lượng mã nguồn thực tế của Bob. Mọi thay đổi mã nguồn phải được đối chiếu nghiêm ngặt trước khi hợp nhất (merge).

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả |
|-----|--------------|----------|-------|
| projectKey | String | Có | Mã hiệu dự án |
| taskKey | String | Có | Mã hiệu task cần review |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả |
|-----|--------------|-------|
| reviewStatus | String | APPROVED hoặc REQUEST CHANGES. |
| reviewReport | File | Upload file `code_review_report.md` lên hệ thống qua `upload_task_doc`. |

## Steps
1. **Tải context:** Đọc file `task-todo.md` do DEV upload trên task để nắm kế hoạch ban đầu.
2. **Quét Guidelines:** Sử dụng `read_resource` để đọc các file tiêu chuẩn chất lượng tại `guideline://REPOSITORY/...`.
3. **Đối chiếu thực tế:** Đọc mã nguồn thay đổi trong Pull Request của DEV, đối chiếu từng dòng với 11 tiêu chuẩn kỹ thuật.
4. **Tạo báo cáo:** Viết báo cáo `code_review_report.md` chỉ ra chi tiết các lỗi vi phạm và dòng code cụ thể cần chỉnh sửa.
5. **Upload kết quả:** Gọi tool `upload_task_doc` để lưu trữ báo cáo review và chuyển trạng thái task tương ứng.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Thiếu tài liệu task-todo.md | Dev bỏ qua bước lập kế hoạch | Tự động đánh dấu REQUEST CHANGES kèm lý do thiếu `task-todo.md`. |
