---
name: fetch-project-guidelines
description: Tự động truy xuất danh mục và nội dung guidelines cấp độ PROJECT từ hệ thống, tích hợp cơ chế cache local để tối ưu hóa hiệu năng.
---

## Description
Kỹ năng tổ hợp (Composite Skill) giúp Lux tự động truy vấn danh mục hướng dẫn chuẩn cấp dự án (PROJECT) từ `local-mcp`. Để tối ưu hóa hiệu năng, tiết kiệm token và tránh gọi API lặp đi lặp lại, kỹ năng này tích hợp cơ chế cache guidelines cục bộ trong workspace và hỗ trợ kéo có chọn lọc theo phạm vi tài liệu cần xử lý.

## Triggers
- Kích hoạt đầu tiên ở Bước 1 của quy trình `documentation-process.md` khi bắt đầu quá trình biên soạn hoặc cập nhật tài liệu.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| target_docs | Array | Không | Danh sách các file tài liệu cụ thể cần viết/cập nhật (VD: `["01-overview.md", "02-roles-and-boundaries.md"]`). Nếu để trống, mặc định sẽ xử lý toàn bộ 10 tài liệu. |
| clear_cache | Boolean | Không | Set thành `true` khi muốn xóa cache local và kéo lại bản guidelines mới nhất từ hệ thống. Mặc định là `false`. |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| project_guidelines | JSON | Object chứa hướng dẫn cấu trúc chi tiết của các tài liệu level PROJECT được yêu cầu. |

## Steps
1. **Kiểm tra Cache cục bộ:**
   - Lux kiểm tra sự tồn tại của các file cache guidelines tại thư mục cục bộ `.agents/cache/guidelines/`.
   - Nếu `clear_cache = true`, tiến hành xóa toàn bộ thư mục cache cục bộ này trước khi đi tiếp.
2. **Kéo danh mục Level PROJECT từ máy chủ (nếu cần):**
   - Nếu cache local trống hoặc `clear_cache = true`:
     - Gọi tool `get_guideline_by_level` từ `local-mcp` với tham số `level="PROJECT"` để lấy danh sách file guidelines chuẩn của hệ thống.
     - Lưu danh sách này vào cache cục bộ.
   - Nếu cache đã có sẵn, đọc danh sách trực tiếp từ cache.
3. **Kéo chi tiết cấu trúc từng file chuẩn (Có chọn lọc):**
   - Xác định danh sách tài liệu mục tiêu dựa trên tham số `target_docs` (nếu không truyền, mặc định lấy toàn bộ danh sách level PROJECT).
   - Với mỗi tài liệu mục tiêu:
     - Nếu đã có file cache tương ứng tại `.agents/cache/guidelines/[name].md`: Đọc nội dung trực tiếp từ cache local.
     - Nếu chưa có cache hoặc bị xóa: Gọi tool `get_guideline_by_level_and_name` từ `local-mcp` với tham số `level="PROJECT"` và `name` (tên file chuẩn tương ứng), sau đó ghi nội dung tải về vào file cache cục bộ `.agents/cache/guidelines/[name].md`.
4. **Đóng gói đầu ra:**
   - Tổng hợp nội dung các guidelines đã đọc được vào một đối tượng JSON: `{ "tên_file.md": "nội_dung_guideline" }`.
   - Gán đối tượng này vào biến đầu ra `project_guidelines` phục vụ cho việc đối chiếu ở các bước tiếp theo.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Không có guideline | Hệ thống chưa cấu hình level PROJECT | Thông báo lỗi chi tiết cho User và dừng workflow. |
| Lỗi ghi cache local | Quyền truy cập thư mục bị hạn chế | Bỏ qua bước ghi cache, chuyển sang chế độ đọc/ghi trực tiếp vào bộ nhớ RAM tạm thời và tiếp tục workflow. |
| Tool timeout / API lỗi | Sự cố đường truyền hệ thống | Retry gọi tool tối đa 2 lần. Nếu vẫn thất bại, thông báo trực tiếp cho User. |
