---
name: solution-design
description: Đề xuất các phương án giải pháp nghiệp vụ (Options Analysis) và lập bảng danh sách Epic/Story sơ bộ để xin phê duyệt từ User.
---

## Description
Kỹ năng giúp Lina phân tích Pain Points từ yêu cầu thô đã làm rõ, xây dựng tối thiểu 2 phương án giải pháp nghiệp vụ (MVP vs Tối ưu) kèm theo đánh giá ưu/nhược điểm. Sau khi User chọn phương án, Lina lập bảng danh sách Epic/Story sơ bộ quy định các thông tin ranh giới phạm vi để User duyệt trước khi viết tài liệu chi tiết.

## Triggers
- Kích hoạt tại Bước 4 và Bước 5 của quy trình `epic-creation.md`.
- Điều kiện tiên quyết: Yêu cầu thực tế (Business Needs) đã được User xác nhận và chốt làm Baseline ở bước Q&A.

## Inputs
| Tên | Kiểu | Bắt buộc | Mô tả |
|-----|------|----------|-------|
| baseline_requirements | Markdown | Có | Các yêu cầu nghiệp vụ thực tế đã được làm rõ và chốt với User. |
| impact_matrix | Markdown Table | Có | Ma trận đánh giá tác động 4 chiều của tính năng lên hệ thống. |

## Outputs
| Tên | Kiểu | Mô tả |
|-----|------|-------|
| proposed_options | Markdown | Các phương án giải pháp nghiệp vụ đề xuất kèm bảng so sánh ưu/nhược điểm (Vận hành, Kỹ thuật, UX). |
| draft_scope_table | Markdown Table | Bảng danh sách Epic/Story sơ bộ (Name, Statement, Business Goals & Metrics, Scope & Boundaries). |

## Steps
1. **Phân tích Pain Points & Đề xuất phương án (Options Analysis):**
   - Trích xuất Pain Points của người dùng từ `baseline_requirements`.
   - Đề xuất tối thiểu 2 phương án giải quyết:
     - *Phương án MVP:* Đơn giản, giải quyết nhanh, ít ảnh hưởng hệ thống.
     - *Phương án Tối ưu:* Tự động hóa cao, trải nghiệm tốt nhất, nhưng sửa đổi nhiều repo/APIs.
   - Lập bảng so sánh các phương án dựa trên: Tài nguyên phát triển, Trải nghiệm người dùng (UX) và Rủi ro kỹ thuật.
   - Gửi các phương án để User chốt phương án được chọn.
2. **Lập bảng Epic/Story sơ bộ (Draft Scope Table):**
   - Dựa trên phương án giải pháp được User chọn ở bước 1, lập bảng danh sách các Epic và User Stories cần thêm mới hoặc chỉnh sửa.
   - Bảng danh sách bắt buộc phải chứa các cột thông tin sau:
     - **Epic/Story Name:** Tên định danh của Epic/Story.
     - **Statement:** Mô tả tóm tắt hành vi nghiệp vụ mong muốn (As a... I want to... So that...).
     - **Business Goals & Metrics:** Mục tiêu nghiệp vụ và cách đo lường độ thành công.
     - **Scope & Boundaries:** Ranh giới phạm vi cụ thể (In-Scope và Out-of-Scope).
3. **Chờ duyệt và Hiệu chỉnh:**
   - Trình bày bảng danh sách sơ bộ cho User phê duyệt.
   - Nếu User có comment chỉnh sửa, BA hiệu chỉnh trực tiếp bảng danh sách cho đến khi User duyệt hoàn toàn.

## Error Handling
| Tình huống Lỗi | Nguyên nhân | Cách xử lý |
|------|------------|-------------|
| User bác bỏ tất cả các phương án đề xuất | Các phương án chưa đáp ứng đúng Pain Point hoặc chi phí quá cao | Quay lại Bước 2 của workflow để Q&A làm rõ thêm nhu cầu, sau đó tái đề xuất các phương án mới. |
| User comment sửa đổi danh sách Epic/Story | Scope hoặc ranh giới biên chưa phù hợp | Thực hiện cập nhật, bổ sung hoặc gỡ bỏ các dòng tương ứng trong `draft_scope_table` theo đúng feedback của User. |
