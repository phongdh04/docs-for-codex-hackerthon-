---
name: react-tailwind-builder
description: Biên soạn giao diện UI React component chuẩn Responsive Mobile-first, TailwindCSS tối giản, Framer Motion animations 60fps.
---

## Description
Dựng các file UI React Component chuẩn Responsive Mobile-first (ưu tiên di động trước, scale lên PC sau) bằng TailwindCSS tối giản và Framer Motion animations đạt hiệu năng 60fps mượt mà trên di động.

## Triggers
- Kích hoạt khi xây dựng hoặc cập nhật bất kỳ trang, layout, component React nào trong dự án.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| componentName | String | Có | Tên component cần tạo. |
| uiDescription | String | Có | Mô tả nghiệp vụ giao diện và tương tác. |
| collectedContext | String | Không | Bối cảnh hoặc cấu trúc dữ liệu thô từ API. |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| component_code | String | Mã nguồn React component hoàn chỉnh viết bằng JSX + Tailwind. |

## Steps
1. **Responsive Mobile-First Layout:**
   - Dựng giao diện mặc định cho di động, scale lên PC bằng các prefix Tailwind `md:`, `lg:`.
   - Nút bấm to, spacing hợp lý, dễ tương tác bằng một tay.
2. **Animation 60fps (Framer Motion):**
   - Áp dụng `motion.div` cho menu, chat slide-in, cards fade-in.
   - Chỉ sử dụng thuộc tính được tăng tốc phần cứng (`transform`, `opacity`) để tối ưu hiệu năng GPU.
3. **Tối ưu Re-rendering & Bundle Size:**
   - Sử dụng `React.memo`, `useMemo`, `useCallback` tại các component danh sách lớn để triệt tiêu re-render thừa.
   - Áp dụng dynamic import (`React.lazy` + `Suspense`) để chia nhỏ bundle size.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|----------------|-------------|------------|
| Vỡ layout di động | Code responsive classes sai | Đảm bảo các class cơ sở (không có tiền tố) là dành cho di động, sau đó mới thêm prefix cho PC. |
| Giật lag animation | Animation tác động trực tiếp vào layout (width, height, margin) | Refactor chỉ animation bằng `scale`, `x`, `y`, `opacity`. |
