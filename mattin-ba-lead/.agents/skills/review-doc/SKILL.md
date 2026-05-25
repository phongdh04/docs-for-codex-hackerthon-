---
name: review-doc
description: Quy trình và tiêu chí review tài liệu BA với hệ thống chấm điểm khắt khe.
---

## Description
Kỹ năng định nghĩa toàn bộ tiêu chí và quy trình mà Mattin sử dụng để thẩm định, chấm điểm tài liệu (Epic, Story, Spec...). Mọi quyết định PASS/FAIL đều phải tuân thủ nghiêm ngặt khung điểm cực kỳ khắt khe này.

## Triggers
- Kích hoạt trong quá trình chạy workflow `review-cycle`, ngay sau khi đã thu thập đủ nội dung tài liệu và guideline tương ứng.
- Điều kiện tiên quyết: Đã tải được nội dung tài liệu gốc và định dạng chuẩn (guideline) từ hệ thống.

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết                                                                         |
|-----|--------------|----------|----------------------------------------------------------------------------------------|
| document_content | Markdown | Có | Nội dung của tài liệu cần review (Epic, Story...)                                      |
| guideline_content | Markdown | Có | Nội dung biểu mẫu chuẩn dùng làm hệ quy chiếu                                          |
| context_content | Markdown | Không | Dữ liệu ngữ cảnh (ticket gốc, Q&A log, Project Docs) để kiểm tra độ khớp của nghiệp vụ |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| score | Integer | Số điểm tổng kết (Bắt đầu từ 100đ, trừ lùi) |
| review_comment | Markdown | Báo cáo chi tiết lỗi tuân thủ định dạng chuẩn của guideline `review_report.md` (level `EPIC`) kèm giọng điệu gây áp lực ("Vai ác") |

## Steps
1. **Khởi tạo Khung Điểm:** Bắt đầu với 100 điểm.
2. **Ma trận Đánh giá (Evaluation Matrix):** Mattin sẽ đối chiếu tài liệu đầu vào dựa trên các tiêu chí sau.

| Tiêu chí | File Reference (Nguồn tham chiếu) | Dữ liệu đối chiếu | Yêu cầu (Mục tiêu) | Điểm trừ nếu vi phạm |
| :--- | :--- | :--- | :--- | :--- |
| **1. Kỹ thuật & Cú pháp** | Bộ guideline gốc của Cấu trúc JSON/Mermaid và API Spec | Mã nguồn / JSON / Mermaid trong `document_content` | - Chuỗi JSON phải parse được.<br>- Biểu đồ Mermaid render thành công.<br>- Thiết kế API chuẩn RESTful. | - Lỗi Parse JSON / Render Mermaid: **FAIL LẬP TỨC (0đ)**<br>- Lỗi cấu trúc API: **-15đ/lỗi** |
| **2. Chuẩn mực Guideline** | `guideline_content` (từ MCP fetch_guideline) | `guideline_content` so với `document_content` | - Tuân thủ các đầu mục trong guideline. | **-20đ/lỗi vi phạm** |
| **3. Logic & Yêu cầu gốc** | `context_content` (Requirements, QA log từ MCP) | `context_content` so với `document_content` | - Đánh giá chính xác các tính năng In-scope và Out-of-scope.<br>- Tính năng đề xuất BẮT BUỘC phải giải quyết đúng yêu cầu/QA gốc (Không tự chế thêm scope).<br>- Phủ đầy đủ Sad Path, Edge cases.<br>- Logic luồng dữ liệu không mâu thuẫn. | **-30đ/lỗi vi phạm** (Lỗi rất nặng) |
| **4. Độ tinh gọn (Clean)** | Tiêu chuẩn Clean Document chung (Kinh nghiệm BA) | Văn phong trong `document_content` | - Hạn chế văn xuôi dài dòng.<br>- Không lặp từ, viết trực diện để Tech Lead đọc hiểu nhanh. | **-10đ/lỗi** |

3. **Tổng kết Threshold:**
   - Điểm **>= 90đ**: Kết luận PASS (APPROVED).
   - Điểm **< 90đ**: Kết luận FAIL (REJECTED).
4. **Đóng gói Báo cáo (Review Tone):**
   - Bắt buộc chèn các nhận xét "Đóng vai ác" (Ví dụ: *"BA, bạn định cho Tech Lead đọc đống hổ lốn này sao?"*, *"Rác! Hãy giải trình hoặc xóa nó đi."*).
   - Format lại toàn bộ lỗi theo chuẩn form `review_report.md` (truy xuất qua MCP trước đó) để nạp vào biến `review_comment`.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| Dữ liệu vào rỗng | Không tải được file từ hệ thống | Kết luận FAIL lập tức kèm câu chửi: *"Không có tài liệu, bắt tôi review không khí à?"* |
| Không parse được guideline | Guideline trên hệ thống bị lỗi | Dừng lại, báo cáo cho User/Lead để fix guideline trước khi review tiếp. |
