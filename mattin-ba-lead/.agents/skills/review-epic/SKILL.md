---
name: review-epic
description: Thẩm định tài liệu Epic Brief (brief.md) của BA dựa trên 4 nhóm tiêu chí chất lượng nghiêm ngặt và chấm điểm.
---

## Description
Kỹ năng giúp Mattin phân tích chuyên sâu nội dung tài liệu Epic Brief (`brief.md`) của BA, chấm điểm dựa trên thang 100 theo 4 nhóm tiêu chí (Nghiệp vụ, Truyền tải, Kiểm thử, Chuẩn hóa). Nếu điểm đạt >= 85, gán trạng thái `PASSED`, ngược lại gán `FAILED` và đính kèm báo cáo lỗi chi tiết sử dụng văn phong nghiêm khắc của BA Lead.

## Triggers
- Kích hoạt tại Bước 3 của quy trình `review-epic.md`.
- Điều kiện tiên quyết: Đã tải thành công tài liệu `brief.md` và bối cảnh Epic từ kỹ năng `get-epic-context`.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| epicDocuments | Markdown | Có | Danh sách tài liệu đính kèm của Epic chứa nội dung file `brief.md`. |
| projectKey | String | Có | Mã hiệu dự án để đối chiếu bối cảnh. |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| status | String | Kết quả thẩm định (`PASSED` hoặc `FAILED`). |
| comment | String | Nội dung báo cáo review chi tiết (Markdown) chỉ rõ các điểm lỗi và điểm số đạt được. |

## Steps
1. **Phân tích bối cảnh & guidelines đối chiếu:**
   - Đọc các tài liệu kiến trúc dự án (`Overview`, `Tech Stack`, `Repos`) qua `read_resource`.
   - Đọc barem biểu mẫu chuẩn của Epic Brief qua kỹ năng `fetch-guideline`.
2. **Thẩm định tài liệu theo 4 nhóm tiêu chí (Thang điểm 100):**
   - **Nhóm 1: Nội dung & Nghiệp vụ (Business Alignment - Tối đa 30 điểm):**
     - Giải quyết đúng Pain Point thực tế không? Có nằm ngoài phạm vi đã chốt không?
     - Đã bao quát đủ Happy Path, Edge Cases (lỗi hệ thống, ngoại lệ) và Non-functional Requirements (hiệu năng, bảo mật) chưa?
     - Tính nhất quán nghiệp vụ giữa các phần, có đá nhau với logic hệ thống cũ không?
   - **Nhóm 2: Kỹ thuật viết & Truyền tải (Clarity & Structure - Tối đa 25 điểm):**
     - Rõ ràng, một nghĩa. Loại bỏ các từ định tính mơ hồ ("đẹp", "nhanh", "tối ưu"), bắt buộc định lượng rõ ràng.
     - Dễ đọc, cấu trúc mục lục logic, đánh số rõ ràng.
     - Tính truy xuất (Traceability) map ngược về Business Goal.
   - **Nhóm 3: Triển khai & Kiểm thử (Feasibility & Testability - Tối đa 25 điểm):**
     - Tính khả thi về công nghệ, repo và timeline (đối chiếu tech stack và repositories registry).
     - Tính kiểm thử được, Acceptance Criteria (AC) rõ ràng (khuyến khích viết theo Given-When-Then).
   - **Nhóm 4: Hình thức & Chuẩn hóa (Standardization & Visualization - Tối đa 20 điểm):**
     - Đúng template Markdown quy chuẩn.
     - Sơ đồ (Mermaid, Flowchart) vẽ chuẩn cú pháp, khớp với phần giải thích chữ.
3. **Chấm điểm và kết luận trạng thái:**
   - Tính tổng điểm.
   - Nếu tổng điểm `>= 85`: Gán `status` = `PASSED`.
   - Nếu tổng điểm `< 85` hoặc có lỗi cú pháp Mermaid nghiêm trọng: Gán `status` = `FAILED`.
4. **Biên soạn Báo cáo Review (Review Report):**
   - Báo cáo viết theo văn phong nghiêm khắc, sắc bén của BA Lead.
   - Định dạng báo cáo gồm:
     - Điểm số đạt được & Kết luận (PASSED/FAILED).
     - Chi tiết lỗi sai chia theo 4 nhóm tiêu chí.
     - Yêu cầu hành động hiệu chỉnh cho BA.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| Không trích xuất được brief.md | Payload tài liệu bị trống hoặc sai định dạng | Đánh giá FAIL ngay lập tức (0 điểm) với lý do: "Không tìm thấy file brief.md trong tài nguyên Epic". |
