# Software Requirements Specification (SRS)
# Tính năng: Login (Đăng nhập hệ thống)

| | |
|---|---|
| **Hệ thống** | Perfex CRM – Anh Tester Demo |
| **URL** | https://crm.anhtester.com/admin/authentication |
| **Phiên bản tài liệu** | 1.0 |
| **Ngày tạo** | 2026-10-12 |
| **Tài khoản test tham khảo** | admin@example.com / 123456 |
| **Nguồn tham khảo** | Khảo sát trực tiếp trên hệ thống thực tế |

---

## 1. Giới thiệu (Introduction)

### 1.1 Mục đích (Purpose)
Tài liệu này mô tả các yêu cầu phần mềm (functional & non-functional requirements) cho tính năng **Login** của hệ thống CRM, làm cơ sở cho đội phát triển (Dev) xây dựng và đội kiểm thử (QA) thiết kế test case.

### 1.2 Phạm vi (Scope)
Tài liệu chỉ tập trung vào tính năng **Đăng nhập (Login)** tại trang `/admin/authentication`, bao gồm:
- Xác thực tài khoản bằng Email + Password.
- Xử lý lỗi khi đăng nhập thất bại.
- Chức năng "Remember me".
- Điều hướng (redirect) liên quan tới trạng thái đăng nhập.
- Liên kết tới chức năng "Quên mật khẩu" (không mô tả chi tiết luồng quên mật khẩu).

Không bao gồm: chức năng đăng ký tài khoản (Register), đổi mật khẩu, phân quyền chi tiết sau khi đăng nhập.

### 1.3 Đối tượng sử dụng tài liệu
- Business Analyst (BA)
- Developer
- QA/Tester
- Product Owner

### 1.4 Định nghĩa, từ viết tắt
| Từ viết tắt | Giải thích |
|---|---|
| SRS | Software Requirements Specification |
| CRM | Customer Relationship Management |
| UI | User Interface |
| FR | Functional Requirement |
| NFR | Non-Functional Requirement |

---

## 2. Mô tả tổng quan (Overall Description)

### 2.1 Bối cảnh sản phẩm
Login là điểm vào (entry point) bắt buộc để người dùng truy cập các chức năng quản trị trong hệ thống CRM (Dashboard, Customers, Projects, Invoices...). Người dùng chưa đăng nhập không được phép truy cập bất kỳ trang nội bộ nào trong khu vực `/admin/*`.

### 2.2 Đối tượng người dùng
- **Admin/Staff**: nhân viên nội bộ sử dụng tài khoản được cấp để đăng nhập vào hệ thống quản trị CRM.

### 2.3 Giả định và phụ thuộc (Assumptions & Dependencies)
- Tài khoản người dùng đã được tạo sẵn trong hệ thống (không có chức năng tự đăng ký công khai).
- Hệ thống sử dụng xác thực dựa trên email + password, lưu trạng thái đăng nhập bằng session/cookie.

---

## 3. Yêu cầu chức năng (Functional Requirements)

### FR-01: Hiển thị form Đăng nhập
**Mô tả:** Khi người dùng truy cập URL `/admin/authentication` và chưa đăng nhập, hệ thống hiển thị form Login gồm các thành phần:

| Thành phần | Loại | Bắt buộc | Ghi chú |
|---|---|---|---|
| Logo + tiêu đề "Login" | Hiển thị | - | Logo thương hiệu hệ thống |
| Email Address | Input text (type=email) | Có | Nhận địa chỉ email |
| Password | Input password | Có | Ký tự nhập vào được ẩn (masked) |
| Remember me | Checkbox | Không | Mặc định không được tick |
| Login | Button (submit) | - | Thực hiện xác thực |
| Forgot Password? | Link | - | Điều hướng tới `/admin/authentication/forgot_password` |

### FR-02: Xác thực đăng nhập thành công
**Mô tả:** Khi người dùng nhập đúng Email và Password đã tồn tại trong hệ thống và nhấn nút Login (hoặc Enter):
- Hệ thống xác thực thông tin với dữ liệu tài khoản.
- Nếu hợp lệ: tạo session đăng nhập cho người dùng và chuyển hướng (redirect) tới trang **Dashboard** (`/admin/`).

