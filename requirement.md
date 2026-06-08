SOFTWARE REQUIREMENTS SPECIFICATION 

### (SRS) 



**Nền tảng LMS Luyện thi Đại học** **Multi-instructor Learning Management System** 

| Thuộc tính | Chi tiết |
| --- | --- |
| Phiên bản 

 | 1.1 (DRAFT) 

 |
| Trạng thái 

 | Nội bộ — Chưa phát hành 

 |
| Ngày cập nhật 

 | 18/05/2026 

 |
| Bảo mật 

 | CONFIDENTIAL — Chỉ dùng nội bộ 

 |

---

Lịch sử Thay đổi Tài liệu 

| Phiên bản | Ngày | Nội dung thay đổi | Tác giả |
| --- | --- | --- | --- |
| 1.0 

 | 16/05/2026 

 | Phiên bản khởi tạo (Draft) 

 | Nhóm phát triển 

 |
| 1.1 

 | 18/05/2026 

 | Tách role Guest → Guest + Freemium Student; Bỏ role Trợ giảng; Bổ sung FR-00 Xác thực & Quản lý tài khoản 

 | Nhóm phát triển 

 |

---

1. Tổng quan Hệ thống & Mục tiêu 

Hệ thống là một nền tảng học tập trực tuyến quản lý tập trung nhiều giảng viên (Multi-instructor LMS), phục vụ ôn thi đại học quy mô lớn. Hệ thống hướng tới việc tự động hóa khâu vận hành (thanh toán, phân quyền, lưu trữ), giải quyết bài toán bảo mật tài liệu, và đảm bảo khả năng chịu tải Livestream lên tới 20.000 người xem đồng thời. 

1.1 Mục tiêu chiến lược 

* Tự động hóa 100% quy trình thanh toán và kích hoạt tài khoản học sinh. 


* Bảo vệ nội dung độc quyền (video, đề thi) khỏi rò rỉ và sao chép trái phép. 


* Hỗ trợ Livestream quy mô lớn với 20.000 người xem đồng thời (PCU). 


* Cung cấp hệ thống thi thử thông minh với cơ chế chống gian lận. 


* Cung cấp dashboard phân quyền doanh thu riêng biệt giữa Admin và Giảng viên. 


* Đảm bảo luồng chuyển đổi Guest → Freemium → VIP liền mạch, tối đa hóa tỷ lệ nâng cấp. 



1.2 Phạm vi hệ thống 

* Phân hệ 0: Xác thực & Quản lý tài khoản (Authentication & Account Management) 


* Phân hệ 1: Quản lý Khóa học & Bảo mật Video 


* Phân hệ 2: Hệ thống Thi thử thông minh (Quiz Engine) 


* Phân hệ 3: Hạ tầng Livestream quy mô lớn 


* Phân hệ 4: Thanh toán tự động (Automated Payment Funnel) 


* Phân hệ 5: Diễn đàn Hỏi đáp & Thông báo đa kênh 


* Phân hệ 6: Màn hình quản trị (Dashboard) 



1.3 Định nghĩa & Thuật ngữ 

| Thuật ngữ | Định nghĩa |
| --- | --- |
| Guest 

 | Người dùng chưa đăng nhập. Chỉ có thể duyệt trang, xem preview khóa học, đăng ký và đăng nhập. 

 |
| Freemium Student 

 | Học sinh đã đăng ký tài khoản miễn phí. Có thể học thử nội dung được gắn nhãn, làm đề thi Static, xem livestream công khai. 

 |
| VIP Student 

 | Học sinh đã thanh toán và kích hoạt khóa học VIP. Truy cập toàn bộ nội dung và tính năng cao cấp. 

 |
| MFA 

 | Multi-Factor Authentication — Xác thực đa yếu tố. 

 |
| OTP 

 | One-Time Password — Mã xác thực dùng một lần, gửi qua SMS hoặc Email. 

 |
| JWT 

 | JSON Web Token — Token xác thực stateless được ký bằng khóa bí mật. 

 |
| PCU / CCU 

 | Peak Concurrent Users / Current Concurrent Users — Số người dùng đồng thời cao nhất. 

 |
| Static Quiz 

 | Đề thi dạng file cố định (PDF/Text) do giảng viên chỉ định, dành cho Freemium. Không lấy từ ngân hàng đề. 

 |

---

2. Quản lý Người dùng & Ma trận Phân quyền (RBAC) 

Hệ thống phân tách nghiêm ngặt thành 5 nhóm quyền người dùng với phạm vi chức năng và phạm vi hiển thị dữ liệu độc lập nhau. Mô hình chuyển đổi vai trò được thiết kế theo hướng funnel: Guest → Freemium Student → VIP Student. 

