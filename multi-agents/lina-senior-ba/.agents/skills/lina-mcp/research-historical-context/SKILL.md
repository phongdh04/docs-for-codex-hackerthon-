---
name: research-historical-context
description: Tìm kiếm bối cảnh và tài liệu Epic/Story cũ theo ngữ nghĩa gần đúng nhất qua mcp tool.
---

## Description
Sử dụng công cụ tìm kiếm ngữ nghĩa để tra cứu các tài liệu Epic Brief hoặc Story Specs cũ của một dự án cụ thể trên hệ thống, giúp tìm ra các tính năng tương đồng có độ khớp gần nhất.

## Triggers
- Kích hoạt khi phân tích yêu cầu mới cần rà soát lịch sử các tính năng hoặc nghiệp vụ tương đồng trong dự án.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu duy nhất của dự án trên hệ thống (VD: `PAI`). |
| query | String | Có | Câu truy vấn tìm kiếm ngữ nghĩa (VD: `Đăng nhập qua JWT`, `gán gói cước môi giới`). |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| historical_context | String/Markdown | Danh sách tài liệu và nội dung cũ có độ khớp ngữ nghĩa gần nhất để tham chiếu. |

## Steps
1. **Tìm kiếm ngữ nghĩa (Semantic Search):**
   - Gọi MCP tool **`research_document`** từ `local-mcp` truyền vào hai tham số: `projectKey` và `query` để lấy danh sách các Epic/Story ID tương đồng có độ khớp ngữ nghĩa gần nhất.
2. **Đọc nội dung chi tiết (Read Details):**
   - Duyệt qua danh sách tài liệu trả về từ bước 1.
   - Với mỗi tài liệu (Epic hoặc Story), sử dụng lệnh **`read_resource`** để đọc trực tiếp nội dung chi tiết thông qua các MCP Resources tĩnh tương ứng (`epic-document://[epicKey]` hoặc `story-document://[storyKey]`).
3. **Trích xuất bối cảnh:**
   - Tổng hợp nội dung chi tiết thu được và nạp vào biến đầu ra `historical_context`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|----------------|-------------|------------|
| Không tìm thấy kết quả | Không có tài liệu nào khớp ngữ nghĩa | Báo cáo User và ghi nhận "Không có bối cảnh lịch sử tương đồng", tiếp tục phân tích từ đầu. |
| Tool timeout / API lỗi | Sự cố đường truyền hệ thống | Thực hiện Retry gọi tool tối đa 2 lần. Nếu vẫn thất bại, thông báo User và bỏ qua bước đối chiếu lịch sử. |