**Input:** Email hợp lệ đã đăng ký + Password đúng.
**Output:** Redirect `/admin/` (Dashboard), hiển thị thông tin người dùng (avatar) ở góc trên bên phải.

### FR-03: Xử lý khi để trống trường bắt buộc
**Mô tả:** Khi người dùng nhấn Login mà chưa nhập Email và/hoặc Password, hệ thống không thực hiện xác thực mà hiển thị thông báo lỗi tương ứng ngay trên form, không chuyển trang.

| Trường hợp | Thông báo lỗi hiển thị |
|---|---|
| Để trống Email | "The Email Address field is required." |
| Để trống Password | "The Password field is required." |
| Để trống cả hai | Hiển thị đồng thời cả hai thông báo trên |

### FR-04: Xử lý khi đăng nhập sai thông tin
**Mô tả:** Khi Email không tồn tại trong hệ thống, hoặc Email tồn tại nhưng Password không khớp, hệ thống:
- Không xác thực thành công.
- Hiển thị thông báo lỗi chung: **"Invalid email or password"**.
- Không tiết lộ việc email có tồn tại trong hệ thống hay không (để tránh dò user hợp lệ — user enumeration).
- Xoá trắng dữ liệu đã nhập ở 2 trường Email và Password sau khi submit lỗi.
- Không chuyển trang, vẫn ở lại trang Login.

### FR-05: Validate định dạng Email
**Mô tả:** Trường Email Address sử dụng input type="email", hệ thống kiểm tra định dạng email hợp lệ (có ký tự `@`, domain...) trước khi cho phép submit. Nếu định dạng không hợp lệ, trình duyệt chặn submit và hiển thị cảnh báo định dạng.

### FR-06: Chức năng "Remember me"
**Mô tả:** Khi người dùng tick chọn checkbox "Remember me" trước khi đăng nhập thành công, hệ thống lưu trạng thái đăng nhập lâu dài hơn (cookie "remember me"), giúp người dùng không cần đăng nhập lại khi mở lại trình duyệt trong một khoảng thời gian xác định.
Nếu không tick, phiên đăng nhập sẽ kết thúc theo cấu hình session mặc định của hệ thống (ví dụ khi đóng trình duyệt hoặc hết hạn session).

> **Ghi chú:** Thời gian lưu cụ thể của "Remember me" cần xác nhận thêm với đội Dev/BA vì chưa được kiểm chứng trực tiếp.

### FR-07: Điều hướng khi đã đăng nhập
**Mô tả:** Nếu người dùng đã đăng nhập (còn session hợp lệ) mà truy cập lại URL `/admin/authentication`, hệ thống tự động redirect về trang Dashboard (`/admin/`) thay vì hiển thị lại form Login.

### FR-08: Chặn truy cập trực tiếp khi chưa đăng nhập
**Mô tả:** Người dùng chưa đăng nhập (hoặc đã hết session) khi truy cập trực tiếp bất kỳ URL nội bộ nào trong `/admin/*` sẽ bị hệ thống redirect về trang Login.

### FR-09: Đăng xuất (Logout)
**Mô tả:** Người dùng đã đăng nhập có thể đăng xuất thông qua menu avatar (góc trên bên phải) > chọn "Logout". Sau khi logout:
- Session hiện tại bị huỷ.
- Hệ thống redirect về trang Login.
- Truy cập lại các trang nội bộ yêu cầu đăng nhập lại (áp dụng FR-08).

### FR-10: Liên kết "Quên mật khẩu"
**Mô tả:** Trên trang Login có link "Forgot Password?", khi click sẽ điều hướng người dùng tới trang `/admin/authentication/forgot_password` để thực hiện khôi phục mật khẩu (chi tiết luồng này nằm ngoài phạm vi tài liệu).

---

## 4. Yêu cầu phi chức năng (Non-Functional Requirements)