| Nhóm quyền (Role) | Phạm vi chức năng | Phạm vi dữ liệu | Xác thực |
| --- | --- | --- | --- |
| Admin (Chủ hệ thống) 

 | Toàn quyền cấu hình hệ thống, duyệt giảng viên, quản trị tất cả khóa học, ngân hàng đề thi, cấu hình phân quyền. 

 | Báo cáo doanh thu tổng toàn hệ thống, toàn bộ chỉ số học tập. 

 | Email + MFA bắt buộc 

 |
| Giảng viên (Teacher) 

 | Tạo và quản lý khóa học, upload video, tạo ngân hàng đề thi thuộc môn học phụ trách. 

 | Chỉ xem doanh thu các khóa học do chính mình đứng tên. 

 | Email + Mật khẩu (MFA khuyến nghị) 

 |
| Học sinh VIP (Student) 

 | Học toàn bộ lộ trình khóa học đã mua, làm bài thi thử từ ngân hàng đề đầy đủ, xem livestream VIP, đăng bài hỏi đáp. 

 | Xem tiến độ học tập và lịch sử thi thử cá nhân. 

 | Email/SĐT + Mật khẩu + OTP khi cần 

 |
| Học sinh Freemium (Freemium Student) 

 | Đăng ký tài khoản miễn phí, học thử các bài được gắn nhãn, làm đề thi giới hạn (Static Quiz), xem livestream công khai, đọc forum. 

 | Xem tiến độ bài học thử. Không xem lộ trình VIP. 

 | Email/SĐT + Mật khẩu 

 |
| Guest (Khách vãng lai) 

 | Xem trang giới thiệu khóa học, preview thông tin công khai, đăng ký tài khoản, đăng nhập. Không truy cập nội dung học. 

 | Không có dữ liệu cá nhân. 

 | Không yêu cầu xác thực 

 |

2.1 Luồng chuyển đổi vai trò (Role Transition Flow) 

Sơ đồ chuyển đổi vai trò theo chiều tuyến tính: 

> 
> **Luồng chuyển đổi:** Guest (Chưa đăng nhập) → [Đăng ký tài khoản] → Freemium Student → [Thanh toán VietQR] → VIP Student 
> 
> 
> 
> **Ngoài ra:** Admin và Giảng viên được tạo trực tiếp bởi Admin — không thể tự đăng ký từ luồng công khai. 
> 
> 

* Guest có thể duyệt trang giới thiệu và preview khóa học mà không cần đăng nhập. 


* Sau khi đăng ký thành công, Guest tự động trở thành Freemium Student. 


* Freemium Student nâng cấp lên VIP Student thông qua luồng thanh toán tự động (FR-04). 


* Admin và Giảng viên chỉ được tạo bởi Admin qua giao diện quản trị — không có luồng tự đăng ký. 



2.2 Bảng phân quyền chi tiết theo chức năng 

| Chức năng | Guest | Freemium | VIP | Giảng viên | Admin |
| --- | --- | --- | --- | --- | --- |
| Xem trang giới thiệu / Preview khóa học 

 | ✔ 

 | ✔ 

 | ✔ 

 | ✔ 

 | ✔ 

 |
| Đăng ký tài khoản 

 | ✔ 

 | — 

 | — 

 | — 

 | — 

 |
| Đăng nhập 

 | ✔ 

 | ✔ 

 | ✔ 

 | ✔ 

 | ✔ 

 |
| Học bài học thử (Freemium content) 

 | — 

 | ✔ 

 | ✔ 

 | ✔ 

 | ✔ 

 |
| Học toàn bộ khóa học VIP 

 | — 

 | — 

 | ✔ 

 | ✔ 

 | ✔ 

 |
| Làm đề thi Static Quiz 

 | — 

 | ✔ 

 | ✔ 

 | ✔ 

 | ✔ 

 |
| Làm đề thi từ Ngân hàng đề 

 | — 

 | — 

 | ✔ 

 | ✔ 

 | ✔ 

 |
| Xem Livestream công khai 

 | ✔* 

 | ✔ 

 | ✔ 

 | ✔ 

 | ✔ 

 |
| Xem Livestream VIP 

 | — 

 | — 

 | ✔ 

 | ✔ 

 | ✔ 

 |
| Đọc Forum hỏi đáp 

 | — 

 | ✔ 

 | ✔ 

 | ✔ 

 | ✔ 

 |
| Đăng câu hỏi Forum 

 | — 

 | — 

 | ✔ 

 | ✔ 

 | ✔ 

 |
| Tạo / quản lý khóa học 

 | — 

 | — 

 | — 

 | ✔ 

 | ✔ 

 |
| Xem doanh thu 

 | — 

 | — 

 | — 

 | Cá nhân 

 | Toàn hệ thống 

 |
| Quản trị toàn bộ hệ thống 

 | — 

 | — 

 | — 

 | — 

 | ✔ 

 |

(*) Guest có thể xem Livestream công khai dạng ẩn danh, không bắt buộc đăng ký. 

---

