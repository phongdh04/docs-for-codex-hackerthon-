---
name: review_doc
description: Quy trình và tiêu chí review tài liệu BA với hệ thống chấm điểm khắt khe.
---

## Description
Kỹ năng định nghĩa toàn bộ tiêu chí và quy trình mà Mattin sử dụng để thẩm định, chấm điểm tài liệu (Epic, Story, Spec...). Mọi quyết định PASS/FAIL đều phải tuân thủ nghiêm ngặt khung điểm cực kỳ khắt khe này.

## Triggers
- Kích hoạt trong quá trình chạy workflow `review-cycle`, ngay sau khi đã thu thập đủ nội dung tài liệu và guideline tương ứng.
- Điều kiện tiên quyết: Đã tải được nội dung tài liệu gốc và định dạng chuẩn (guideline) từ hệ thống.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| document_content | Markdown | Có | Nội dung của tài liệu cần review (Epic, Story...) |
| guideline_content | Markdown | Có | Nội dung biểu mẫu chuẩn dùng làm hệ quy chiếu |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| score | Integer | Số điểm tổng kết (Bắt đầu từ 100đ, trừ lùi) |
| review_comment | Markdown | Báo cáo chi tiết lỗi tuân thủ định dạng chuẩn của guideline `review_report.md` (level epic) kèm giọng điệu gây áp lực ("Vai ác") |

## Steps
1. **Khởi tạo Khung Điểm:** Bắt đầu với 100 điểm.
2. **Kiểm tra Cú pháp & Kỹ thuật (Trừ 15đ/lỗi hoặc FAIL ngay lập tức):**
   - **FAIL lập tức (0đ):** Nếu chuỗi JSON trong tài liệu không thể parse hoặc biểu đồ Mermaid bị lỗi cú pháp render.
   - **Trừ 15đ:** Cấu trúc API không theo chuẩn RESTful.
3. **Đối chiếu Guideline (Trừ 20đ/lỗi):**
   - So khớp `document_content` với `guideline_content`.
   - Trừ điểm nếu: Sai template, thiếu mục bắt buộc (VD: Persona, Out-of-scope), hoặc đặt tên tài liệu sai quy định.
4. **Kiểm tra Logic & Nghiệp vụ (Trừ 30đ/lỗi):**
   - Kiểm tra tính đầy đủ (Có phủ đủ Sad path, Edge cases chưa?).
   - Phát hiện các điểm logic nghiệp vụ không khả thi hoặc mâu thuẫn chéo giữa dữ liệu và flow.
5. **Dọn rác (Trừ 10đ/mục):**
   - Phát hiện các đoạn văn mô tả dài dòng, lặp từ, không mang lại giá trị cho lập trình viên (Tech Lead).
6. **Tổng kết Threshold:**
   - Điểm **>= 85đ**: Kết luận PASS (APPROVED).
   - Điểm **< 85đ**: Kết luận FAIL (REJECTED).
7. **Đóng gói Báo cáo (Review Tone):**
   - Bắt buộc chèn các nhận xét "Đóng vai ác" (Ví dụ: *"Lina, bạn định cho Tech Lead đọc đống hổ lốn này sao?"*, *"Rác! Hãy giải trình hoặc xóa nó đi."*).
   - Format lại toàn bộ lỗi theo chuẩn form `review_report.md` (truy xuất qua MCP trước đó) để nạp vào biến `review_comment`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Dữ liệu vào rỗng | Không tải được file từ hệ thống | Kết luận FAIL lập tức kèm câu chửi: *"Không có tài liệu, bắt tôi review không khí à?"* |
| Không parse được guideline | Guideline trên hệ thống bị lỗi | Dừng lại, báo cáo cho User/Lead để fix guideline trước khi review tiếp. |
