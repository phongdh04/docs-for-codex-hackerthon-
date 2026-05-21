# Detailed Workflow Matrix (22-Step I/O Audit)

Tài liệu này là kết quả phân tích chuyên sâu (Brainstorming) để chuẩn hóa quy trình 22 bước của Multi-Agent System. Mỗi bước quy định rõ ràng Người thực thi, Dữ liệu đầu vào, Dữ liệu đầu ra, và các file Guideline cần tham chiếu.

*(Ghi chú: Các nhãn `<Bổ sung>` đại diện cho những tài liệu/trạng thái hoặc hành động truyền dữ liệu trên hệ thống thực tế nhưng hiện chưa có template markdown tĩnh trong thư mục `guidelines`)*.

## Giai đoạn 1: Khởi tạo và Phê duyệt Đặc tả (Bước 0 đến Bước 7)

| Bước | Diễn giải quy trình | Ai làm? | Dữ liệu đầu vào (Input) | Tài liệu Tham khảo (References) | Kết quả đầu ra (Output) |
|:---|:---|:---|:---|:---|:---|
| **B0** | Q&A làm rõ yêu cầu | **Lina** | Yêu cầu thô từ khách hàng/PM | **[Project-Level]**: `01-overview.md` | Khởi tạo yêu cầu trên hệ thống `<Bổ sung>`, Lưu lại `qa_batch_log.md` `<Bổ sung>` |
| **B1** | Tìm hiểu dự án & Tạo/Update EPIC (ưu tiên update) kèm Brief | **Lina** | Khởi tạo yêu cầu & `qa_batch_log.md` (B0) | **[Project-Level]**: `02-roles-and-boundaries.md`, `06-workflow-and-process.md` | Epic & brief cho từng EPIC |
| **B2** | Duyệt tài liệu EPIC (Tối đa 3 lần) | **Mattin/David/PM** | Epic & brief cho từng EPIC (B1) | **[Project-Level]**: `01-overview.md` | **OK:** Sang B3, **Fail:** Quay lại B0 |
| **B3** | Phân rã & Làm rõ các Story | **Lina** | Epic & brief đã duyệt (B2) | **[Project-Level]**: `02-roles-and-boundaries.md`, `06-workflow-and-process.md` | **[Story-Level]**: `user-story.md`, `concept_note.md`, `api-spec.md`, `db_design.md`, `user-flow.md`, `data-dictionary.md`, `analytics-spec.md` |
| **B4** | Thiết kế UI/UX & Trích xuất Assets | **Robin** | `concept_note.md` (từ B3) | **[Project-Level]** *(Đề xuất dịch chuyển)*: `09_DESIGN_SYSTEM.md`, `stitch.md` | **[Story-Level]**: `ui_ux_spec.md`, `asset_handover_list.md`, Design Assets (Image/Icon) `<Bổ sung>` |
| **B5** | Đóng gói & Gửi review nghiệp vụ | **Lina** | Toàn bộ Specs của Lina (B3), `ui_ux_spec.md` & `asset_handover_list.md` (B4) | *(Bước gom nhóm, không cần đối chiếu)* | Epic Specs Package (Bản nén tổng hợp) `<Bổ sung>` |
| **B6** | Review nghiệp vụ (Tối đa 3 lần) | **Mattin**| Epic Specs Package `<Bổ sung>` | **[Project-Level]**: `01-overview.md`, `02-roles-and-boundaries.md`, `06-workflow-and-process.md`. **[Data]**: `qa_batch_log.md` `<Bổ sung>`, Khởi tạo yêu cầu `<Bổ sung>` | **OK:** Lệnh đi tiếp B7 `<Bổ sung>`, **Fail:** `review_report.md` (Gửi Lina quay lại B3) |
| **B7** | Review kiến trúc (Tối đa 3 lần) | **David** | Epic Specs Package `<Bổ sung>` (Đã qua Mattin B6) | **[Project-Level]**: `03-db-diagram.md`, `04-tech-stack.md`, `05-repositories-registry.md`, `07-api-integration.md`. **[Repo-Level]**: `01_PROJECT_ARCHITECTURE.md` | **OK:** Lệnh đi tiếp B8 `<Bổ sung>`, **Fail:** `architecture_audit.md` (Gửi Lina quay lại B3) |

## Giai đoạn 2: Phân rã Kỹ thuật & Chuẩn bị Kiểm thử (Bước 8 đến Bước 15)