3. Đặc tả Yêu cầu Chức năng (Functional Requirements) 

FR-00: Xác thực & Quản lý Tài khoản (Authentication & Account Management) 

Phân hệ xác thực là nền tảng bảo mật cho toàn bộ hệ thống, áp dụng cho tất cả các role người dùng. Đây là điểm tương tác đầu tiên của người dùng với hệ thống. 

**FR-00.1 Đăng ký tài khoản (Registration)** 

* Áp dụng cho: Guest → Freemium Student 


* Hệ thống cung cấp form đăng ký với các trường bắt buộc: Họ và tên, Số điện thoại, Email, Mật khẩu, Xác nhận mật khẩu. 


* Hệ thống cung cấp hai phương thức đăng ký: (1) Email + Mật khẩu;  (2) Số điện thoại + Mật khẩu. 


* Hỗ trợ đăng ký nhanh qua OAuth 2.0: Google, Facebook (tùy cấu hình). 


* Tài khoản OAuth được liên kết tự động nếu email trùng. 


* Xác thực email: Hệ thống gửi email chứa link xác thực (có hiệu lực 24 giờ) ngay sau khi đăng ký. 


* Tài khoản chưa xác thực bị giới hạn tính năng. 


* Xác thực số điện thoại: Gửi OTP 6 chữ số qua SMS. OTP có hiệu lực 5 phút. 


* Giới hạn 5 lần gửi lại trong 24 giờ. 


* Ràng buộc mật khẩu: Tối thiểu 8 ký tự, bao gồm ít nhất 1 chữ hoa, 1 chữ số, 1 ký tự đặc biệt. 


* Không được trùng với 3 mật khẩu gần nhất. 


* Kiểm tra trùng lặp: Hệ thống từ chối đăng ký nếu email hoặc SĐT đã tồn tại trong hệ thống. 


* Hiển thị thông báo lỗi cụ thể. 


* Sau khi đăng ký và xác thực thành công, tài khoản được cấp quyền Freemium Student ngay lập tức. 


* Chính sách reCAPTCHA/bot protection: Kích hoạt sau 3 lần thất bại liên tiếp từ cùng IP. 



**FR-00.2 Đăng nhập (Login)** 

* Áp dụng cho: Tất cả role (Freemium, VIP, Giảng viên, Admin) 


* Hệ thống hỗ trợ đăng nhập bằng: (1) Email + Mật khẩu; (2) Số điện thoại + Mật khẩu;  (3) OAuth 2.0 (Google, Facebook). 


* Sau khi xác thực thành công, hệ thống cấp phát: Access Token (JWT, TTL 15 phút) + Refresh Token (Opaque, TTL 7 ngày, HttpOnly Cookie). 


* Refresh Token Rotation: Mỗi lần làm mới Access Token, Refresh Token cũ bị thu hồi và cấp mới. 


* Phát hiện token reuse → vô hiệu hóa toàn bộ session. 


* Ghi nhớ đăng nhập (Remember Me): Kéo dài TTL của Refresh Token lên 30 ngày khi người dùng tích chọn. 


* Giới hạn đăng nhập thất bại: Sau 5 lần nhập sai mật khẩu liên tiếp → khóa tài khoản tạm thời 15 phút và gửi cảnh báo email. 


* Trang đăng nhập của Giảng viên và Admin được tách biệt với trang đăng nhập học sinh (URL riêng, không hiển thị trên giao diện công khai). 


* Hệ thống lưu lịch sử đăng nhập: IP, Trình duyệt/Thiết bị, Thời gian. 


* Người dùng có thể xem và thu hồi session từ trang cài đặt tài khoản. 



**FR-00.3 Quên mật khẩu & Đặt lại mật khẩu (Password Reset)** 

* Người dùng yêu cầu đặt lại mật khẩu bằng email hoặc số điện thoại đã đăng ký. 


* Với email: Gửi link đặt lại mật khẩu (token một lần, hiệu lực 1 giờ). 


* Với SĐT: Gửi OTP 6 chữ số (hiệu lực 5 phút). Giới hạn 3 lần gửi OTP/giờ. 


* Sau khi đặt lại thành công: Thu hồi toàn bộ session đang hoạt động trên tất cả thiết bị. 


* Gửi email thông báo. 


* Hiển thị trang xác nhận thành công và tự động chuyển hướng đến trang đăng nhập. 



**FR-00.4 Đổi mật khẩu (Change Password)** 

* Người dùng đã đăng nhập có thể đổi mật khẩu tại trang cài đặt tài khoản. 


* Bắt buộc nhập mật khẩu hiện tại để xác minh danh tính trước khi đặt mật khẩu mới. 


* Áp dụng cùng ràng buộc chính sách mật khẩu như FR-00.1. 


* Sau khi đổi thành công: Thu hồi tất cả session khác (trừ session hiện tại), gửi email thông báo. 



