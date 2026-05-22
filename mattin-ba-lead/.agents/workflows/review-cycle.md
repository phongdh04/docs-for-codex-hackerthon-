# Workflow: Review Cycle & Reporting
## Description
Quy trình thẩm định và kiểm soát chất lượng tài liệu BA. Mattin sẽ truy xuất tài liệu, chấm điểm một cách khắt khe dựa trên guideline hệ thống và cập nhật lại trạng thái/báo cáo lỗi lên hệ thống.

## Triggers
- **Manual Command:** Người dùng ra lệnh với nội dung *"Hãy review yêu cầu [Mã hiệu yêu cầu] đi"* hoặc các câu mang ý nghĩa tương tự.

## Mermaid Diagram

```mermaid
flowchart TD
  A[Start] --> B[Nhận yêu cầu]
  B --> C{Có dữ liệu?}
  C -->|Không/Lỗi| D[Dừng & Chờ lệnh]
  C -->|Có| E[Thu thập dữ liệu]
  E --> F[Thẩm định chéo]
  F --> G{Chấm điểm}
  G -->|>= 85| H[PASS / APPROVED]
  G -->|< 85| I[FAIL / REJECTED]
  H --> J[Cập nhật hệ thống]
  I --> J
  J --> K[End]
```

## Steps
| # | Bước | Actor | Tool/Action | Output |
|---|------|-------|-------------|--------|
| 1 | Nhận yêu cầu | Mattin | Gọi `get_review_request`. Nếu trả về null hoặc ERROR thì dừng lại đợi mệnh lệnh tiếp theo. | Danh sách tài liệu/yêu cầu cần review |
| 2 | Thu thập dữ liệu | Mattin | Chạy 2 skill: `[../skills/get-document/SKILL.md](../skills/get-document/SKILL.md)` và `[../skills/fetch-guideline/SKILL.md](../skills/fetch-guideline/SKILL.md)` | Nội dung guideline và tài liệu gốc |
| 3 | Thẩm định & Chấm điểm | Mattin | `[../skills/review_doc/SKILL.md](../skills/review_doc/SKILL.md)` | Số điểm cuối cùng và danh sách lỗi |
| 4 | Cập nhật hệ thống | Mattin | Gọi tool `update_review_request` với `status` và `comment`. Nội dung `comment` bám sát chuẩn guideline epic level của file `review_report.md` trên hệ thống. | Cập nhật thành công trạng thái trên hệ thống |
| 5 | Báo cáo Lead | Mattin | Thông báo kết quả ra chat (Pass/Fail) | Hoàn thành lượt review |

## Definition of Done
- [ ] BẮT BUỘC phải gọi `get_review_request` ở đầu quy trình.
- [ ] Sử dụng skill `review_doc/SKILL.md` thay vì `strict-review.md` cũ.
- [ ] Báo cáo (Review Report) truyền vào trường `comment` của `update_review_request` phải đúng chuẩn guideline tại epic level của file `review_report.md` trên hệ thống.
- [ ] TUYỆT ĐỐI KHÔNG ghi file local (như `review_report.md`) ra ổ đĩa.
