---
name: context-collector
description: Rà soát mã nguồn thực tế và tài liệu cũ, phân tích điểm khuyết thông tin và lập bộ câu hỏi phỏng vấn tối giản gửi trực tiếp cho User.
---

## Description
Kỹ năng giúp Lux thu thập bối cảnh thực tế bằng cách quét cấu trúc mã nguồn trong workspace cục bộ và đối chiếu với tài liệu hiện có được tải về từ máy chủ. Từ đó, tiến hành phân tích khoảng trống tri thức (Gap Analysis) so với yêu cầu của Guidelines, và tự động lập một bộ câu hỏi phỏng vấn tối giản gửi trực tiếp duy nhất cho User để làm rõ các điểm nghiệp vụ hoặc quyết định kỹ thuật còn thiếu.

## Triggers
- Kích hoạt ở Bước 2 của quy trình `documentation-process.md` sau khi đã fetch xong guidelines chuẩn và tài liệu cũ (nếu có).

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| project_guidelines | JSON | Có | Object chứa hướng dẫn cấu trúc chuẩn level PROJECT. |
| existing_docs | JSON | Không | Object chứa nội dung các file tài liệu hiện tại của dự án đã tải từ máy chủ (dùng khi cập nhật). |
| target_docs | Array | Có | Danh sách các file tài liệu đang thực hiện biên soạn/cập nhật. |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| interview_questions | String | Bộ câu hỏi phỏng vấn tập trung cao độ, gửi trực tiếp duy nhất cho User (tối đa 5 câu). |
| initial_context | JSON | Object chứa các thông tin nghiệp vụ/kỹ thuật thô đã thu thập được từ mã nguồn cục bộ và tài liệu cũ. |

## Steps
1. **Quét mã nguồn cục bộ để thu thập bối cảnh kỹ thuật:**
   - Thực hiện quét cấu trúc thư mục, đọc các file cấu hình chính (như `package.json`, `mcp_config.json`, các file config framework...) trong workspace cục bộ của dự án.
   - Trích xuất thông tin kỹ thuật thực tế: Ngôn ngữ lập trình, thư viện/công nghệ sử dụng, cấu trúc thư mục, các endpoints (nếu có) để làm chất liệu viết tài liệu.
2. **Đối chiếu & Phân tích điểm khuyết (Gap Analysis):**
   - So sánh bối cảnh thô đã trích xuất từ mã nguồn + nội dung tài liệu cũ (`existing_docs`) với 5 tiểu mục bắt buộc của từng file hướng dẫn trong `project_guidelines`.
   - Xác định rõ các phần thông tin nghiệp vụ (như mục tiêu dự án, luồng nghiệp vụ cụ thể) hoặc các quyết định kiến trúc không thể suy luận được từ mã nguồn.
3. **Lập bộ câu hỏi phỏng vấn duy nhất cho User:**
   - Gom tất cả các điểm khuyết thông tin cốt lõi thành một danh sách câu hỏi phỏng vấn.
   - **Quy tắc Vàng bắt buộc:**
     - **Chỉ phỏng vấn User**, tuyệt đối không giao tiếp hay chuyển câu hỏi sang bất kỳ Agent nào khác.
     - Tối giản hóa và chắt lọc tối đa **dưới 5 câu hỏi** trong một lượt chat để tránh hỏi lắt nhắt (tuân thủ Guardrail *No Scattered Questions*).
     - Câu hỏi phải rõ ràng, đi thẳng vào các lựa chọn nghiệp vụ hoặc thông tin kiến trúc bị khuyết.
4. **Bàn giao kết quả:**
   - Xuất danh sách câu hỏi phỏng vấn ra cuộc chat cho User và trả về biến `initial_context` chứa bối cảnh kỹ thuật đã quét được.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Workspace cục bộ trống | Dự án mới tinh, chưa khởi tạo mã nguồn | Bỏ qua bước quét mã nguồn. Chuyển sang phỏng vấn User 3 câu hỏi vĩ mô cốt lõi: "Mục tiêu dự án là gì?", "Các vai trò người dùng chính?", "Công nghệ chủ chốt dự định sử dụng?". |
| Không phát hiện điểm khuyết | Bối cảnh từ mã nguồn và tài liệu cũ đã đầy đủ | Trả về thông báo "Thông tin đầy đủ, sẵn sàng soạn thảo" và tự động kích hoạt bước soạn thảo tiếp theo mà không cần phỏng vấn. |
