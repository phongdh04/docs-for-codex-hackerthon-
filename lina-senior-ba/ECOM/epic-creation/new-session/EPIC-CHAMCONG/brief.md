# Hệ thống Chấm công và Quản lý vận hành nhân sự cho ECOM

## 1. TÊN EPIC (Epic Title)

Xây dựng tính năng Chấm công và Quản lý vận hành nhân sự cho dự án ECOM.

---

## 2. MÔ TẢ EPIC (Epic Statement)

Là một **Nhân viên, HR và Admin** của hệ thống ECOM,
Tôi muốn **sử dụng hệ thống chấm công trực tuyến, đăng ký nghỉ phép/làm bù, tăng ca và quản lý phiếu phạt trực tiếp trên UI**,
Để **ghi nhận chính xác thời gian làm việc thực tế, tối ưu hóa quy trình duyệt phép và nâng cao tính tự giác kỷ luật trong tổ chức**.

---

## 3. MỤC TIÊU KINH DOANH (Business Goals & Metrics)

* **Vấn đề đang gặp phải (Pain-point):**
  * Chưa có hệ thống chấm công trực tuyến chính thức, việc quản lý giờ làm việc và kỷ luật nhân sự (đi muộn/về sớm) còn thủ công.
  * Quy trình xin nghỉ phép, nghỉ bù và tăng ca (OT) chưa được số hóa, dễ gây nhầm lẫn và thiếu minh bạch.
  * Admin/HR tốn nhiều thời gian thao tác DB thủ công khi cần cấu hình hoặc điều chỉnh giờ giấc làm việc của công ty.
* **Giá trị mang lại (Value):**
  * Tự động hóa việc ghi nhận công và phát hiện đi muộn, giúp HR quản lý dễ dàng.
  * Số hóa quy trình duyệt OT, nghỉ phép và nghỉ bù ngay trên giao diện web-client (React + Vite + PWA + TailwindCSS).
  * Admin có thể tự thay đổi cấu hình giờ làm, GPS, IP trực tiếp trên giao diện Admin Settings.
* **Chỉ số đo lường (Metrics/KPIs):**
  * Đạt 100% nhân viên chấm công trực tuyến hàng ngày.
  * Tiết kiệm 80% thời gian tổng hợp công và phiếu phạt của HR vào cuối tháng.
  * Giảm 100% thao tác can thiệp trực tiếp vào Database của Admin khi thay đổi cấu hình giờ làm việc hoặc IP/GPS.

---

## 4. PHẠM VI (Scope & Boundaries)

* ✅ **In-scope (Sẽ bao gồm):**
  * Giao diện chấm công (Check-in/Check-out) trên `web-client` (React + Vite + PWA + TailwindCSS), tự động thu thập IP, GPS, User-Agent.
  * Backend `api-gw` đối chiếu thông tin chấm công với cấu hình động `app_configs` trong MongoDB.
  * Tự động tạo phiếu phạt `punishments` cho trường hợp đi muộn với `amount = null`.
  * Màn hình HR/Admin để duyệt phép, duyệt OT, xem/điều chỉnh/tạo phiếu phạt thủ công (đặc biệt là phạt về sớm).
  * Quản lý nghỉ phép (`break_times`) hỗ trợ nghỉ bù (`has_compensation = true`) với thời gian nghỉ và bù khác nhau, làm cơ sở chấm công bù.
  * Màn hình Admin Settings để cấu hình động giờ làm, tọa độ GPS văn phòng & bán kính, danh sách IP cho phép, ngày làm việc trong tuần.
  * Dashboard lịch sử chấm công cá nhân.
* ❌ **Out-of-scope (Không bao gồm / Để dành cho Phase sau):**
  * Tự động hóa hoàn toàn việc phát hiện về sớm (tạm thời HR đối soát thủ công).
  * Tính năng tính lương tự động từ dữ liệu chấm công.
  * Quản lý nhiều chi nhánh với các cấu hình chấm công khác nhau (chỉ cấu hình một cấu hình văn phòng chung).

---

## 5. TIÊU CHÍ NGHIỆM THU CẤP CAO (High-Level Acceptance Criteria)