**FR-00.5 Preview Khóa học (Dành cho Guest)** 

* Áp dụng cho: Guest (chưa đăng nhập) 


* Guest có thể xem trang giới thiệu khóa học bao gồm: Tiêu đề, mô tả, danh sách chương/bài học (không có link), thông tin giảng viên, giá bán, đánh giá tổng hợp. 


* Hiển thị preview video giới thiệu khóa học (≤ 3 phút) mà không cần đăng nhập. 


* Hiển thị số liệu thống kê công khai: Tổng số học viên, tổng số bài học, thời lượng khóa học. 


* Khi Guest nhấn vào bất kỳ nội dung học nào (bài học, đề thi): Hiển thị modal kêu gọi đăng ký/đăng nhập với hai nút rõ ràng: [Đăng ký miễn phí] và [Đăng nhập]. 


* SEO-friendly: Trang preview khóa học được render server-side, có đầy đủ meta tags (og:title, og:description, og:image). 



**FR-00.6 Quản lý Hồ sơ tài khoản (Profile Management)** 

* Người dùng (Freemium trở lên) có thể cập nhật: Họ tên, Ảnh đại diện, Ngày sinh, Giới tính. 


* Thay đổi email hoặc SĐT yêu cầu xác thực OTP tại địa chỉ/số mới. 


* Người dùng có thể xem danh sách thiết bị đang đăng nhập và đăng xuất từ xa bất kỳ thiết bị nào. 


* Admin có thể khóa tài khoản học sinh (kèm lý do) hoặc mở khóa từ Dashboard quản trị. 


* Học sinh bị khóa tài khoản: Nhận thông báo email với lý do và hướng dẫn kháng cáo. 



**FR-00.7 Xác thực đa yếu tố — MFA (Multi-Factor Authentication)** 

* Admin và Giảng viên: MFA bắt buộc (TOTP qua Google Authenticator hoặc Authy). 


* Học sinh VIP: MFA tùy chọn, được khuyến khích kích hoạt từ trang cài đặt bảo mật. 


* Các phương thức MFA hỗ trợ: TOTP (RFC 6238), SMS OTP, Email OTP. 


* Mã dự phòng (Backup Codes): Cấp 10 mã một lần dùng khi thiết lập MFA. 


* Người dùng có thể tạo lại khi cần. 



FR-01: Quản lý Khóa học & Bảo mật Video 

| FR-01.1 Lộ trình học tự do (Flexible Learning) 

 | Độ ưu tiên: Cao 

 | Actor: VIP Student, Freemium Student 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Tự do lựa chọn bài học 

 | Hệ thống không khóa bài học theo tuyến tính. Học sinh (VIP/Freemium) có thể tự do lựa chọn bài học theo nhu cầu cá nhân trong phạm vi quyền hạn của mình. 

 |
| 2 

 | Đánh dấu tiến độ 

 | Hệ thống tự động lưu và hiển thị trạng thái hoàn thành từng bài học (Chưa học / Đang học / Hoàn thành). Học sinh có thể tiếp tục từ điểm dừng. 

 |

| FR-01.2 Cơ chế Học thử (Freemium Access) 

 | Độ ưu tiên: Cao 

 | Actor: Freemium Student, Guest 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Gắn nhãn Freemium 

 | Giảng viên có thể gắn nhãn [Cho phép học thử] cho các bài học/video nhất định khi tạo hoặc chỉnh sửa khóa học. 

 |
| 2 

 | Tự động chặn nội dung VIP 

 | Hệ thống tự động chặn các bài chưa được gắn nhãn đối với tài khoản Freemium Student. 

 |
| 3 

 | Pop-up upsell 

 | Hiển thị pop-up điều hướng mua khóa học khi Freemium Student truy cập bài học bị chặn. Pop-up có nút [Nâng cấp ngay] dẫn thẳng đến luồng thanh toán. 

 |
| 4 

 | Không truy cập với Guest 

 | Guest không thể truy cập bất kỳ nội dung bài học nào, kể cả bài học thử. Hệ thống hiển thị modal đăng ký/đăng nhập. 

 |

| FR-01.3 Bảo mật Video — Mức độ 2 (Level 2 Security) 

 | Độ ưu tiên: Rất cao 

 | Actor: Tất cả 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Chặn chuột phải & DevTools 

 | Chặn hoàn toàn click chuột phải trên trình phát video. Chặn phím tắt F12 (Developer Tools) để ngăn lấy đường dẫn video gốc. 

 |
| 2 

 | Mã hóa luồng phát 

 | Tích hợp mã hóa luồng phát qua API bên thứ ba: Bunny.net hoặc Vimeo (quyết định tại OI-01). Không lưu URL gốc phía client. 

 |
| 3 

 | Dynamic Watermark 

 | Tự động chèn Số điện thoại và Họ tên của học sinh đang đăng nhập, chạy mờ ngẫu nhiên trên màn hình video trong suốt thời gian phát. 

 |

