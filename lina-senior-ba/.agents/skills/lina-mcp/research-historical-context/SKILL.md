---
name: research-historical-context
description: Đào bới lịch sử hệ thống để lấy bối cảnh tính năng cũ (EPIC/STORY).
---

## Description
Kỹ năng tổ hợp (Composite Skill) giúp Lina tự động dò tìm các tính năng (Epic/Story) đã từng làm trong quá khứ có ngữ nghĩa tương đồng với yêu cầu mới, sau đó tự động tải về danh sách tài liệu và đọc chi tiết.

## Triggers
- Kích hoạt ở Bước 1 của quy trình tạo EPIC (`epic-creation.md`), ngay sau khi đọc tài liệu dự án, để thu thập thêm các thông tin nghiệp vụ kế thừa.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu của dự án đang thao tác |
| text | String | Có | Cụm từ khóa hoặc ngữ cảnh tính năng cần tra cứu |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| historical_docs | String/Markdown | Nội dung chi tiết các tài liệu Epic/Story cũ đã lấy được. Trả về rỗng nếu không tìm thấy. |

## Steps
1. **Tìm kiếm ngữ nghĩa (Semantic Search):**
   - Sử dụng công cụ `search_semantic` (truyền `projectKey`, `text`).
2. **Kiểm tra kết quả:**
   - Nếu KHÔNG CÓ kết quả trả về: Hệ thống chưa có tính năng này. Bỏ qua các bước sau, trả về `historical_docs` rỗng.
   - Nếu có kết quả (trả về danh sách `epicKey` hoặc `storyKey`), chuyển sang Bước 3.
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
| Không có kết quả search | Tính năng mới hoàn toàn | Coi như bình thường. Ghi chú là "Chưa có tính năng liên quan" và tiếp tục luồng công việc chính. |
| Thiếu tham số | Quên `projectKey` hoặc `text` | Nhắc nhở bản thân lấy `projectKey` từ context hiện tại trước khi gọi tool. |
