---
name: bug-management
description: Kiểm duyệt báo cáo lỗi từ QC Sarah trên hệ thống, lọc lỗi, phân rã task sửa lỗi, và lưu trữ nợ kỹ thuật.
---

## Description
Bob duyệt các lỗi do Sarah log, loại bỏ các lỗi sai nghiệp vụ hoặc trùng lặp, phân rã task fix lỗi, và ghi nhận các lỗi/nợ kỹ thuật chưa sửa ngay vào file nợ kỹ thuật `technical_debt_note.md`.

## Triggers
- QC Sarah báo cáo đã hoàn thành test và log danh sách lỗi.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả |
|-----|--------------|----------|-------|
| projectKey | String | Có | Mã hiệu dự án |
| storyKey | String | Có | Mã hiệu Story có lỗi |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả |
|-----|--------------|-------|
| bugFixTasks | Action | Tạo các task fix lỗi thực tế trên hệ thống qua `create_task` và `upload_task_doc`. |
| bugDebts | File | Tạo hoặc cập nhật file nợ kỹ thuật `technical_debt_note.md` trên hệ thống nếu có bug hoãn lại. |

## Steps
1. **Tải danh sách lỗi:** Gọi tool **`get_bugs`** (bằng `projectKey` và `storyKey`) để kéo toàn bộ báo cáo lỗi của QC từ hệ thống.
2. **Kiểm duyệt (Triage):** Đọc kỹ từng dòng bug, đối chiếu lại Specs nghiệp vụ và API contract để lọc bỏ các lỗi sai hoặc không hợp lý.
3. **Phân rã sửa lỗi hoặc Hoãn lại (Debt Allocation):** 
   - Với các bug cần sửa ngay: Xác định Repo chịu trách nhiệm, gọi tool `create_task` tạo task và `upload_task_doc` đặc tả nhiệm vụ sửa lỗi.
   - Với các bug hoãn lại (Deferred bugs) hoặc nợ kỹ thuật phát sinh: Ghi nhận chi tiết lỗi vào file **`technical_debt_note.md`** để thực hiện sau.
4. **Upload File Nợ:** Gọi tool `upload_task_doc` đẩy file `technical_debt_note.md` cập nhật lên hệ thống dưới Story đó.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Mô tả bug không rõ ràng | QC thiếu thông tin các bước tái hiện | Đánh dấu trạng thái lỗi cần làm rõ và phản hồi Sarah (QC) trong chat. |
