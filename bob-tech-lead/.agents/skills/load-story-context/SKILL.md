---
name: load-story-context
description: Sử dụng mcp tool get_context_review_story để lấy đầy đủ context của Story (Specs nghiệp vụ, UI Spec, API, DB) trước khi review.
---

## Description
Kỹ năng giúp Tech Lead Bob truy xuất toàn bộ thông tin, đặc tả nghiệp vụ, tài liệu thiết kế giao diện, sơ đồ luồng dữ liệu, từ điển dữ liệu, đặc tả API và thiết kế cơ sở dữ liệu của một Story trong một cuộc gọi MCP duy nhất. Điều này giúp Bob có cái nhìn 360 độ về Story trước khi thực hiện đánh giá khả thi kỹ thuật hoặc phân rã task.

## Triggers
- Khi Bob chuẩn bị thực hiện đánh giá tính khả thi kỹ thuật (Feasibility Check) cho một Story.
- Khi Story được duyệt sang trạng thái `IN_PROGRESS` và Bob cần lấy context đầy đủ để phân rã task.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả |
|-----|--------------|----------|-------|
| storyKey | String | Có | Mã hiệu Story cần lấy context (ví dụ: `PAI-STORY-12`) |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả |
|-----|--------------|-------|
| projectKey | String | Mã hiệu dự án trích xuất từ Story (ví dụ: `PAI`) |
| epicKey | String | Mã hiệu Epic cha trích xuất từ Story (ví dụ: `PAI-EPIC-46`) |
| storyTitle | String | Tiêu đề của Story |
| priority | String | Mức độ ưu tiên MoSCoW của Story (Must, Should, Could, Won't) |
| userStory | Markdown | Nội dung đặc tả User Story nghiệp vụ (`user-story.md`) |
| conceptNote | Markdown | Nội dung đặc tả ý tưởng thiết kế giao diện (`concept_note.md`) |
| userFlow | Markdown | Sơ đồ luồng đi của người dùng và thiết kế màn hình (`user-flow.md`) |
| dataDictionary | Markdown | Định nghĩa thuật ngữ và ràng buộc trường dữ liệu (`data-dictionary.md`) |
| apiSpec | Markdown | Đặc tả API và các cổng kết nối dữ liệu (`api-spec.md`) |
| dbDesign | Markdown | Đặc tả CSDL máy chủ và cơ chế mã hóa cục bộ (`db_design.md`) |
| epicBrief | Markdown | Nội dung tài liệu đặc tả Epic cha (`brief.md` trích xuất từ `data.epic.documents`) |
| techStack | Markdown | Tài liệu stack công nghệ của dự án, được đọc trực tiếp từ MCP resource `project-document://[projectKey]/04-tech-stack.md` bằng `read_resource`. |
| repoRegistry | Markdown | Tài liệu danh mục các repositories của dự án, được đọc trực tiếp từ MCP resource `project-document://[projectKey]/05-repositories-registry.md` bằng `read_resource`. |

## Steps
1. **Gọi MCP Tool:** Gọi mcp tool **`get_context_review_story`** và chỉ truyền tham số **`storyKey`** (không truyền `projectKey` để tránh lỗi cấu trúc validation).
2. **Trích xuất Metadata:** Đọc và phân tích payload JSON trả về để lấy các trường:
   - `data.projectKey` $\rightarrow$ Ghi nhận dự án.
   - `data.epicKey` $\rightarrow$ Ghi nhận Epic cha để đối chiếu `brief.md`.
   - `data.title` $\rightarrow$ Ghi nhận tiêu đề.
   - `data.priority` $\rightarrow$ Ghi nhận mức độ ưu tiên.
3. **Phân tích danh sách tài liệu (`data.documents`):** Duyệt qua mảng tài liệu để trích xuất nội dung `content` tương ứng với từng tên tài liệu `name`:
   - Lọc ra `user-story.md` $\rightarrow$ Đọc kịch bản Acceptance Criteria.
   - Lọc ra `concept_note.md` $\rightarrow$ Đọc ý tưởng giao diện di động của Lina.
   - Lọc ra `user-flow.md` $\rightarrow$ Đọc sơ đồ Mermaid luồng người dùng và link thiết kế Stitch.
   - Lọc ra `data-dictionary.md` $\rightarrow$ Đọc ràng buộc các trường dữ liệu.
   - Lọc ra `api-spec.md` $\rightarrow$ Đọc danh sách endpoints và quy chuẩn an toàn truyền token (như Hash Fragment).
   - Lọc ra `db_design.md` $\rightarrow$ Đọc thiết kế bảng SQL và cơ chế bảo mật (như mã hóa đối xứng AES bằng Device-bound key).
4. **Tải tài liệu Epic & Dự án:**
   - Duyệt qua `data.epic.documents` lọc ra `brief.md` $\rightarrow$ Đọc tóm tắt và KPI của Epic.
   - **Đọc tài liệu dự án tĩnh từ MCP Resources:** Sử dụng tool `read_resource` để đọc trực tiếp đặc tả stack công nghệ dự án tại URI `project-document://[projectKey]/04-tech-stack.md` và danh mục repositories tại URI `project-document://[projectKey]/05-repositories-registry.md`.
5. **Đồng bộ bộ nhớ hoạt động:** Lưu trữ toàn bộ thông tin này vào bộ nhớ context làm việc của Bob, sẵn sàng làm đầu vào cho kỹ năng `feasibility-analysis` hoặc `task-decomposition`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Lỗi Validation Schema | Truyền dư tham số `projectKey` khi gọi tool | Chỉ truyền duy nhất tham số `storyKey` khi gọi `get_context_review_story`. |
| Story không tồn tại | Mã hiệu `storyKey` sai hoặc Story chưa được tạo trên DB | Dừng thực thi, ghi log lỗi và thông báo cho PM/User kiểm tra lại mã Story. |
| Thiếu tài liệu để trích xuất | BA/Designer chưa upload hoặc thiếu `brief.md`/`04-tech-stack.md`/`05-repositories-registry.md` hoặc tài liệu Story | Dừng thực thi, ghi log lỗi và thông báo cho PM/User kiểm tra lại. |

