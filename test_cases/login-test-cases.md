# Test Cases - Tính năng Login (Đăng nhập)

**Ứng dụng:** Perfex CRM – Anh Tester Demo
**URL:** https://crm.anhtester.com/admin/authentication
**Tài khoản test:** admin@example.com / 123456
**Ngày tạo:** 2026-10-11

## Ghi chú khảo sát màn hình Login

- Form gồm 2 trường: `Email Address` (type=email, bắt buộc), `Password` (type=password, bắt buộc), checkbox `Remember me`, nút `Login`, link `Forgot Password?`.
- Validate rỗng: hiển thị "The Email Address field is required." và "The Password field is required." (hiển thị cùng lúc nếu cả hai trống).
- Sai email/password: hiển thị thông báo chung "Invalid email or password" (không tiết lộ email có tồn tại hay không), đồng thời cả 2 trường bị xoá trắng.
- Đăng nhập đúng: chuyển hướng tới Dashboard (`/admin/`).
- Nếu user đã đăng nhập mà truy cập lại `/admin/authentication` thì tự động redirect về Dashboard.
- Logout (qua menu avatar góc phải > Logout) đưa user quay lại trang Login.

## Danh sách Test Case

| ID | Tiêu đề | Mức ưu tiên | Tiền điều kiện | Các bước thực hiện | Dữ liệu test | Kết quả mong đợi |
|---|---|---|---|---|---|---|
| TC_LOGIN_01 | Đăng nhập thành công với email và password hợp lệ | High | User chưa đăng nhập, đang ở trang Login | 1. Truy cập https://crm.anhtester.com/admin/authentication<br>2. Nhập Email Address<br>3. Nhập Password<br>4. Click nút Login | Email: `admin@example.com`<br>Password: `123456` | Đăng nhập thành công, chuyển hướng tới trang Dashboard (`/admin/`), hiển thị menu, avatar user góc trên phải |
| TC_LOGIN_02 | Đăng nhập thất bại khi để trống cả Email và Password | High | User chưa đăng nhập, đang ở trang Login | 1. Để trống Email và Password<br>2. Click nút Login | Email: (trống)<br>Password: (trống) | Hiển thị thông báo lỗi "The Email Address field is required." và "The Password field is required." ngay trên form. Không chuyển trang |
| TC_LOGIN_03 | Đăng nhập thất bại khi để trống Email | Medium | User chưa đăng nhập, đang ở trang Login | 1. Để trống Email<br>2. Nhập Password hợp lệ<br>3. Click Login | Email: (trống)<br>Password: `123456` | Hiển thị thông báo "The Email Address field is required.". Không chuyển trang |
| TC_LOGIN_04 | Đăng nhập thất bại khi để trống Password | Medium | User chưa đăng nhập, đang ở trang Login | 1. Nhập Email hợp lệ<br>2. Để trống Password<br>3. Click Login | Email: `admin@example.com`<br>Password: (trống) | Hiển thị thông báo "The Password field is required.". Không chuyển trang |
| TC_LOGIN_05 | Đăng nhập thất bại với sai Password | High | User chưa đăng nhập, đang ở trang Login | 1. Nhập đúng Email<br>2. Nhập sai Password<br>3. Click Login | Email: `admin@example.com`<br>Password: `wrongpass123` | Hiển thị thông báo lỗi chung "Invalid email or password". Các trường nhập bị xoá trắng. Không chuyển trang |
| TC_LOGIN_06 | Đăng nhập thất bại với Email không tồn tại | High | User chưa đăng nhập, đang ở trang Login | 1. Nhập Email không tồn tại trong hệ thống<br>2. Nhập Password bất kỳ<br>3. Click Login | Email: `wrong@example.com`<br>Password: `wrongpass123` | Hiển thị thông báo lỗi chung "Invalid email or password" (không tiết lộ email tồn tại hay không). Không chuyển trang |
| TC_LOGIN_07 | Đăng nhập thất bại với Email sai định dạng | Medium | User chưa đăng nhập, đang ở trang Login | 1. Nhập Email sai định dạng (không có @)<br>2. Nhập Password bất kỳ<br>3. Click Login | Email: `notanemail`<br>Password: `123456` | Trình duyệt chặn submit do input type="email" (hiển thị cảnh báo định dạng email của trình duyệt), hoặc hệ thống trả lỗi định dạng email. Không chuyển trang |
| TC_LOGIN_08 | Kiểm tra trường Password được ẩn ký tự | Medium | User chưa đăng nhập, đang ở trang Login | 1. Click vào trường Password<br>2. Nhập password | Password: `123456` | Các ký tự nhập vào được hiển thị dạng ẩn (dấu chấm/asterisk), không hiển thị plain text |
| TC_LOGIN_09 | Đăng nhập với Email có khoảng trắng đầu/cuối | Low | User chưa đăng nhập, đang ở trang Login | 1. Nhập Email có khoảng trắng đầu/cuối<br>2. Nhập đúng Password<br>3. Click Login | Email: `  admin@example.com  `<br>Password: `123456` | Hệ thống tự động trim khoảng trắng và đăng nhập thành công, hoặc báo lỗi rõ ràng nếu không hỗ trợ trim |
| TC_LOGIN_10 | Đăng nhập với Email/Password phân biệt hoa-thường | Low | User chưa đăng nhập, đang ở trang Login | 1. Nhập Email viết hoa toàn bộ<br>2. Nhập đúng Password<br>3. Click Login | Email: `ADMIN@EXAMPLE.COM`<br>Password: `123456` | Email thường không phân biệt hoa thường → đăng nhập thành công. Nếu không thành công, cần xác nhận lại với team business xem đây có phải hành vi mong muốn |
| TC_LOGIN_11 | Đăng nhập thất bại khi Password phân biệt hoa-thường | Medium | User chưa đăng nhập, đang ở trang Login | 1. Nhập đúng Email<br>2. Nhập Password đúng nhưng sai hoa/thường<br>3. Click Login | Email: `admin@example.com`<br>Password: `123456 ` (hoặc biến thể hoa/thường nếu password có chữ) | Đăng nhập thất bại (password phân biệt hoa/thường), hiển thị "Invalid email or password" |
| TC_LOGIN_12 | Chức năng "Remember me" | Medium | User chưa đăng nhập, đang ở trang Login | 1. Nhập đúng Email/Password<br>2. Tick checkbox "Remember me"<br>3. Click Login<br>4. Đóng trình duyệt và mở lại, truy cập lại trang | Email: `admin@example.com`<br>Password: `123456` | Sau khi đóng/mở lại trình duyệt, user vẫn ở trạng thái đăng nhập (session được lưu bằng cookie "remember me") |
| TC_LOGIN_13 | Không tick "Remember me" thì session hết khi đóng trình duyệt | Low | User chưa đăng nhập, đang ở trang Login | 1. Nhập đúng Email/Password<br>2. Không tick "Remember me"<br>3. Click Login<br>4. Đóng hoàn toàn trình duyệt (xoá session) và mở lại | Email: `admin@example.com`<br>Password: `123456` | Session hết hạn theo cấu hình hệ thống (tuỳ thuộc server-side session timeout) |
| TC_LOGIN_14 | Truy cập lại trang Login khi đã đăng nhập | Medium | User đã đăng nhập thành công | 1. Sau khi đăng nhập thành công<br>2. Truy cập trực tiếp lại URL `/admin/authentication` | - | Hệ thống tự động redirect về trang Dashboard (`/admin/`), không hiển thị lại form Login |
| TC_LOGIN_15 | Đăng xuất (Logout) sau khi đăng nhập | High | User đã đăng nhập thành công | 1. Click avatar user ở góc trên phải<br>2. Click "Logout" | - | User được đăng xuất, redirect về trang Login. Truy cập lại các trang trong `/admin/` yêu cầu đăng nhập lại |
| TC_LOGIN_16 | Truy cập trực tiếp trang nội bộ khi chưa đăng nhập | High | User chưa đăng nhập (hoặc đã logout) | 1. Truy cập trực tiếp URL một trang nội bộ, ví dụ `/admin/` hoặc `/admin/customers` | - | Hệ thống redirect về trang Login, không cho truy cập trực tiếp |
| TC_LOGIN_17 | Kiểm tra link "Forgot Password?" | Medium | User chưa đăng nhập, đang ở trang Login | 1. Click vào link "Forgot Password?" | - | Chuyển hướng tới trang `/admin/authentication/forgot_password` để khôi phục mật khẩu |
| TC_LOGIN_18 | Kiểm tra giới hạn số lần đăng nhập sai (Brute-force / Account lockout) | High | User chưa đăng nhập, đang ở trang Login | 1. Nhập đúng Email, sai Password nhiều lần liên tiếp (ví dụ 5 lần)<br>2. Thử đăng nhập lại với Password đúng | Email: `admin@example.com`<br>Password: sai nhiều lần, sau đó `123456` | Hệ thống khoá tài khoản/tạm khoá đăng nhập hoặc yêu cầu captcha sau N lần sai (cần xác nhận cơ chế thực tế); nếu không có cơ chế này, đây là rủi ro bảo mật cần báo cáo |
| TC_LOGIN_19 | Kiểm tra SQL Injection tại trường Email/Password | High | User chưa đăng nhập, đang ở trang Login | 1. Nhập chuỗi SQL injection vào Email và/hoặc Password<br>2. Click Login | Email: `' OR '1'='1`<br>Password: `' OR '1'='1` | Đăng nhập thất bại, hệ thống không bị lỗi, không có truy vấn SQL bị khai thác, hiển thị lỗi thông thường "Invalid email or password" |
| TC_LOGIN_20 | Kiểm tra XSS tại trường Email/Password | High | User chưa đăng nhập, đang ở trang Login | 1. Nhập đoạn script vào trường Email<br>2. Click Login | Email: `<script>alert(1)</script>`<br>Password: `123456` | Hệ thống không thực thi script, hiển thị lỗi định dạng/đăng nhập thất bại bình thường, dữ liệu được escape khi hiển thị lại |
| TC_LOGIN_21 | Kiểm tra nút Login bằng phím Enter | Low | User chưa đăng nhập, đang ở trang Login | 1. Nhập đúng Email/Password<br>2. Nhấn phím Enter thay vì click nút Login | Email: `admin@example.com`<br>Password: `123456` | Form được submit tương tự như click nút Login, đăng nhập thành công |
| TC_LOGIN_22 | Kiểm tra giao diện trang Login hiển thị đầy đủ | Low | Không cần | 1. Truy cập trang Login | - | Hiển thị đầy đủ logo, tiêu đề "Login", trường Email Address, Password, checkbox Remember me, nút Login, link Forgot Password?. Giao diện không bị vỡ layout |
| TC_LOGIN_23 | Kiểm tra trang Login responsive trên mobile/tablet | Low | Không cần | 1. Truy cập trang Login trên các kích thước màn hình khác nhau (mobile, tablet, desktop) | - | Giao diện hiển thị đúng, không bị vỡ layout, các trường nhập/nút bấm vẫn thao tác được bình thường |

## Ghi chú

- Các test case TC_LOGIN_18, TC_LOGIN_19, TC_LOGIN_20 là các test case bảo mật (security testing), cần thực hiện cẩn trọng và chỉ trên môi trường được phép test.
- TC_LOGIN_10, TC_LOGIN_12, TC_LOGIN_13, TC_LOGIN_18 cần xác nhận thêm hành vi thực tế của hệ thống vì chưa được kiểm chứng trực tiếp trong quá trình khảo sát.