FR-02: Hệ thống Thi thử thông minh (Quiz Engine) 

| FR-02.1 Ngân hàng đề thi (Dành cho VIP Student) 

 | Độ ưu tiên: Cao 

 | Actor: VIP Student, Giảng viên, Admin 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Phân cấp thư mục 

 | Quản lý câu hỏi theo cây thư mục môn học (Môn → Chương → Chuyên đề). 

 |
| 2 

 | Gắn nhãn nhận thức 

 | Gắn Tag theo Chuyên đề và 4 mức độ: Nhận biết, Thông hiểu, Vận dụng, Vận dụng cao (theo chuẩn Bloom). 

 |
| 3 

 | Đảo đề tự động 

 | Tự động đảo thứ tự câu hỏi và thứ tự đáp án khi học sinh bắt đầu làm bài mới. 

 |

| FR-02.2 Đề thi giới hạn — Static Quiz (Dành cho Freemium Student) 

 | Độ ưu tiên: Cao 

 | Actor: Freemium Student 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Đề thi cố định 

 | Freemium Student chỉ được tiếp cận các đề thi cố định (Static Quiz) do giảng viên chỉ định riêng. 

 |
| 2 

 | Định dạng 

 | Các đề này được tải lên trực tiếp dạng file PDF/Text — tuyệt đối không truy xuất hoặc bốc câu hỏi từ Ngân hàng đề gốc. 

 |
| 3 

 | Giới hạn số đề 

 | Số lượng Static Quiz Freemium được làm do Admin cấu hình (xem OI-07). 

 |

| FR-02.3 Luật phòng thi chống gian lận 

 | Độ ưu tiên: Rất cao 

 | Actor: VIP Student, Freemium Student 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Xử lý sự cố mạng 

 | Nếu học sinh mất mạng hoặc bấm F5, đồng hồ đếm ngược vẫn chạy liên tục (server-side timer). Khi kết nối lại, học sinh tiếp tục làm bài trên thời gian còn lại. 

 |
| 2 

 | Chống chuyển Tab 

 | Học sinh chuyển tab quá 3 lần → hệ thống tự động khóa bài và nộp bài tại thời điểm đó. Số lần cảnh báo hiển thị real-time. 

 |
| 3 

 | Kết quả sau nộp bài 

 | Trả điểm, hiển thị bảng lời giải chi tiết và biểu đồ thống kê các dạng kiến thức hay sai ngay sau khi nộp. 

 |

FR-03: Hạ tầng Livestream quy mô lớn 

| FR-03.1 Thông số kỹ thuật Livestream 

 | Độ ưu tiên: Rất cao 

 | Actor: Tất cả 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Giao thức 

 | Low-Latency HLS, độ trễ tối ưu 3–5 giây. 

 |
| 2 

 | Hạ tầng CDN 

 | AWS IVS hoặc Cloudflare Stream (quyết định tại OI-02). 

 |
| 3 

 | Khả năng chịu tải 

 | 20.000 người xem đồng thời (PCU). 

 |

| FR-03.2 Livestream Công khai (Marketing Funnel) 

 | Độ ưu tiên: Cao 

 | Actor: Guest, Freemium Student 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Xem ẩn danh 

 | Guest có thể vào xem ẩn danh, không bắt buộc đăng ký tài khoản. 

 |
| 2 

 | Widget CTA 

 | Tích hợp widget Call-to-Action nổi bật dưới khung video → click chuyển thẳng đến luồng thanh toán khóa VIP hoặc trang đăng ký. 

 |

| FR-03.3 Livestream Nội bộ (Lớp VIP) 

 | Độ ưu tiên: Rất cao 

 | Actor: VIP Student 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Tokenized URL 

 | Bảo mật luồng phát: mỗi học sinh nhận 1 token duy nhất, link hết hạn theo thời gian thực. 

 |
| 2 

 | Dynamic Watermark 

 | Nhúng Dynamic Watermark (hiển thị SĐT/Tên mờ) tương tự video bài giảng thông thường. 

 |

| FR-03.4 Hệ thống Phòng chat (Chat Engine) 

 | Độ ưu tiên: Trung bình 

 | Actor: VIP Student, Freemium Student 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Slow Mode 

 | Kích hoạt Slow mode: giới hạn khoảng thời gian giữa 2 lần nhắn tin từ 5–10 giây/user. 

 |
| 2 

 | Bad-words Filter 

 | Bộ lọc tự động quét và ẩn bình luận chứa từ khóa vi phạm. Danh sách từ khóa do Admin quản lý. 

 |
| 3 

 | Giao diện Moderation 

 | Admin (thay thế Trợ giảng) có quyền xóa bình luận hoặc block tài khoản ngay thời gian thực. 

 |