* Hệ thống **PHẢI** kiểm tra tính hợp lệ của IP, GPS (lat/long) và User-Agent của nhân viên khi check-in/out so với cấu hình động trong `app_configs`.
* Hệ thống **PHẢI** tự động tạo phiếu phạt `punishments` khi nhân viên đi muộn, với trường số tiền phạt `amount` để trống.
* Hệ thống **PHẢI** cho phép HR/Admin điều chỉnh số tiền phạt và tạo phiếu phạt thủ công trên UI.
* Hệ thống **PHẢI** xác thực thời gian nghỉ phép và thời gian làm bù của nhân viên là khác nhau khi `has_compensation = true`.
* Hệ thống **PHẢI** cho phép Admin cấu hình giờ làm việc chuẩn, IP được phép, tọa độ & bán kính GPS văn phòng, ngày làm việc trên UI Admin Settings và lưu trực tiếp vào MongoDB collection `app_configs`.

---

## 6. RÀNG BUỘC & GIẢ ĐỊNH (Assumptions & Constraints)

* **Giả định (Assumptions):**
  * Thiết bị của nhân viên có hỗ trợ định vị GPS (trình duyệt cấp quyền truy cập vị trí).
  * Hệ thống xác thực người dùng (ID-system) đã hoạt động ổn định và cung cấp đầy đủ thông tin định danh của nhân viên qua JWT token.
* **Ràng buộc (Constraints):**
  * Giao diện Admin Settings và Dashboard phải tuân thủ hệ thống thiết kế "E-FLAME" (Flame Gradient, Montserrat/Inter, Radius Medium 8px).

---

## 7. SỰ PHỤ THUỘC & RỦI RO (Dependencies & Risks)

* **Phụ thuộc (Dependencies):**
  * Cần hệ thống ID-system cung cấp thông tin vai trò người dùng (nhân viên, HR, Admin) để phân quyền truy cập UI và API thích hợp.
* **Rủi Ro (Risks):**
  * Trình duyệt hoặc thiết bị của nhân viên chặn quyền truy cập GPS -> Nhân viên không thể chấm công.
  * *Biện pháp giải quyết:* Cho phép chấm công kèm cảnh báo "Thiếu GPS" và gửi yêu cầu phê duyệt thủ công cho Admin/HR nếu có lý do chính đáng.

---

## 8. DANH SÁCH USER STORY DỰ KIẾN (Child Stories / Breakdown)

* `[ECOM-US-01] - Chấm công Check-in/Check-out`
  * **Story:** Là một Nhân viên, tôi muốn thực hiện Check-in/Check-out hàng ngày trên `web-client`, để hệ thống ghi nhận thời gian làm việc thực tế của tôi dựa trên kiểm tra IP, GPS và thiết bị.
* `[ECOM-US-02] - Tự động ghi nhận đi muộn và Quản lý phiếu phạt (Punishments)`
  * **Story:** Là một HR/Admin, tôi muốn hệ thống tự động tạo phiếu phạt đi muộn (để trống số tiền) và cho phép tôi điều chỉnh/tạo thủ công phiếu phạt trên UI, để kiểm soát kỷ luật thời gian làm việc của nhân sự.
* `[ECOM-US-03] - Đăng ký và phê duyệt Tăng ca (OT)`
  * **Story:** Là một Nhân viên, tôi muốn gửi yêu cầu tăng ca (không bắt buộc có Task) để Admin phê duyệt, để ghi nhận và tính thời gian làm thêm ngoài giờ.
* `[ECOM-US-04] - Đăng ký Nghỉ phép và Nghỉ bù (Break Times)`
  * **Story:** Là một Nhân viên, tôi muốn đăng ký xin nghỉ phép và nghỉ bù (với thời gian nghỉ và làm bù khác nhau) để được Admin phê duyệt, làm cơ sở đối chiếu chấm công làm bù.
* `[ECOM-US-05] - Dashboard Lịch sử chấm công cá nhân`
  * **Story:** Là một Nhân viên, tôi muốn xem lịch sử chấm công, nghỉ phép, OT và phiếu phạt cá nhân dưới dạng lịch/bảng biểu, để tự kiểm tra công làm việc hàng tháng.
* `[ECOM-US-06] - Quản lý cấu hình chấm công dành cho Admin`
  * **Story:** Là một Admin, tôi muốn cấu hình động giờ làm việc, bán kính GPS văn phòng, danh sách IP được phép chấm công và lịch làm việc trực tiếp trên UI Admin Settings, để hệ thống tự động áp dụng các quy tắc chấm công này mà không cần can thiệp DB trực tiếp.
