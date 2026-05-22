# HƯỚNG DẪN THIẾT KẾ VÀ VIẾT KỸ NĂNG CHO AI AGENT (SKILL_STANDARD)

> **Mục tiêu:** Tiêu chuẩn hóa cách định nghĩa các kỹ năng (Skills) của Agent, đảm bảo tính nguyên tử (Atomic), tiết kiệm Token (Token Efficiency) và dễ dàng lắp ghép vào các Workflows khác nhau.

---

## 1. NGUYÊN TẮC CỐT LÕI (ATOMIC SKILLS)

Kỹ năng của AI Agent phải tuân thủ **Nguyên tắc Đơn nhiệm (Single Responsibility Principle)**:
- Mỗi file `SKILL.md` chỉ giải quyết **một nhiệm vụ duy nhất** (Ví dụ: `fetch-guideline`, `write-epic-specs`, `request-review`).
- Nếu một kỹ năng trở nên quá phức tạp hoặc có nhiều nhánh điều kiện rườm rà, nó nên được chuyển thành một Workflow (luồng công việc) chứa nhiều Skills nhỏ hơn.
- Không gộp chung nhiều công cụ không liên quan vào cùng một file Skill.

---

## 2. CẤU TRÚC BẮT BUỘC (THE BLUEPRINT)

Mọi file `SKILL.md` phải tuân thủ chính xác template Markdown sau đây. **Tuyệt đối không được bỏ sót bất kỳ một section nào.**

```markdown
---
name: <kebab-case-skill-name>
description: <Mô tả ngắn gọn trong 1 câu>
---

## Description
Mô tả ngắn gọn, súc tích về mục đích của kỹ năng và giá trị nghiệp vụ mà nó giải quyết.

## Triggers
- Khi nào Agent nên kích hoạt kỹ năng này?
- Điều kiện tiên quyết để chạy (ví dụ: cần có file XYZ, hoặc sau khi nhận được yêu cầu).

## Inputs
| Tên | Kiểu dữ liệu | Bắt buộc | Mô tả chi tiết |
|-----|--------------|----------|----------------|
| ... | ...          | Có/Không | ...            |

## Outputs
| Tên | Kiểu dữ liệu | Mô tả chi tiết |
|-----|--------------|----------------|
| ... | ...          | ...            |

## Steps
1. **Bước 1:** Thực hiện hành động A.
2. **Bước 2:** Thực hiện hành động B (sử dụng công cụ X).
3. **Bước 3:** Đóng gói kết quả C.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý (Self-healing) |
|----------------|-------------|---------------------------|
| ...            | ...         | ...                       |
```

---

## 3. HƯỚNG DẪN CHI TIẾT TỪNG PHẦN

### 3.1 Metadata (Frontmatter)
- Dùng cú pháp YAML (kẹp giữa 2 dòng `---`) ở đầu file.
- **name:** Bắt buộc viết thường, dùng dấu gạch ngang (kebab-case), không chứa dấu cách (VD: `requirement-analysis`).
- **description:** Một câu duy nhất mô tả trọng tâm hành động.

### 3.2 Inputs & Outputs (I/O Matrix)
- **Bắt buộc dùng Bảng (Markdown Table).** Việc dùng bảng giúp mô hình ngôn ngữ lớn (LLM) phân tách các tham số cực kỳ chính xác.
- Ở cột `Kiểu dữ liệu`, hãy quy định rõ định dạng (ví dụ: `String`, `JSON`, `Markdown`, `Boolean`, `List`).
- Bảng Output là cam kết về đầu ra mà Skill này sẽ bàn giao cho bước (hoặc skill) tiếp theo trong Workflow.

### 3.3 Steps (Các bước thực thi)
- Trình bày dạng danh sách có đánh số thứ tự (Numbered List).
- Dùng động từ mạnh để ra lệnh (Ví dụ: "Truy xuất", "Tổng hợp", "Viết", "Đóng gói").
- **Ghi nhớ:** Phải chỉ đích danh MCP tool hoặc phương thức nào được dùng ở từng bước nếu có.

### 3.4 Error Handling (Xử lý Ngoại lệ)
AI Agent tự động hóa rất dễ bị kẹt vòng lặp (loop) nếu gặp lỗi. Bảng Error Handling cung cấp "Lối thoát" (Escape Hatch):
- Nếu API Timeout $\rightarrow$ Hướng dẫn Retry bao nhiêu lần.
- Nếu Input bị thiếu $\rightarrow$ Hướng dẫn dùng kỹ năng hỏi Q&A hoặc báo lỗi.
- Nếu dữ liệu rác (Invalid data) $\rightarrow$ Hướng dẫn ghi log vào file `out_scope_thinking.md` và dừng lại.

---

## 4. QUY TẮC TỐI ƯU TOKEN (TOKEN EFFICIENCY)

- **Viết kỹ thuật, không viết văn xuôi:** File SKILL được máy (LLM) đọc chứ không phải con người đọc. Hãy bỏ các câu rườm rà như *"Kỹ năng này rất hữu ích trong việc giúp bạn..."*. Thay vào đó hãy viết trực tiếp: *"Kỹ năng thực hiện chức năng X."*
- **Sử dụng Bullet Points & Bold Text:** Giúp nhấn mạnh những chỉ thị cốt lõi (Guardrails).
- **Tránh nhúng Code quá dài:** Trừ trường hợp file mẫu cực kỳ ngắn, còn lại không nên nhúng cả một template tài liệu vài trăm dòng vào trong `SKILL.md`. Hãy dùng phương pháp lấy dữ liệu thông qua MCP (như `fetch-guideline`).
