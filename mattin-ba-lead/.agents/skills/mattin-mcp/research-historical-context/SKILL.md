---
name: research-historical-context
description: Đào bới lịch sử hệ thống để lấy bối cảnh tính năng cũ (EPIC/STORY).
---

## Description
Kỹ năng tổ hợp (Composite Skill) giúp Mattin tự động dò tìm các tính năng (Epic/Story) đã làm trong quá khứ liên quan đến yêu cầu review hiện tại, nhằm kiểm tra xem BA có kế thừa chuẩn xác hoặc có làm trùng lặp/mâu thuẫn logic hay không.

## Triggers
- Kích hoạt ở Bước 2 của quy trình `review-cycle.md` trong giai đoạn thu thập dữ liệu đối chiếu.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu của dự án đang thao tác |
| text | String | Có | Cụm từ khóa hoặc ngữ cảnh tính năng cần tra cứu đối chiếu |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| historical_docs | String/Markdown | Nội dung chi tiết các tài liệu Epic/Story cũ để làm căn cứ thẩm định. Rỗng nếu không có. |

## Steps
1. **Tìm kiếm ngữ nghĩa (Semantic Search):**
   - Gọi tool `search_semantic` từ `mattin-mcp` (truyền `projectKey`, `text`).
2. **Kiểm tra kết quả:**
   - Nếu KHÔNG CÓ kết quả: Trả về `historical_docs` rỗng.
   - Nếu có kết quả, chuyển sang Bước 3.
3. **Lấy danh sách tài liệu:**
   - Với mỗi `epicKey` tìm được: Gọi `epic_docs_list` để lấy danh sách tên tài liệu.
   - Với mỗi `storyKey` tìm được: Gọi `story_docs_list` để lấy danh sách tên tài liệu.
4. **Đọc chi tiết tài liệu:**
   - Sử dụng `get_epic_doc_by_name` (truyền `projectKey`, `epicKey`, `name`) để đọc các file của Epic.
   - Sử dụng `get_story_doc_by_name` (truyền `projectKey`, `storyKey`, `name`) để đọc các file của Story.
5. **Tổng hợp:** Gom toàn bộ nội dung đọc được trả về biến `historical_docs`.

## Error Handling
| Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Không có kết quả search | Tính năng mới hoàn toàn | Coi như không có dữ liệu lịch sử đối chiếu, tiếp tục review các tiêu chí khác. |