| Bước | Diễn giải quy trình | Ai làm? | Dữ liệu đầu vào (Input) | Tài liệu Tham khảo (References) | Kết quả đầu ra (Output) |
|:---|:---|:---|:---|:---|:---|
| **B8** | Phân rã Task theo Repo | **Bob** | Epic Specs Package `<Bổ sung>` (Từ B7) | **[Project-Level]**: `05-repositories-registry.md`. **[Repo-Level]**: `01_PROJECT_ARCHITECTURE.md` | `task-spec.md` (Chia theo Repo), `feasibility_check.md` |
| **B9** | Gửi request review danh sách Task | **Bob** | Danh sách `task-spec.md` (B8) | *(Bước điều hướng)* | Task Review Request `<Bổ sung>` |
| **B10** | Audit kiến trúc danh sách Task (Max 3 lần) | **David** | Danh sách `task-spec.md` (B9) | **[Repo-Level]**: `01_PROJECT_ARCHITECTURE.md`, `03_DATABASE_AND_CACHE.md`, `04_SECURITY_AND_GATEWAY.md`, `06_KAFKA_MESSAGING.md` | **OK:** Lệnh đi tiếp B15 `<Bổ sung>`, **Fail:** Báo cáo lỗi `<Bổ sung>` (Trả về B8) |
| **B11** | Viết Kịch bản Test Hộp đen | **Sarah** | Các file Nghiệp vụ & UI (`user-story.md`, `ui_ux_spec.md`, `user-flow.md`, `api-spec.md`) | **[Repo-Level]**: `11_QA_TESTING_STANDARD.md` | `test-scenarios.md`, `test-automation-spec.md` (E2E Automation) |
| **B12** | Gửi request review Kịch bản Test | **Sarah** | Các file Specs từ B11 | *(Bước điều hướng)* | Test Review Request `<Bổ sung>` |
| **B13** | Audit Test Scenarios (Nghiệp vụ Test) | **Mattin** | `test-scenarios.md` (B12) | **[Project-Level]**: `06-workflow-and-process.md` | **OK:** Pass `<Bổ sung>`, **Fail:** Báo cáo lỗi `<Bổ sung>` (Trả về B11) |
| **B14** | Audit Test Automation Spec (Kiến trúc Test) | **David** | `test-automation-spec.md` (B12) | **[Repo-Level]**: `01_PROJECT_ARCHITECTURE.md`, `11_QA_TESTING_STANDARD.md` | **OK:** Pass `<Bổ sung>`, **Fail:** Báo cáo lỗi `<Bổ sung>` (Trả về B11) |
| **B15** | Giao việc (Assign) cho Dev | **Bob** | `task-spec.md` (Duyệt ở B10), `test-scenarios.md` (Duyệt ở B13) | *(Bước vận hành hệ thống)* | Task Assignment `<Bổ sung>` |

## Giai đoạn 3: Thực thi Code, Kiểm thử & Bàn giao (Bước 16 đến Bước 21)

| Bước | Diễn giải quy trình | Ai làm? | Dữ liệu đầu vào (Input) | Tài liệu Tham khảo (References) | Kết quả đầu ra (Output) |
|:---|:---|:---|:---|:---|:---|
| **B16** | Lập kế hoạch & Thực thi Code | **Dev** | `task-spec.md`, kèm `test-scenarios.md` để đọc trước | **[Repo-Level]**: `02_CODING_STANDARDS.md`, `12_ENGINEERING_EXECUTION_STANDARD.md` | `task-todo.md`, `logic_flow.md`, `design.md`, `acceptance_criteria.md`. Source Code & Pull Request `<Bổ sung>` |
| **B17** | Code Review & Merge | **Bob** | Source Code & Pull Request từ Dev (B16), `task-todo.md` | **[Project-Level]**: `09-devops-playbook.md`. **[Repo-Level]**: `10_CODE_REVIEW_STANDARD.md` | **OK:** Lệnh Merge đẩy lên Staging `<Bổ sung>`, **Fail:** `code_review_report.md` (Bắt Dev quay lại B16) |
| **B18** | Trigger Test khi đủ Task | **Hệ thống** | Trạng thái của tất cả các Task thuộc 1 Story (Merged) | *(Hệ thống quét dữ liệu)* | Test Request `<Bổ sung>` (Bắn thông báo cho Sarah) |
| **B19** | Kiểm thử Hộp đen (Story & E2E) | **Sarah** | Test Request (B18), Bản build trên Staging `<Bổ sung>`, `test-scenarios.md`, `test-automation-spec.md` | **[Project-Level]**: `08-qa-strategy.md`. **[Repo-Level]**: `11_QA_TESTING_STANDARD.md` | **OK:** `test_execution_report.md` & `story_test_report.md` (Sang B21), **Fail:** `bug_report.md` (Sang B20) |
| **B20** | Lọc lỗi & Điều phối Fix | **Bob** | `bug_report.md` (Từ B19) | *(Kinh nghiệm phân tích lỗi)* | Cập nhật trạng thái `bug_report.md`. Tạo Bug Fix Task `<Bổ sung>` & Assign cho Dev (Quay lại B16) |
| **B21** | Đóng Epic & Báo cáo | **Bob** | Toàn bộ `story_test_report.md` của Epic | *(Logic toàn vẹn dữ liệu)* | Epic Completion Report `<Bổ sung>` (Gửi PM/PO) |

