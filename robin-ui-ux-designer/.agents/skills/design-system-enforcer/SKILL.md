---
name: design-system-enforcer
description: Rà soát và giải quyết mọi xung đột thiết kế giữa concept_note.md và DESIGN.md trực tiếp với User.
---

## Description
Kỹ năng giúp Robin đối chiếu ý tưởng thô từ `concept_note.md` với hệ thống thiết kế chuẩn `DESIGN.md`. Nếu phát hiện bất kỳ điểm mâu thuẫn nào (về màu sắc, font, layout, component), Robin sẽ thảo luận trực tiếp với User để giải quyết xung đột, đảm bảo tính nhất quán thẩm mỹ cao nhất trước khi sinh giao diện.

## Triggers
- Kích hoạt ở Bước 2 của quy trình `design-creation-process.md` ngay sau khi tiếp nhận `concept_note.md`.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| concept_note | String | Có | Nội dung ý tưởng thô của màn hình cần thiết kế. |
| design_system | String | Có | Các quy chuẩn thiết kế từ `DESIGN.md`. |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| review_status | String | Kết quả rà soát (APPROVED_BY_DESIGN / CONFLICT_FOUND). |
| conflict_report | String | Báo cáo chi tiết các điểm xung đột và đề xuất sửa đổi gửi User. |

## Steps
1. **Đối chiếu & Rà soát xung đột:**
   - So sánh các mô tả về màu sắc, kiểu dáng button, góc bo tròn, font chữ trong `concept_note` với các hằng số và quy định cụ thể trong `DESIGN.md`.
   - Ghi chú lại mọi điểm mâu thuẫn (Ví dụ: Yêu cầu dùng màu đỏ `#FF0000` trong khi primary color của hệ thống là cam đỏ `#d83a1a`).
2. **Thảo luận và giải quyết xung đột với User:**
   - *Nếu phát hiện xung đột:*
     - Lập báo cáo `conflict_report` ngắn gọn, chỉ rõ các lỗi mâu thuẫn và đề xuất phương án thay thế tối ưu (tuân thủ `DESIGN.md`).
     - Gửi báo cáo này trực tiếp trong chat cho User và xin ý kiến chốt duyệt: *"Ý tưởng của bạn có một số điểm chưa khớp với Design System EziAssistant. Tôi đề xuất điều chỉnh như sau để đảm bảo tính đồng bộ thương hiệu..."*.
     - Gán `review_status = "CONFLICT_FOUND"`.
   - *Nếu hoàn toàn nhất quán (hoặc User đã chốt duyệt phương án chỉnh sửa):*
     - Gán `review_status = "APPROVED_BY_DESIGN"` và chuyển giao sang bước sinh giao diện.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|----------------|-------------|------------|
| User giữ nguyên ý kiến | User muốn phá vỡ Design System một cách có chủ đích | Ghi nhận quyết định của User, lưu ý kiến của User vào bối cảnh thiết kế và tiếp tục chuyển giao sang Bước 3 theo đúng chỉ đạo (User chốt là tối cao). |
