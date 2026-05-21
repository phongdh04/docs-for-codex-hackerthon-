# Workflows: Lina's Tasks (B0, B1 & B3)

## Description
Quy trình thực thi của Lina được phân rã làm 2 nhiệm vụ độc lập, tương ứng với các bước trong `Detailed_Workflow_Matrix.md`.

## Nhiệm vụ 1: Làm rõ yêu cầu thô và xử lý EPIC (Bước B0 & B1)

### Bước 1.1: Tiếp nhận & Q&A
- Lina nhận yêu cầu thô và hỏi rõ `projectKey`.
- Thực hiện phân tích khía cạnh (5W1H, Edge cases).
- Xuất bản **duy nhất một danh sách câu hỏi tổng hợp** và dừng lại chờ phản hồi (Batching Q&A).

### Bước 1.2: Khởi tạo Requirement & Rà soát
- Sau khi chốt được yêu cầu, khởi tạo phiếu yêu cầu (ProjectRequirement) qua tool `create_requirement`.
- Lưu vết nội dung làm rõ vào `qa_batch_log.md`.
- Sử dụng tool `search_semantic` để quét hệ thống tìm EPIC/Story cũ. Nếu có, tiến hành cập nhật thay vì tạo mới.

### Bước 1.3: Viết Epic Brief & Upload
- Viết file `brief.md` theo template cho EPIC (cũ hoặc mới).
- Sử dụng tool `upload_epic_doc` để đẩy tài liệu lên hệ thống.

### Bước 1.4: Gửi phê duyệt
- Sử dụng tool `request_review_epic` để gửi yêu cầu phê duyệt cho các EPIC đã tạo/cập nhật.
- **Dừng và kết thúc nhiệm vụ 1 (chờ duyệt).**

---

## Nhiệm vụ 2: Chi tiết hóa EPIC (Bước B3)

### Bước 2.1: Truy xuất tài liệu Epic
- Nhận lệnh chi tiết hóa một Epic.
- Dùng các tool (vd: `get_epic_doc_by_name`) để kéo tài liệu `brief.md` đã được duyệt về.

### Bước 2.2: Triển khai bộ Spec chi tiết
- Phân rã ra các User Story. Với mỗi User Story, Lina tiến hành viết:
    1. `user-story.md`
    2. `concept_note.md`
    3. `user-flow.md`
    4. `data-dictionary.md`
    5. `api-spec.md`
    6. `db_design.md`

### Bước 2.3: Phối hợp UI/UX (Robin)
- Khi viết xong `concept_note.md`, Lina dùng tool `request_screen_design` với tham số `storyKey` và `conceptNote` để gửi yêu cầu thiết kế cho Robin.

### Bước 2.4: Upload & Bàn giao
- Khi đã hoàn thành nội dung cho Story, Lina dùng tool `upload_story_doc` (hoặc các tool tương ứng) để đẩy Specs lên hệ thống. Đóng gói quy trình.
