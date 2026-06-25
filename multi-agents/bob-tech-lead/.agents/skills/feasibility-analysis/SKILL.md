---
name: feasibility-analysis
description: Đọc tài liệu Story từ hệ thống, review chi tiết api-spec & db_design, đánh giá khả thi kỹ thuật và đề xuất kỹ năng/workflows.
---

## Description
Kỹ năng giúp Tech Lead Bob kiểm tra tính sẵn sàng của hạ tầng công nghệ, cấu hình và review đặc tả API cũng như thiết kế cơ sở dữ liệu đối với các yêu cầu nghiệp vụ mới thông qua việc đối chiếu động giữa yêu cầu của BA và năng lực thực tế dự án, sau đó đẩy báo cáo khả thi lên hệ thống.

## Triggers
- Khi nhận được Story mới từ BA hoặc có yêu cầu đánh giá khả thi từ hệ thống/User.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả |
|-----|--------------|----------|-------|
| projectKey | String | Có | Mã hiệu dự án (ví dụ: PAI) |
| storyKey | String | Có | Mã hiệu story cần đánh giá (PAI-STORY-12) |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả                                                                               |
|-----|--------------|-------------------------------------------------------------------------------------|
| feasibilityReport | File | Gửi báo cáo đặc tả `feasibility_check.md` lên hệ thống qua tool `report_feasibility_analysis`. |

## Feasibility Check Basis (Cơ sở đối chiếu đánh giá)
Để đưa ra kết quả khả thi chính xác và khách quan, Bob bắt buộc phải thu thập dữ liệu từ các nguồn cơ sở và thực hiện đối chiếu chéo động (không áp dụng các giả định hoặc fix cứng công nghệ):

### 1. Nguồn cơ sở Yêu cầu Nghiệp vụ (BA Story Specs):
*   **Trích xuất từ `api-spec.md`:** Các giao thức kết nối yêu cầu (REST, WebSocket, gRPC, Kafka topic), Endpoints, và cơ chế bảo mật truyền token (Headers, URL Query, Hash Fragment).
*   **Trích xuất từ `db_design.md`:** Hệ quản trị cơ sở dữ liệu yêu cầu (MySQL, PostgreSQL, NoSQL), cấu trúc bảng mới, kiểu dữ liệu trường chính/ngoại, và cơ chế mã hóa/bảo vệ dữ liệu (AES, RSA, hashing).
*   **Trích xuất từ `concept_note.md`:** Các tương tác đặc thù phía Client (chuyển tiếp giao diện, offline detection, background sync).

### 2. Nguồn cơ sở Năng lực Hệ thống (Project Architecture):
*   **Đọc từ `04-tech-stack.md` (qua `read_resource`):** Danh sách ngôn ngữ (Java, React, Node.js), Frameworks và Thư viện lõi hiện hành, Hệ CSDL chính thức của dự án, và các dịch vụ cổng Gateway/Security được cấu hình sẵn.
*   **Đọc từ `05-repositories-registry.md` (qua `read_resource`):** Vai trò nghiệp vụ của từng Repository và danh mục các thư viện cấu hình sẵn của mỗi Repo.

---

## Steps
1. **Tải context Story:** Gọi mcp tool `get_context_review_story` (hoặc thông qua kỹ năng `load-story-context`) để lấy toàn bộ Specs của Story.
2. **Tải context Dự án:** Sử dụng `read_resource` để đọc trực tiếp các tài liệu năng lực thực tế dự án tại:
   - Stack công nghệ: `project-document://[projectKey]/04-tech-stack.md`
   - Danh bạ repositories: `project-document://[projectKey]/05-repositories-registry.md`
3. **Đối chiếu chéo động (Dynamic Cross-Referencing):**
   *   **Đối chiếu CSDL:** So sánh các kiểu dữ liệu và hệ CSDL yêu cầu trong `db_design.md` với Hệ CSDL chính thức trong `04-tech-stack.md`. (Ví dụ: DB của PAI sử dụng MySQL v8.x, nếu BA thiết kế kiểu dữ liệu NoSQL hoặc UUID không tương thích MySQL $\rightarrow$ Đánh giá NOT FEASIBLE).
   *   **Đối chiếu Giao thức (Protocol):** So sánh giao thức yêu cầu trong `api-spec.md` với các cấu hình dịch vụ trong `04-tech-stack.md`. (Ví dụ: BA yêu cầu realtime Socket.io nhưng dự án không cấu hình service Node.js/Socket.io $\rightarrow$ Đánh giá NOT FEASIBLE).
   *   **Đối chiếu Repo & Thư viện:** Đối chiếu các chức năng đặc thù (như mã hóa thiết bị, Service Worker) với khả năng đáp ứng của Repo được gán vai trò đó trong `05-repositories-registry.md`.
4. **Đóng gói báo cáo:** Viết báo cáo theo chuẩn format guideline `feasibility_check.md` (đọc từ `guideline://EPIC/feasibility_check.md`) bao gồm:
   - Trạng thái khả thi (FEASIBLE / FEASIBLE WITH CONDITIONS / NOT FEASIBLE).
   - Chi tiết phân tích API Spec & DB Design đối chiếu cụ thể với hạ tầng.
   - Đề xuất các kỹ năng/workflows cần thiết của Dev thực thi.
5. **Đẩy lên hệ thống:** Gọi mcp tool `report_feasibility_analysis` để lưu trữ báo cáo khả thi trực tiếp trên Story.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Thiếu tài liệu Specs/Dự án | BA chưa upload Specs Story hoặc thiếu tài liệu stack/repo dự án | Dừng thực thi, ghi log lỗi và thông báo cho PM/User kiểm tra lại. |
| API/DB Spec lỗi hoặc bất hợp lý | BA viết sai chuẩn, sai cấu trúc DB hoặc mâu thuẫn hạ tầng | Đánh dấu NOT FEASIBLE, ghi rõ lý do đối chiếu bất tương thích và trả yêu cầu điều chỉnh Specs. |

