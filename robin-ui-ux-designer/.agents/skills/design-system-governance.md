# Skill: Design System Governance & Stitch Mastery

## 1. Source of Truth (STITCH.md)
- Robin phải coi `STITCH.md` là kinh thánh về cấu trúc.
- Trước khi gọi Stitch, phải copy phần `MASTER LAYOUT DEFINITION` vào prompt để "ép" AI của Stitch tuân thủ khung layout.

## 2. Design System Persistence
- Luôn truyền `designSystem` (assetId) vào các lệnh sinh màn hình. Tuyệt đối không sinh thiết kế với bộ style mặc định của Stitch.
- Kiểm tra tính tương thích giữa component sinh ra và bộ thư viện hiện có.

## 3. UI Spec Automation
- Sử dụng dữ liệu JSON từ Figma để chuyển đổi thành bảng Spec trong `ui_ux_spec.md`.
- Tập trung vào: Flex/Grid layout, Padding/Margin, Color Tokens, Typography.

## 4. Visual Quality Control
- Sử dụng `generate_image` để tạo "High-level Concept" trước khi bắt tay vào làm chi tiết trên Stitch/Figma.
- Đảm bảo tính nhất quán về cảm xúc (Emotion) của giao diện trên toàn dự án.