| FR-03.5 Tự động hóa VOD 

 | Độ ưu tiên: Trung bình 

 | Actor: Admin, Giảng viên 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Lưu trữ tự động 

 | Hệ thống tự động lưu trữ luồng phát trực tiếp sau khi kết thúc buổi live. 

 |
| 2 

 | Render VOD 

 | Tự động render và đóng gói thành file video bài học đưa vào kho khóa học. 

 |

FR-04: Thanh toán tự động (Automated Payment Funnel) 

| FR-04 Thanh toán VietQR tự động 

 | Độ ưu tiên: Rất cao 

 | Actor: Freemium Student 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Tạo QR động 

 | Khi học sinh bấm [Mua khóa học] hoặc CTA tại livestream → hiển thị mã QR động (Dynamic QR) kèm số tiền và nội dung chuyển khoản mã hóa duy nhất (VD: LMS98765). 

 |
| 2 

 | Webhook ngân hàng 

 | Hệ thống nhận tín hiệu Webhook từ ngân hàng đối tác. Xác thực chữ ký HMAC của webhook trước khi xử lý. 

 |
| 3 

 | Kích hoạt tự động 

 | Tự động kích hoạt khóa học / nâng cấp tài khoản Freemium → VIP trong vòng 3 giây sau xác nhận thanh toán. 

 |
| 4 

 | Idempotency 

 | Cơ chế idempotency key đảm bảo không kích hoạt khóa học nhiều lần từ một giao dịch. 

 |
| 5 

 | Thông báo xác nhận 

 | Gửi thông báo xác nhận hiển thị trực tiếp trên màn hình học sinh + Email xác nhận + SMS. 

 |

FR-05: Diễn đàn Hỏi đáp & Thông báo đa kênh 

| FR-05 Forum & Thông báo 

 | Độ ưu tiên: Cao 

 | Actor: VIP Student, Giảng viên, Admin 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Vị trí Forum 

 | Xây dựng mục thảo luận dạng Forum nằm ngay phía dưới mỗi bài học/video. 

 |
| 2 

 | Phân quyền đọc/viết 

 | Freemium Student: chỉ được đọc (read-only). VIP Student: có quyền đăng câu hỏi bằng hình ảnh và văn bản. 

 |
| 3 

 | Thông báo đa kênh 

 | Khi có câu hỏi mới từ VIP → hệ thống tự động gọi API gửi thông báo kèm link trực tiếp về Email, Zalo hoặc Facebook Messenger của Giảng viên phụ trách. 

 |
| 4 

 | Không có role Trợ giảng 

 | Chức năng duyệt và trả lời Forum do Giảng viên phụ trách trực tiếp. Admin có quyền quản trị toàn bộ. 

 |

FR-06: Màn hình Quản trị (Dashboard) 

| FR-06.1 Dashboard Admin 

 | Độ ưu tiên: Cao 

 | Actor: Admin 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Doanh thu tổng 

 | Biểu đồ tổng doanh thu toàn hệ thống (theo ngày/tháng/năm). 

 |
| 2 

 | Tăng trưởng tài khoản 

 | Tốc độ tăng trưởng tài khoản VIP mới, Freemium mới, tỷ lệ chuyển đổi Freemium → VIP. 

 |
| 3 

 | Bảng xếp hạng khóa học 

 | Bảng xếp hạng các khóa học có doanh thu và số học viên cao nhất. 

 |
| 4 

 | Thống kê thi thử 

 | Tổng số lượt học sinh làm bài thi thử trong ngày. 

 |
| 5 

 | Quản lý tài khoản 

 | Tìm kiếm, khóa/mở khóa tài khoản học sinh. Tạo tài khoản Giảng viên mới. 

 |

| FR-06.2 Dashboard Giảng viên 

 | Độ ưu tiên: Trung bình 

 | Actor: Giảng viên 

 |
| --- | --- | --- |
| **STT** | **Yêu cầu** | **Mô tả chi tiết** |
| 1 

 | Dữ liệu cá nhân 

 | Doanh thu cá nhân và các chỉ số học tập liên quan đến môn học do giảng viên đó quản lý (tự động lọc theo role). 

 |
| 2 

 | Quản lý khóa học 

 | Danh sách khóa học đang dạy, tỷ lệ hoàn thành của học sinh, câu hỏi Forum chưa trả lời. 

 |
| 3 

 | Ngân hàng đề thi 

 | Tổng số câu hỏi trong ngân hàng, số đề thi đã tạo, thống kê tỷ lệ đúng/sai theo dạng câu hỏi. 

 |

---

4. Yêu cầu Phi chức năng (Non-Functional Requirements) 

| Nhóm NFR | Chỉ tiêu | Mô tả |
| --- | --- | --- |
| Chống chia sẻ tài khoản 

 | Tối đa 2 thiết bị/tài khoản 

 | Thiết bị thứ 3 đăng nhập → tự động đăng xuất session cũ nhất. Phát hiện IP 2 tỉnh trong 1 giờ → khóa tạm thời + xác minh OTP. 

 |
