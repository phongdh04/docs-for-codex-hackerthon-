---
name: build_guideline
description: Xây dựng tài liệu guideline theo chuẩn 5 tiểu mục đệ quy (Recursive Meta-Guideline Standard).
---

# 1. Hướng dẫn sử dụng Skill

Kích hoạt skill này khi cần viết, tạo mới hoặc bổ sung một tài liệu guideline trong hệ thống. Skill này đảm bảo rằng mỗi phần của guideline được tạo ra sẽ trở thành "Executable Logic" có tính nhất quán cao, cung cấp ngữ cảnh đầy đủ cho AI và Con người.

# 2. Quy trình thực hiện (Cách viết)

Khi sử dụng skill này, bạn cần tuân thủ cấu trúc sau:

1. **Định hình các đầu mục chính**: Dựa vào thông tin thu thập được từ bước Q&A và rà soát hệ thống, liệt kê ra danh sách các đầu mục cần thiết cho guideline (đánh số 1, 2, 3... n).
2. **Triển khai 5 tiểu mục bắt buộc**: Trong TỪNG ĐẦU MỤC đã liệt kê ở trên, bạn bắt buộc phải tạo 5 tiểu mục con sau:
   - **Mô tả**: Trình bày rõ ràng mục này là gì, tại sao nó lại quan trọng trong ngữ cảnh chung.
   - **Cách viết**: Các bước hướng dẫn cụ thể để thực hiện công việc trong mục này. (Chỉ rõ "Làm như thế nào").
   - **Nguồn thông tin**: Định nghĩa rõ thông tin đầu vào cần lấy từ đâu (tên file, cơ sở dữ liệu, api, con người).
   - **Cách thu thập**: Phương pháp, công cụ hoặc câu lệnh để lấy thông tin từ các nguồn trên.
   - **Format gợi ý / Template áp dụng**: Đưa ra một khung mẫu dạng Markdown Code Block cụ thể để người hoặc AI khác có thể dễ dàng copy và điền thông tin vào.

# 3. Yêu cầu đầu ra (Format gợi ý)

Khi bạn viết xong guideline, cấu trúc tài liệu bắt buộc phải trông giống như mẫu dưới đây:

```markdown
# [Tên Guideline]

## 1. [Tên đầu mục 1]

### Mô tả
[Giải thích...]

### Cách viết
[Hướng dẫn từng bước...]

### Nguồn thông tin
[Dữ liệu lấy từ đâu...]

### Cách thu thập
[Phương pháp lấy dữ liệu...]

### Format gợi ý / Template áp dụng
\`\`\`
[Template...]
\`\`\`

## 2. [Tên đầu mục 2]
...
(lặp lại cấu trúc 5 tiểu mục cho mọi đầu mục)
```

# 4. Quy tắc bắt buộc

## RECURSIVE META-GUIDELINE STANDARD
Mỗi tài liệu guideline phải bao gồm các đầu mục đánh số từ 1 đến n. TỪNG ĐẦU MỤC bắt buộc có 5 tiểu mục:
1. **Mô tả:** Giải thích rõ mục này là gì, tại sao quan trọng.
2. **Cách viết:** Hướng dẫn chi tiết các bước.
3. **Nguồn thông tin:** Nguồn dữ liệu/thông tin cần thiết.
4. **Cách thu thập:** Phương pháp cụ thể để lấy thông tin.
5. **Format gợi ý / Template áp dụng:** Khung mẫu (Markdown code block) để copy/điền.

## HIERARCHY & FORMATTING
- Số thứ tự lớn (1., 2., 3...) cho đầu mục chính.
- Định dạng Bold hoặc Heading nhỏ hơn cho 5 tiểu mục chuẩn để phân cấp.
- Sử dụng `mermaid` (flowchart) cho quy trình phức tạp.
- Sử dụng bảng Markdown cho thuật ngữ/so sánh.

## Lưu ý khác
- Không bao giờ được phép thiếu 1 trong 5 tiểu mục chuẩn trong bất cứ đầu mục chính nào.
- Nội dung ở mục **Format gợi ý** phải có tính khả thi và rõ ràng, chỉ tập trung vào "Template Result".
