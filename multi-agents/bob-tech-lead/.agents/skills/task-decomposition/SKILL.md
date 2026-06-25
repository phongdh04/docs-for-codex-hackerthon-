---
name: task-decomposition
description: Phân rã Story thành các Task kỹ thuật chi tiết theo từng Repository và đăng ký lên hệ thống.
---

## Description
Bob chia nhỏ Story thành các đơn vị công việc chuyên biệt cho lập trình viên (DEV) dựa trên cấu trúc các Repository trong hệ thống, đảm bảo nguyên tắc cô lập trách nhiệm.

## Triggers
- Khi hệ thống/User gửi tín hiệu trigger trạng thái Story là `IN_PROGRESS` (được duyệt bởi PM).

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả |
|-----|--------------|----------|-------|
| projectKey | String | Có | Mã hiệu dự án |
| storyKey | String | Có | Mã hiệu story được duyệt phân rã |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả |
|-----|--------------|-------|
| systemTasks | Action | Gọi `create_task` tạo danh sách task trên hệ thống DB. |
| taskSpecs | Action | Gọi `upload_task_doc` để đẩy chi tiết `task_spec.md` cho từng Task. |

## Steps
1. **Đọc danh mục Repos:** Sử dụng `read_resource` để đọc danh bạ mã nguồn `project-document://[projectKey]/05-repositories-registry.md`.
2. **Trích xuất Contract:** Sử dụng tài liệu API Spec (`apiSpec`) đã được tải từ context của Story trước đó để lấy API contract cần thiết.
3. **Thiết kế Task:** Chia nhỏ Story thành các đầu việc độc lập. Ràng buộc: *1 Task = 1 Repo duy nhất*.
4. **Tạo Task trên DB:** Với mỗi task được thiết kế, gọi tool **`create_task`** để đăng ký lấy `taskKey` từ hệ thống.
5. **Upload Đặc tả:** Soạn nội dung đặc tả nhiệm vụ kỹ thuật (`task_spec.md`) bao gồm:
   - Repository gán.
   - Hướng dẫn triển khai kỹ thuật.
   - Thư viện cần cài đặt và Contract trích xuất.
   - **Các Kỹ năng Dev bắt buộc** (Relative path trỏ trực tiếp đến file `SKILL.md` trong Agent Dev tương ứng như `sanji-senior-react-dev/.agents/skills/...`).
   - Gọi tool **`upload_task_doc`** để đẩy lên hệ thống.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Story không ở trạng thái IN_PROGRESS | PM chưa duyệt Story | Dừng thực thi, báo cáo lỗi trạng thái và yêu cầu PM duyệt trước. |
| Một task chạm nhiều Repo | Thiết kế task bị gộp chung | Hủy bỏ thiết kế cũ, bắt buộc thực hiện chia tách nhỏ hơn để đạt tính đơn nhiệm. |
