# 1. Giới Thiệu Tổng Quan (Executive Summary)
OrderFlow AI là hệ thống Copilot nhập đơn thông minh dành riêng cho các nhà phân phối ngành nước (ống nhựa, phụ kiện). Thay vì chỉ dừng lại ở việc đọc ảnh bằng AI (OCR) như các giải pháp thông thường, OrderFlow AI giải quyết triệt để bài toán kinh doanh cốt lõi: chuyển đổi ngôn ngữ đặt hàng tự do, phi cấu trúc của thợ thầu thành dữ liệu kỹ thuật chuẩn xác, đồng thời áp dụng các rào chắn tự động về Giá, Tồn kho và Công nợ. Hệ thống không thay thế con người, mà đóng vai trò là lưới lọc thông minh, giúp giải phóng hoàn toàn Sale Admin khỏi việc nhập liệu thủ công, biến họ thành những người kiểm duyệt (Reviewer) nhanh nhạy, chốt đơn trong vòng vài giây.

# 2. Vấn Đề & Thách Thức (Pain Points)
| Mã | Mô tả vấn đề (Hiện trạng đau đớn) | Hậu quả / Ảnh hưởng Kinh doanh (Business Impact) |
|---|---|---|
| P1 | **Ngôn ngữ đặt hàng hỗn loạn:** Khách hàng (thợ/đại lý) nhắn tin bằng tiếng lóng, viết tắt, không chuẩn xác ("co 25", "ống nóng phi 27", "như đơn cũ"). | **Nút thắt cổ chai (Bottleneck) xử lý:** Sale Admin phải tốn hàng giờ mỗi ngày để "dịch" tin nhắn, tra cứu chéo sổ sách và gọi điện hỏi lại khách. Nguy cơ giao sai hàng (nhầm hệ mét/inch, nhầm áp suất PN) gây tổn thất chi phí đổi trả và uy tín. |
| P2 | **Kiểm soát rủi ro phân mảnh:** Giá bán, chính sách chiết khấu, tồn kho khả dụng và hạn mức công nợ nằm rải rác ở nhiều file Excel và hệ thống kế toán rời rạc. | **Rò rỉ lợi nhuận (Profit Leakage) & Nợ xấu:** Dễ dàng duyệt sai giá, cho phép xuất hàng khi khách đã vượt hạn mức tín dụng, hoặc nhận đơn nhưng kho không đủ hàng khiến dự án của khách bị đình trệ. |
| P3 | **Tốc độ phản hồi chậm chạp:** Việc xử lý thủ công khiến quy trình từ lúc nhận tin nhắn đến lúc xuất báo giá / phiếu lấy hàng mất từ 30 phút đến vài giờ. | **Đánh mất cơ hội bán hàng:** Trải nghiệm khách hàng kém, dễ bị đối thủ cạnh tranh cướp khách vì chốt đơn chậm, đồng thời doanh nghiệp không thể mở rộng quy mô (scale up) số lượng đơn hàng mà không phình to quỹ lương nhân sự Sale Admin. |

# 3. Mục Tiêu & Tiêu Chí Đo Lường (Objectives & KPIs)
| Mục tiêu chiến lược | Con số kỳ vọng | Phương pháp / Công cụ đo lường |
|---|---|---|
| Tự động hóa trích xuất và ánh xạ mã sản phẩm (SKU) | > 90% Top-3 SKU Candidate trên tập Test Set | Chạy kiểm thử tự động với tập Seed Data tin nhắn mẫu. |
| Giảm thời gian xử lý một đơn hàng | < 30 giây / đơn (từ khi nhận tin đến Draft Order) | System logs (Time tracking from Intake to Ready_For_Review). |
| Ngăn chặn đơn hàng vi phạm (Giá, Tồn, Công nợ) | 100% cảnh báo đúng (Hold) trên tập Test Set | Kịch bản kiểm thử Audit Trail với các case vượt hạn mức/hết tồn kho. |

# 4. Đối Tượng Thụ Hưởng & Giá Trị Kinh Doanh (Stakeholders & Business Value)
| Đối tượng | Trạng thái Hiện tại (Nỗi đau) | Trạng thái Tương lai (Giá trị mang lại từ OrderFlow AI) |
|---|---|---|
| Khách hàng (Đại lý, Thợ) | Bực mình vì phải chờ báo giá lâu, hoặc bị ép dùng form đặt hàng cứng nhắc. | **Giao dịch tự nhiên:** Vẫn giữ nguyên thói quen nhắn tin đặt hàng tự do qua Zalo, nhưng nhận lại báo giá chốt đơn ngay lập tức. |
| Nhân viên Bán hàng / Sale Admin | Quá tải vì gõ lại đơn tay, căng thẳng vì sợ nhầm lẫn giá và mã hàng. | **Giải phóng sức lao động:** Chuyển từ "Data Entry" sang "Reviewer". Sử dụng Unified Workbench để lướt qua đơn đã được AI chuẩn hóa, tập trung xử lý ngoại lệ và CSKH. |
| Quản lý / Kế toán Công nợ | Phải rà soát thủ công từng đơn, luôn lo lắng việc nợ xấu hoặc thất thoát giá. | **Rào chắn tự động:** Mọi đơn vi phạm đều bị Hold (Credit/Price Hold). Kế toán chỉ cần duyệt các case ngoại lệ trên 1 màn hình duy nhất có lưu vết (Audit Trail) rõ ràng. |
| Thủ kho | Mất thời gian nhặt hàng theo đơn viết tay, thường xuyên phát hiện kho thiếu hàng khi đang nhặt. | **Vận hành chính xác:** Nhận "Phiếu lấy hàng" chuẩn xác 100% dựa trên Tồn kho khả dụng, triệt tiêu hoàn toàn tình trạng "đơn rỗng". |

