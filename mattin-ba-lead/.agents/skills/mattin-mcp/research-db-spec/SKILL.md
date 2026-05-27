---
name: research-db-spec
description: Tìm kiếm thiết kế database hoặc đặc tả bảng dữ liệu theo ngữ nghĩa gần đúng nhất qua mcp tool để đối chiếu review.
---

## Description
Sử dụng công cụ tìm kiếm ngữ nghĩa để tra cứu các tài liệu DB Spec hoặc cấu trúc bảng dữ liệu cũ của một dự án cụ thể trên hệ thống, giúp đối chiếu chéo cấu trúc dữ liệu chính xác và khớp nhất khi review.

## Triggers
- Kích hoạt khi review tài liệu DB Specs của Story cần đối chiếu cấu trúc database cũ.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| projectKey | String | Có | Mã hiệu duy nhất của dự án trên hệ thống (VD: `PAI`). |
| query | String | Có | Câu truy vấn tìm kiếm cấu trúc DB (VD: `bảng user`, `gói cước môi giới`). |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| db_spec_content | String/Markdown | Đặc tả cơ sở dữ liệu và cấu trúc các bảng khớp ngữ nghĩa nhất. |

## Steps
1. **Gọi công cụ tìm kiếm:**
   - Gọi tool `research_db_table_spec` từ `local-mcp` truyền vào hai tham số: `projectKey` và `query`.
2. **Bàn giao:** Nạp kết quả tìm kiếm vào context `db_spec_content`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|----------------|-------------|------------|
| Không tìm thấy kết quả | Không có bảng nào khớp ngữ nghĩa | Báo cáo User, bỏ qua bước đối chiếu DB cũ và tiếp tục review. |