### NFR-01: Bảo mật (Security)
- Password phải được truyền và lưu trữ an toàn (mã hoá/hash), không hiển thị dạng plain text trên UI (input type="password").
- Thông báo lỗi đăng nhập sai phải dùng chung một message ("Invalid email or password") cho cả hai trường hợp sai email và sai password, nhằm tránh lộ thông tin tài khoản nào tồn tại (chống user enumeration).
- Hệ thống cần có cơ chế hạn chế tấn công brute-force (ví dụ: giới hạn số lần đăng nhập sai, captcha, hoặc khoá tạm thời tài khoản) — **cần xác nhận thêm cơ chế thực tế, hiện chưa kiểm chứng được trên hệ thống demo**.
- Hệ thống phải chống chịu được các tấn công cơ bản như SQL Injection, XSS tại các trường nhập liệu Email/Password.
- Các trang nội bộ (`/admin/*`) phải được bảo vệ, chỉ truy cập được khi có session hợp lệ (áp dụng FR-08).

### NFR-02: Hiệu năng (Performance)
- Thời gian phản hồi khi submit form Login (thành công hoặc thất bại) nên dưới 3 giây trong điều kiện mạng bình thường.

### NFR-03: Khả năng sử dụng (Usability)
- Giao diện Login đơn giản, rõ ràng, dễ sử dụng trên cả desktop và mobile (responsive).
- Thông báo lỗi hiển thị rõ ràng, dễ hiểu, vị trí hiển thị ngay phía trên form.

### NFR-04: Khả năng tương thích (Compatibility)
- Trang Login cần hoạt động đúng trên các trình duyệt phổ biến (Chrome, Firefox, Edge, Safari) và các kích thước màn hình khác nhau (desktop, tablet, mobile).

---

## 5. Quy tắc nghiệp vụ (Business Rules)

| Mã | Quy tắc |
|---|---|
| BR-01 | Email và Password là hai trường bắt buộc, không được để trống khi submit. |
| BR-02 | Hệ thống không phân biệt rõ lỗi "sai email" và "sai password" trong thông báo lỗi trả về cho người dùng — chỉ hiển thị "Invalid email or password". |
| BR-03 | Mỗi tài khoản được xác định duy nhất bởi địa chỉ Email. |
| BR-04 | Một người dùng chỉ được xem là "đã đăng nhập" khi có session/cookie hợp lệ được hệ thống xác nhận. |
| BR-05 | Người dùng đã đăng nhập không được phép xem lại form Login (tự động redirect sang Dashboard). |

---

## 6. Luồng xử lý chính (Main Flow / Use Case)

### UC-01: Đăng nhập thành công
1. User truy cập trang Login.
2. User nhập Email và Password hợp lệ.
3. User nhấn nút "Login".
4. Hệ thống xác thực thông tin với cơ sở dữ liệu.
5. Hệ thống tạo session và redirect user tới Dashboard.

**Điều kiện tiên quyết:** User chưa đăng nhập, tài khoản đã tồn tại và đang hoạt động (active).
**Kết quả:** User được chuyển vào Dashboard.

### UC-02: Đăng nhập thất bại (sai thông tin)
1. User truy cập trang Login.
2. User nhập Email và/hoặc Password không chính xác.
3. User nhấn nút "Login".
4. Hệ thống xác thực thất bại.
5. Hệ thống hiển thị thông báo lỗi "Invalid email or password", xoá dữ liệu đã nhập, giữ user ở lại trang Login.

### UC-03: Đăng xuất
1. User đã đăng nhập, click vào avatar góc trên phải.
2. User chọn "Logout".
3. Hệ thống huỷ session hiện tại.
4. Hệ thống redirect user về trang Login.

---

## 7. Ràng buộc và giới hạn (Constraints)

- Tài liệu được xây dựng dựa trên quan sát thực tế hành vi hệ thống demo tại thời điểm khảo sát (2026-10-12), một số quy tắc nghiệp vụ nội bộ (ví dụ: chính sách khoá tài khoản, thời gian sống của session/remember-me, độ dài tối thiểu của password...) **chưa được xác nhận chính thức** và cần làm rõ thêm với đội Dev/BA.
- Không có chức năng tự đăng ký tài khoản công khai trên trang Login.

---

## 8. Phụ lục (Appendix)

### 8.1 Thông tin tài khoản test tham khảo
| Trường | Giá trị |
|---|---|
| Email | admin@example.com |
| Password | 123456 |

### 8.2 Tài liệu liên quan
- [test-cases/login-test-cases.md](../test-cases/login-test-cases.md) — Bộ test case chi tiết cho tính năng Login.
