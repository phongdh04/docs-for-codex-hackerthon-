---
name: report-review-epic
description: Báo cáo kết quả review Epic lên hệ thống thông qua MCP tool `report_review_epic`.
---

## Description
Kỹ năng giúp Mattin đẩy kết quả thẩm định và báo cáo review chi tiết của Epic lên hệ thống qua MCP tool `report_review_epic`, đồng bộ hóa dữ liệu đánh giá và gửi phản hồi đến BA.

## Triggers
- Kích hoạt tại Bước 4 của quy trình `review-epic.md`.
- Điều kiện tiên quyết: Kỹ năng `review-epic` đã hoàn tất việc thẩm định và cung cấp `status` (PASSED/FAILED) cùng báo cáo chi tiết `comment`.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| projectKey | String | Có | Mã hiệu dự án (ví dụ: `PAI`). |
| epicKey | String | Có | Mã hiệu Epic cần báo cáo (ví dụ: `PAI-EPIC-46`). |
| status | String | Có | Trạng thái thẩm định (`PASSED` hoặc `FAILED`). |
| title | String | Có | Tiêu đề của báo cáo (ví dụ: `BA Lead Review - PAI-EPIC-46`). |
| content | String | Có | Nội dung báo cáo review chi tiết chứa điểm số và các lỗi sai cần khắc phục. |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| result | Object | Kết quả thô trả về từ việc gọi MCP tool `report_review_epic` để xác nhận việc cập nhật thành công. |

## Steps
1. **Chuẩn bị tham số:** Định cấu hình tham số đầu vào cho tool bao gồm:
   - `projectKey`: Mã hiệu dự án.
   - `epicKey`: Mã hiệu Epic.
   - `status`: Giá trị `"PASSED"` hoặc `"FAILED"`.
   - `title`: Tiêu đề báo cáo.
   - `content`: Nội dung comment chi tiết từ kỹ năng `review-epic`.
2. **Gọi MCP Tool:** Thực hiện cuộc gọi tới tool **`report_review_epic`** (thuộc server `local-mcp`) với các tham số đã chuẩn bị.
3. **Xác nhận kết quả:** Kiểm tra phản hồi từ tool để đảm bảo hệ thống đã nhận và cập nhật báo cáo thành công. Báo cáo kết quả cho User.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Gọi tool thất bại (Timeout/API Error) | Sự cố kết nối hoặc lỗi phía server | Thử lại tối đa 2 lần. Nếu vẫn lỗi, ghi nhận log và thông báo User để kiểm tra kết nối MCP server. |
| Thiếu tham số bắt buộc | Tham số đầu vào từ bước trước bị thiếu | Quay lại bước thẩm định để sinh lại báo cáo hoặc báo lỗi chi tiết ra màn hình cho User. |
