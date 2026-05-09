# Skill: Blackbox Test Design & Automation Spec

## Description
Kỹ năng thiết kế kịch bản kiểm thử không cần đọc code, tập trung vào việc mô tả các bước tự động hóa cho bộ công cụ Playwright/Appium/Requests.

## Test Scenario Design (Gherkin Style)
Sarah sử dụng định dạng Given-When-Then để mô tả kịch bản:
- **Scenario:** [Tên kịch bản]
- **Given:** [Trạng thái ban đầu]
- **When:** [Hành động của người dùng]
- **Then:** [Kết quả mong đợi]

## Automation Specification Rules
Khi viết `test-automation-spec.md`, Sarah phải chỉ rõ:
1. **Web UI (Playwright):** 
   - Selector type (CSS/XPath/Text).
   - Action (Click/Fill/Wait).
   - Assertion (Element visible/Text match).
2. **Mobile (Appium):**
   - Accessibility ID / Resource ID.
   - Device capabilities.
3. **API (Requests):**
   - Method (GET/POST/PUT/DELETE).
   - Request Body & Headers.
   - Expected Status Code & JSON Schema.

## Bug Reporting Standard
Mỗi bug phải có:
- **ID:** STORY_ID_BUG_XX
- **Title:** Tóm tắt lỗi.
- **Severity:** [Critical / High / Medium / Low].
- **Steps to Reproduce:** Các bước cực kỳ chi tiết để tái hiện.
- **Expected:** Kết quả đúng theo Spec.
- **Actual:** Kết quả sai thực tế.
