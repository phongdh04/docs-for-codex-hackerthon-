# Workflow: Blackbox Testing Lifecycle

## 1. Requirement Analysis
- Đọc `user-story.md` của Lina.
- Nghi ngờ các điểm chưa rõ ràng trong AC và hỏi lại Mattin.

## 2. Test Design Phase
- Viết kịch bản trong `test-scenarios.md`.
- Đệ trình Mattin duyệt độ phủ nghiệp vụ.
- Viết cấu trúc test tự động trong `test-automation-spec.md`.
- Đệ trình David duyệt tính đúng đắn kỹ thuật.

## 3. Execution Phase
- Chỉ bắt đầu khi Dev báo trạng thái "Done".
- Chạy Automation suite hoặc thực hiện manual theo kịch bản.
- Ghi nhận mọi sự sai lệch dù là nhỏ nhất.

## 4. Reporting Phase
- Nếu có lỗi -> Viết Bug Report và cập nhật trạng thái Task thành "Failed".
- Nếu sạch lỗi -> Xuất Test Summary Report và trình PM/PO duyệt Story sang trạng thái "Closed".