| Hiệu năng Quiz 

 | 3.000 nộp bài đồng thời 

 | Hệ thống DB đảm bảo xử lý 3.000 học sinh cùng nhấn "Nộp bài" trong 1 giây mà không treo hoặc mất dữ liệu. 

 |
| Thời gian phản hồi 

 | < 1,5 giây 

 | Các tác vụ tải trang thông thường phản hồi dưới 1,5 giây trong điều kiện mạng ổn định. 

 |
| Chịu tải Livestream 

 | 20.000 CCU 

 | Low-Latency HLS + CDN (AWS IVS / Cloudflare Stream) đảm bảo 20.000 người xem đồng thời với độ trễ 3–5 giây. 

 |
| Bảo mật dữ liệu 

 | Mã hóa end-to-end 

 | Mật khẩu lưu dạng bcrypt (cost ≥ 12). Token JWT có TTL ngắn + Refresh Token rotation. HTTPS/TLS 1.3 bắt buộc toàn hệ thống. 

 |
| Khả dụng (Availability) 

 | > 99,5% uptime/tháng 

 | Kiến trúc có failover. Bảo trì theo lịch với thông báo trước 24 giờ. Backup dữ liệu mỗi ngày, lưu 30 ngày. 

 |

4.1 Bảo mật (Security) 

* HTTPS/TLS 1.3 bắt buộc trên toàn bộ luồng dữ liệu. 


* Mật khẩu lưu trữ dạng bcrypt với cost factor ≥ 12 — tuyệt đối không lưu plaintext. 


* Access Token (JWT) TTL tối đa 15 phút. Refresh Token lưu HttpOnly Secure Cookie. 


* Rate limiting: Tất cả endpoint xác thực giới hạn 20 requests/phút/IP. 


* OWASP Top 10: Hệ thống phải vượt qua kiểm tra bảo mật theo chuẩn OWASP Top 10 trước khi go-live. 


* SQL Injection & XSS: Tất cả đầu vào người dùng phải được validate và sanitize trước khi xử lý. 


* Audit Log: Ghi lại toàn bộ hành động nhạy cảm (đăng nhập, đổi mật khẩu, thanh toán) với timestamp và IP. 



4.2 Chống chia sẻ tài khoản (Anti-Account Sharing) 

* Giới hạn tối đa 2 thiết bị đăng nhập đồng thời trên một tài khoản (mặc định: 1 máy tính + 1 điện thoại). 


* Nếu thiết bị thứ 3 đăng nhập → hệ thống tự động đăng xuất session cũ nhất. 


* Phát hiện IP bất thường: 1 tài khoản đăng nhập từ 2 tỉnh/thành khác nhau trong dưới 1 giờ → khóa tạm thời và gửi OTP xác minh. 



4.3 Tuân thủ pháp lý (Compliance) 

* Tuân thủ Nghị định 13/2023/NĐ-CP về bảo vệ dữ liệu cá nhân (PDPD). 


* Người dùng phải đồng ý với Điều khoản dịch vụ và Chính sách bảo mật trước khi hoàn tất đăng ký. 


* Cung cấp chức năng xóa tài khoản (Right to be forgotten) theo yêu cầu của người dùng. 


* Dữ liệu bài thi và kết quả học tập không được mất mát trong bất kỳ trường hợp nào. 


* Luồng thanh toán phải đảm bảo idempotency — tránh kích hoạt khóa học nhiều lần từ 1 giao dịch. 



---

5. Giả định & Ràng buộc 

5.1 Giả định kỹ thuật 

* Hệ thống sử dụng kiến trúc microservices hoặc monolith có khả năng scale ngang (horizontal scaling). 


* Bunny.net hoặc Vimeo sẽ được lựa chọn làm nền tảng mã hóa video — quyết định cuối cùng dựa trên cost analysis. 


* Cổng thanh toán VietQR được tích hợp qua Webhook từ ngân hàng đối tác. 


* Hệ thống email transactional sử dụng provider bên thứ ba (SendGrid, AWS SES hoặc tương đương) đảm bảo tỷ lệ giao thư > 99%. 


* SMS OTP sử dụng nhà cung cấp telco Việt Nam (ESMS, SpeedSMS hoặc tương đương). 



5.2 Ràng buộc 

* Tuân thủ Nghị định 13/2023/NĐ-CP về bảo vệ dữ liệu cá nhân. 


* Dữ liệu bài thi và kết quả học tập phải được lưu trữ an toàn, không mất mát trong bất kỳ trường hợp nào. 


* Luồng thanh toán đảm bảo idempotency — tránh kích hoạt nhiều lần từ 1 giao dịch. 


* Không có role Trợ giảng trong phiên bản 1.1 trở đi. 


