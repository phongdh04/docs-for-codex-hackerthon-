---
name: HeyEziPWA
colors:
  primary: "#d83a1a"
  secondary: "#f5a623"
  surface: "#ffffff"
  surface-variant: "#fffaf5"
  on-surface: "#2d3748"
  error: "#e53e3e"
typography:
  body-md:
    fontFamily: Be Vietnam Pro
    fontSize: 16px
    fontWeight: 400
rounded:
  md: 12px
---

# Design System - HeyEziPWA

## Overview
Giao diện sáng (Light Theme) hiện đại, thân thiện và tối giản dành cho ứng dụng Trợ lý Marketing. Thiết kế tập trung vào việc giảm thiểu sự phức tạp cho các nhà môi giới phổ thông, giúp họ dễ dàng quản lý chiến dịch và tương tác mượt mà với Trợ lý AI.

## Colors
- **Primary** (#d83a1a): Sắc cam đỏ thương hiệu lấy cảm hứng từ logo Ezi Web. Dành cho các nút hành động chính (CTA), trạng thái kích hoạt và các điểm nhấn tương tác quan trọng.
- **Secondary** (#f5a623): Màu vàng cam ấm áp từ dải gradient trang chủ. Sử dụng cho các hành động phụ, các nhãn trạng thái (tag/chip) và phân loại dự án.
- **Surface** (#ffffff): Màu nền của các thẻ (Card), ô nhập liệu và khu vực nội dung chính.
- **Surface-variant** (#fffaf5): Màu nền kem nhạt ấm áp của ứng dụng, giúp phân biệt các vùng giao diện mà không gây mỏi mắt.
- **On-surface** (#2d3748): Sắc xám than đậm cho toàn bộ văn bản chính để đảm bảo độ tương phản cao, dễ đọc cho người dùng phổ thông.
- **Error** (#e53e3e): Đỏ cảnh báo cho các thông tin lỗi hoặc các hành động xóa dữ liệu nguy hiểm.

## Typography
- **Headlines**: Be Vietnam Pro, Bold (700) hoặc Semi-bold (600) tạo điểm nhấn rõ ràng, hiện đại và hỗ trợ tiếng Việt hoàn hảo.
- **Body**: Be Vietnam Pro, Regular (400), cỡ chữ từ 14px – 16px để tối ưu khả năng đọc thông tin chiến dịch.
- **Labels**: Be Vietnam Pro, Medium (500), 12px dành cho các thẻ tag dự án và tiêu đề phụ.

## Components
- **Buttons**: Bo góc lớn (12px) để tạo cảm giác thân thiện, dễ chạm. Nút chính sử dụng màu đỏ cam (`#d83a1a`) phủ bóng mờ nhẹ (drop shadow) để kích thích tương tác.
- **Inputs**: Bo góc 8px, viền mảnh 1px màu xám kem nhạt. Khi active sẽ đổi viền sang màu `primary`.
- **Cards (Thẻ dự án/Dashboard)**: Bo góc 12px, sử dụng bóng đổ cực nhẹ (Soft Shadow) thay vì viền đậm để tạo không gian thoáng đãng, hiện đại.
- **Chat Widget (Trợ lý AI)**: 
  * Bong bóng chat của Trợ lý: Nền xám kem nhạt (`#fffaf5`), văn bản màu đen xám.
  * Bong bóng chat của User: Nền cam đỏ thương hiệu (`#d83a1a`), văn bản màu trắng.
  * Khung nhập liệu Chat: Thiết kế dạng thanh bo tròn hoàn toàn (Capsule shape - 24px) để tối ưu trải nghiệm gõ phím.

## Do's and Don'ts
- **Do**: Luôn duy trì khoảng trắng (white space) rộng rãi giữa các widget dashboard để người dùng không bị ngợp thông tin.
- **Don't**: Không sử dụng quá nhiều màu sắc khác nhau cho các trạng thái chiến dịch; chỉ dùng thang màu `primary` và `secondary` để biểu thị tiến độ.
- **Do**: Luôn đặt khung Chat với Trợ lý ở vị trí dễ tiếp cận nhất (góc dưới bên phải hoặc tab cố định riêng biệt).