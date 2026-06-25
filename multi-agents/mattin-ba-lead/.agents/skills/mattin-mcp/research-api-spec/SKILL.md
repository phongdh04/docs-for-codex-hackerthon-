---
name: research-api-spec
description: Tìm kiếm đặc tả API theo ngữ nghĩa gần đúng nhất qua mcp tool để đối chiếu review.
---

## Description
Sử dụng công cụ tìm kiếm ngữ nghĩa để tra cứu các tài liệu API Spec hoặc giao ước API cũ của một dự án cụ thể trên hệ thống, giúp đối chiếu chéo giao ước truyền nhận dữ liệu chính xác và khớp nhất khi review.

## Triggers
- Kích hoạt khi review tài liệu API Specs của Story cần tham chiếu và đối chiếu.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu duy nhất của dự án trên hệ thống (VD: `PAI`). |
| query | String | Có | Câu truy vấn tìm kiếm API (VD: `API đăng nhập`, `Kafka sync user`). |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| api_spec_content | String/Markdown | Đặc tả API và các endpoints khớp ngữ nghĩa nhất. |

## Steps
1. **Gọi công cụ tìm kiếm:**
   - Gọi tool `research_api_spec` từ `local-mcp` truyền vào hai tham số: `projectKey` và `query`.
2. **Bàn giao:** Nạp kết quả tìm kiếm vào context `api_spec_content`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|----------------|-------------|------------|
| Không tìm thấy kết quả | Không có API nào khớp ngữ nghĩa | Báo cáo User, tiếp tục review dựa trên guidelines chung. |