* Mọi chức năng trước đây của Trợ giảng được chuyển về Admin và Giảng viên. 



---

6. Vấn đề cần xác nhận thêm (Open Issues) 

Các vấn đề dưới đây cần được thống nhất trước khi bắt đầu thiết kế kỹ thuật chi tiết: 

| Mã | Vấn đề | Nội dung cần xác nhận | Phân hệ liên quan |
| --- | --- | --- | --- |
| OI-01 

 | Nhà cung cấp video 

 | Bunny.net hay Vimeo? (So sánh chi phí, latency, SLA) 

 | Kiến trúc video 

 |
| OI-02 

 | Hạ tầng Livestream CDN 

 | AWS IVS hay Cloudflare Stream? 

 | Hạ tầng 

 |
| OI-03 

 | Cơ chế hoàn tiền 

 | Học sinh yêu cầu hoàn tiền sau khi kích hoạt → thủ công hay tự động? 

 | Thanh toán 

 |
| OI-04 

 | Chính sách lưu trữ VOD 

 | Sau bao nhiêu ngày VOD được xóa hay lưu trữ vĩnh viễn? 

 | Hạ tầng 

 |
| OI-05 

 | Ngưỡng cảnh báo chuyển tab 

 | 3 lần có phải con số cuối cùng không? 

 | Quiz Engine 

 |
| OI-06 

 | Xác thực Social Login 

 | Có hỗ trợ đăng nhập bằng Google/Facebook không? 

 | Xác thực 

 |
| OI-07 

 | Hạn mức Freemium 

 | Giới hạn bao nhiêu bài học thử và bao nhiêu đề thi Static cho Freemium? 

 | Phân quyền 

 |

---

7. Phụ lục 

7.1 Danh sách use case theo role 

| UC-ID | Actor | Use Case |
| --- | --- | --- |
| UC-01 

 | Guest 

 | Xem trang giới thiệu và preview khóa học 

 |
| UC-02 

 | Guest 

 | Đăng ký tài khoản Freemium mới 

 |
| UC-03 

 | Guest / Freemium / VIP 

 | Đăng nhập vào hệ thống 

 |
| UC-04 

 | Freemium / VIP 

 | Đặt lại mật khẩu qua email hoặc SĐT 

 |
| UC-05 

 | Freemium 

 | Học bài học được gắn nhãn Freemium 

 |
| UC-06 

 | Freemium 

 | Làm đề thi Static Quiz 

 |
| UC-07 

 | Freemium 

 | Xem livestream công khai 

 |
| UC-08 

 | Freemium 

 | Nâng cấp lên VIP qua thanh toán VietQR 

 |
| UC-09 

 | VIP Student 

 | Học toàn bộ lộ trình khóa học 

 |
| UC-10 

 | VIP Student 

 | Làm bài thi từ Ngân hàng đề 

 |
| UC-11 

 | VIP Student 

 | Xem livestream VIP với Tokenized URL 

 |
| UC-12 

 | VIP Student 

 | Đăng câu hỏi trên Forum hỏi đáp 

 |
| UC-13 

 | Giảng viên 

 | Tạo và quản lý khóa học, upload video 

 |
| UC-14 

 | Giảng viên 

 | Gắn nhãn bài học Freemium 

 |
| UC-15 

 | Giảng viên 

 | Tạo ngân hàng đề thi và Static Quiz 

 |
| UC-16 

 | Giảng viên 

 | Xem Dashboard cá nhân 

 |
| UC-17 

 | Admin 

 | Quản lý tài khoản (khóa/mở/tạo Giảng viên) 

 |
| UC-18 

 | Admin 

 | Xem Dashboard tổng toàn hệ thống 

 |
| UC-19 

 | Admin 

 | Moderation phòng chat Livestream 

 |
| UC-20 

 | Admin 

 | Cấu hình hệ thống và phân quyền 

 |

7.2 Yêu cầu tích hợp bên thứ ba 

| Dịch vụ | Mục đích | Ghi chú |
| --- | --- | --- |
| Bunny.net / Vimeo 

 | Mã hóa & phát video 

 | Quyết định tại OI-01 

 |
| AWS IVS / Cloudflare Stream 

 | CDN Livestream 

 | Quyết định tại OI-02 

 |
| VietQR / Ngân hàng đối tác 

 | Thanh toán tự động 

 | Tích hợp qua Webhook HMAC 

 |
| SendGrid / AWS SES 

 | Email transactional 

 | OTP, xác thực, thông báo 

 |
| ESMS / SpeedSMS 

 | SMS OTP 

 | Xác thực SĐT, cảnh báo bảo mật 

 |
| Google / Facebook OAuth 2.0 

 | Social Login 

 | Tùy chọn, xem OI-06 

 |
| Zalo API / FB Messenger API 

 | Thông báo đa kênh cho Giảng viên 

 | Thông báo câu hỏi Forum mới 

 |
