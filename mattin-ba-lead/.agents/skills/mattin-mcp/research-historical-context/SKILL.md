---
name: research-historical-context
description: Tìm kiếm bối cảnh và tài liệu Epic/Story cũ theo ngữ nghĩa gần đúng nhất qua mcp tool để đối chiếu review.
---

## Description
Sử dụng công cụ tìm kiếm ngữ nghĩa để tra cứu các tài liệu Epic Brief hoặc Story Specs cũ của một dự án cụ thể trên hệ thống, giúp đối chiếu tính nhất quán lịch sử có độ khớp gần nhất với yêu cầu hiện tại khi review.

## Triggers
- Kích hoạt khi review tài liệu mới cần rà soát lịch sử các tính năng hoặc nghiệp vụ tương đồng trong dự án.

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
1. **Gọi công cụ tìm kiếm:**
   - Gọi tool `research_document` từ `local-mcp` truyền vào hai tham số: `projectKey` và `query`.
2. **Trích xuất bối cảnh:**
   - Thu nhận danh sách tài liệu khớp ngữ nghĩa gần nhất và nạp nội dung của chúng vào biến đầu ra `historical_context`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|----------------|-------------|------------|
| Không tìm thấy kết quả | Không có tài liệu nào khớp ngữ nghĩa | Báo cáo User và ghi nhận "Không có bối cảnh lịch sử tương đồng", tiếp tục review cấu trúc nội tại. |
| Tool timeout / API lỗi | Sự cố đường truyền hệ thống | Thực hiện Retry gọi tool tối đa 2 lần. Nếu vẫn thất bại, thông báo User và bỏ qua bước đối chiếu lịch sử. |