# 5. Phạm Vi Tổng Quan (High-level Scope)
- **In-Scope:** Tiếp nhận đơn văn bản thô (Text-only); Nhận diện Công trình & Repeat order; Trích xuất & Ánh xạ SKU có độ phức tạp cao (có tính năng Unit Conversion); Khởi tạo đơn nháp; Áp dụng quy tắc Giá/Tồn kho/Công nợ; Giao diện Approval chung (Unified Workbench); Sinh file PDF báo giá/Phiếu lấy hàng; Hệ thống lưu vết (Audit Trail).
- **Out-of-Scope:** Nhận diện và xử lý từ Ảnh/Voice (Hoàn toàn loại bỏ ở MVP); Hệ thống Login/Phân quyền nhiều role phức tạp (Tạm gộp chung UI); Tích hợp trực tiếp ERP thật; Tích hợp API Zalo OA tự động nhắn lại.

# 6. Giải Pháp Công Nghệ & Kiến Trúc (Tech Stack)
| Layer | Công nghệ | Lý do chọn / Ứng dụng |
|---|---|---|
| Frontend | Next.js | Code nhanh, hỗ trợ tốt SSR, làm Review Workbench mượt mà. |
| Backend | Spring Boot | Quản lý logic Pricing, Inventory, Credit Rules vững chắc. Xử lý API. |
| Database | PostgreSQL | Lưu trữ Master data, đơn hàng, fuzzy search và alias matching. |
| AI Integration | OpenAI API (Structured Outputs, Function Calling) | Trích xuất thực thể, đề xuất SKU, tạo câu hỏi làm rõ (Clarification). |

# 7. Lộ Trình Phát Triển (Roadmap / Milestones)
- **Phase 1 (MVP - Hackathon 5h):** Xây dựng luồng Order-entry cơ bản từ Text-only. Seed cứng Master Data (100 SKU, Khách hàng, bảng quy đổi đơn vị). Hoàn thiện Review Workbench (Unified Workbench) hiển thị 3 lớp chốt chặn (Giá, Tồn, Công nợ) với hệ thống lưu vết (Audit Trail) và in PDF Báo giá/Phiếu lấy hàng.
- **Phase 2 (Post-Hackathon):** Tích hợp Zalo OA, bổ sung Vision AI đọc ảnh hóa đơn/ảnh chụp, hoàn thiện UI phân quyền theo role (Sales, Kế toán, Kho).

# 8. Rủi Risk & Biện Pháp Giảm Thiểu (Risks & Mitigations)
| Rủi ro | Hậu quả | Biện pháp phòng ngừa | Biện pháp xử lý |
|---|---|---|---|
| Master Data ngành nước quá phức tạp, AI trích xuất sai | Giao sai hàng, KH phàn nàn | Dùng PostgreSQL Trigram/Alias table thay vì chỉ dùng Vector DB. | Đưa vào trạng thái `NEEDS_CLARIFICATION`, sinh câu hỏi bắt Sales/KH xác nhận lại. |
| Rủi ro tích hợp hệ thống nội bộ (Giả lập) | Chạy lỗi luồng Hold | Chuẩn bị sẵn Seed Data hoàn chỉnh cực sạch trước khi chạy. | Kích hoạt chức năng "Manual Exception Override" để vượt lỗi và tiếp tục trình bày. |
| Code không kịp trong 5 tiếng Hackathon | Bể Demo | Chốt scope hẹp MVP Text-only. | Thiết kế 3 kịch bản Demo script cứng để present mượt mà nhất. |

# 9. Thuật Ngữ & Quy Định Chung (Glossary & Global Rules)
### 9.1 Thuật ngữ cốt lõi (Glossary)
| Thuật ngữ | Tiếng Anh (trong Code) | Ý nghĩa / Giải thích |
|---|---|---|
| Mã SKU | `sku_code` | Mã sản phẩm kỹ thuật duy nhất của NSX (phụ thuộc vào PN, độ dày, phi). |
| Danh pháp lóng | `product_aliases` | Tên thường gọi của thợ (co, cút, nối góc, phi 27). |
| Chờ xác nhận | `NEEDS_CLARIFICATION` | Trạng thái đơn hàng khi AI tìm thấy nhiều hơn 1 SKU hợp lệ và không tự quyết định được. |
| Chặn công nợ | `CREDIT_HOLD` | Trạng thái treo đơn khi Dư nợ dự kiến > Hạn mức cho phép. |
| Chặn tồn kho | `STOCK_HOLD` | Trạng thái treo đơn khi Tồn khả dụng (Available) < Số lượng yêu cầu. |

### 9.2 Ràng buộc nghiệp vụ cốt lõi (Global Business Constraints)
* **Quyền quyết định SKU:** AI tuyệt đối KHÔNG tự quyết định SKU nếu mức độ tin cậy thấp hoặc có nhiều lựa chọn tương đương (phải trả về danh sách Candidate để người dùng/Sales chọn).
* **Kiểm soát xuất kho:** Một đơn hàng chỉ được đẩy xuống kho (Sinh Phiếu lấy hàng) khi và chỉ khi đã pass toàn bộ các trạng thái Hold (Stock, Credit, Price) và được Approve thủ công.
