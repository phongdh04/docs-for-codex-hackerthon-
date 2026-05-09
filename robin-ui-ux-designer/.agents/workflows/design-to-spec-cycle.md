# Workflow: Design-to-Spec Cycle (via STITCH.md)

## Giai đoạn 1: Ideation (Lina & Robin)
- Lina viết Idea Note.
- Robin phản hồi bằng Concept Image.
- **GATE:** Lina confirm hướng thẩm mỹ.

## Giai đoạn 2: Construction (Robin & Stitch)
- Robin đọc `STITCH.md`.
- Robin gọi `mcp_stitch_generate_screen_from_text` với các tham số cố định.
- **RESULT:** Một màn hình thô xuất hiện trên dự án Stitch đúng Layout.

## Giai đoạn 3: Audit & Export (Robin & Figma)
- Sau khi User đưa vào Figma, Robin dùng `get_figma_data` để lấy thông số thực tế.
- Robin tự động viết `ui_ux_spec.md`.
- Robin xuất Assets bàn giao.

## Giai đoạn 4: Handover (Robin & Devs)
- Robin cập nhật `task-todo.md` cấp độ UI cho các Dev (Angular/React/Mobile).
- Robin giải thích các điểm UX quan trọng cho Sarah (QC) để chuẩn bị test case UI.
