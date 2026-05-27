---
name: stitch-handover
description: Bàn giao link thiết kế Stitch cho User và quản lý vòng lặp phản hồi sửa đổi.
---

## Description
Kỹ năng giúp Robin trích xuất đường dẫn thiết kế (Stitch URL) từ kết quả sinh giao diện của StitchMCP, bàn giao trực tiếp cho User truy cập kiểm tra và quản lý chu trình tiếp nhận feedback chỉnh sửa.

## Triggers
- Kích hoạt ở Bước 4 của quy trình `design-creation-process.md` sau khi Stitch đã sinh màn hình thành công.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| stitch_response | JSON | Có | Phản hồi chi tiết từ mcp tool `generate_screen_from_text` (chứa URL thiết kế). |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| handover_status | String | Trạng thái bàn giao (HANDED_OVER / WAITING_FOR_FEEDBACK). |

## Steps
1. **Trích xuất link thiết kế:**
   - Đọc kết quả `stitch_response` từ server.
   - Tìm kiếm và trích xuất đường dẫn liên kết đến màn hình thiết kế trên Stitch (Stitch URL).
2. **Bàn giao cho User:**
   - Gửi link thiết kế trực tiếp trong chat cho User với thông điệp ngắn gọn: *"Màn hình thiết kế đã được sinh thành công trên Stitch. Bạn có thể truy cập và kiểm tra trực tiếp tại link sau: [Stitch Design Link]"*.
   - Khuyến khích User đưa ra phản hồi: *"Nếu bạn muốn điều chỉnh bất kỳ chi tiết nào (layout, text, tương tác), hãy cho tôi biết."*
3. **Quản lý Vòng lặp Phản hồi (Feedback Loop):**
   - Chờ nhận feedback chỉnh sửa từ User.
   - *Nếu User yêu cầu sửa đổi:* Kích hoạt quay ngược lại **Bước 2** (đối chiếu `DESIGN.md`) để thực hiện chu trình cập nhật tiếp theo.
   - *Nếu User chốt duyệt hoàn toàn:* Kết thúc quy trình thiết kế thành công.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|----------------|-------------|------------|
| Không lấy được link từ response | API Stitch thay đổi cấu trúc trả về | Rà soát log response, gọi tool `get_project` để truy tìm lại danh sách screen và link của project, sau đó gửi cho User. |
