# Documentation Responsibility Matrix

Dựa trên việc rà soát kỹ `agent_workflow.md`, bảng phân vai trong `README.md` và `AGENTS.md` của từng Agent, dưới đây là ma trận trách nhiệm chuẩn xác nhất đối với quy trình vận hành tài liệu dự án.

## Bảng Tổng Hợp Trách Nhiệm Tài Liệu

| Cấp độ (Level) | Tên file Guideline | Agent viết tài liệu (Writer) | Các Agent đọc tài liệu (Readers) | Agent audit tài liệu (Auditor) |
| :--- | :--- | :--- | :--- | :--- |
| **Project-Level** | `01-overview.md` đến `10-observability.md` | Project-Doc Specialist, Bob | Tất cả các Agents | Mattin, David, Bob |
| **Epic-Level** | `brief.md` | Lina (Senior BA) | Robin, Sarah, PM/PO, Devs | Mattin (Duyệt), Bob (Audit) |
| | `architecture_audit.md` | David (System Architect) | PM/PO, Mattin, Bob | PM/PO (Người dùng) |
| | `feasibility_check.md` | Bob (Tech Lead) | David, PM/PO | David (System Architect) |
| | `review_report.md` | Mattin (BA Lead) | Lina, Sarah, PM/PO | PM/PO (Người dùng) |
| **Story-Level** | `user-story.md` | Lina (Senior BA) | Robin, Sarah, Bob, Devs | Mattin (Nghiệp vụ), David (Kiến trúc) |
| | `ui_ux_spec.md` | Robin (UI/UX Designer) | Lina, Bob, Sarah, Devs (FE/Mobile) | Lina (Gom bộ), Mattin, David |
| | `api-spec.md`, `db_design.md` | Lina (Senior BA) | Kevin, Devs FE, Sarah, Bob | Mattin (Nghiệp vụ), David (Kiến trúc) |
| | `user-flow.md`, `data-dictionary.md`, `analytics-spec.md` | Lina (Senior BA) | Robin, Bob, Sarah, Devs | Mattin (BA Lead) |
| | `concept_note.md` | Lina (Senior BA) | Robin (UI/UX Designer) | Mattin (BA Lead) |
| | `asset_handover_list.md`| Robin (UI/UX Designer) | Lina, Devs (FE/Mobile) | Mattin (BA Lead) |
| | `test-scenarios.md` | Sarah (QC Lead) | Bob, Devs | Mattin (BA Lead) |
| | `story_test_report.md` | Sarah (QC Lead) | Bob, PM/PO | PM/PO (Phê duyệt cuối) |
| | `bug_report.md` (Cấp độ Story) | Sarah (QC Lead) | Bob (Tech Lead), Devs | Bob (Lọc lỗi & Yêu cầu fix) |
| **Task-Level (BE/FE/Mobile)** | `task-spec.md` | Bob (Tech Lead) | Kevin, Luffy, Sanji, Zoro, Sarah | David (System Architect) |
| | `task-todo.md`, `acceptance_criteria.md` | Các Devs (Kevin, Luffy, Sanji, Zoro) | Bob (Tech Lead) | Bob (Tech Lead) |
| | `logic_flow.md`, `design.md` | Các Devs | Bob (Tech Lead) | Bob (Tech Lead) |
| | `code_review_report.md` | Bob (Tech Lead) | Các Devs, PM/PO | PM/PO (Người dùng) |
| | `bug_report.md` (Cấp độ Task) | Sarah (QC Lead) | Bob, Devs | Bob (Lọc lỗi) |
| | `technical_debt_note.md` | Bob (Tech Lead), Devs | David (System Architect) | David (System Architect) |
| **Task-Level (Test)** | `test-automation-spec.md` | Sarah (QC Lead) | David, Bob, Devs | David (System Architect) |
| | `test_execution_report.md` | Sarah (QC Lead) | PM/PO, Bob | PM/PO (Người dùng) |

## Chuỗi Audit & Workflow (Căn cứ theo `agent_workflow.md`)

- **Bước 1:** Lina làm SPECs (brief / user story list), trong đó bao gồm cả việc viết `concept_note.md` cho từng story.
- **Bước 2:** Lina gửi `concept_note.md` của từng story cho Robin để yêu cầu thiết kế. Robin thiết kế xong sẽ gửi lại `ui_ux_spec.md` và tài nguyên cho Lina.
- **Bước 3:** Lina tổng hợp và gửi toàn bộ bộ Specs của Epic đi review. Gửi trước cho Mattin để review về mặt nghiệp vụ.
- **Bước 4:** Mattin review:
  - Nếu **OK** => Tiếp tục sang Bước 5.
  - Nếu **Không OK** => Quay lại Bước 1. (Lưu ý: Nếu số lần không OK quá 3 lần, tạm dừng và gọi PM/PO giải quyết).
- **Bước 5:** Gửi request review đến David (Audit Kiến trúc):
  - Nếu **OK** => Tiếp tục sang Bước 6.
  - Nếu **Không OK** => Quay lại Bước 1.
- **Bước 6:** Bob tiếp nhận bộ Specs đã được duyệt => Tiến hành phân rã Task theo từng Repository (`task-spec.md`).
- **Bước 7:** Bob gửi request review danh sách Task đến David.
- **Bước 8:** David audit danh sách Task của Bob:
  - Nếu **OK** => Chuyển sang Bước 9.
  - Nếu **Không OK** => Trả về Bước 6 để Bob chia lại task. (Nếu không OK quá 3 lần, tạm dừng và gọi PM/PO).
- **Bước 9:** Bob tiến hành giao việc (Assign) cho các Devs (Kevin, Luffy, Sanji, Zoro).
- **Bước 10:** Devs thực thi code => Push code và tạo Merge Request.
- **Bước 11:** Bob review code của Devs:
  - Nếu **OK** => Merge code và đẩy lên Staging.
  - Nếu **Không OK** => Yêu cầu Dev sửa lại theo comment.
- **Bước 12:** Hệ thống tự động kiểm tra xem một Story đã hoàn thành hết các Tasks bên trong chưa => Nếu đủ, Request Sarah vào test.
- **Bước 13:** Sarah tiến hành Test:
  - Nếu **OK** => Request Release Production (Chuyển sang Bước 15).
  - Nếu **Không OK** => Log bug theo Task/Story và Request Bob fix.
- **Bước 14:** Bob kiểm tra và đọc log bug của Sarah => Lọc lỗi => Tạo task fix => Assign cho Devs (Quay lại Bước 10).
- **Bước 15:** Kiểm tra các Story trong Epic đã hoàn thành hết chưa. Nếu đã hoàn thành trọn vẹn 1 Epic => Lập báo cáo gửi PM/PO.
