# Biên bản Phỏng vấn và Khơi gợi Yêu cầu (Requirements Elicitation)

**Dự án**: Hệ thống LMS Luyện Thi Đại học
**Thành phần tham dự**: 
- **Ông Trần Văn A (PO)** - Product Owner / Chủ đầu tư
- **Chị Nguyễn Thị B (RE)** - Kỹ sư Yêu cầu (Requirements Engineer)
**Mục tiêu**: Khai thác, phân tích, làm rõ và thống nhất các yêu cầu nghiệp vụ, chức năng và phi chức năng cho hệ thống LMS mới.

---

## Buổi 1: Khởi tạo và Khơi gợi Yêu cầu Tổng quan (Inception & Elicitation)

**RE**: Chào anh A. Rất cảm ơn anh đã dành thời gian cho buổi thảo luận hôm nay. Để bắt đầu quá trình xây dựng hệ thống LMS Luyện Thi Đại học, anh có thể chia sẻ tầm nhìn tổng quan và mục tiêu kinh doanh cốt lõi mà anh kỳ vọng ở nền tảng này không?

**PO**: Chào chị. Mục tiêu của tôi là xây dựng một nền tảng học trực tuyến chất lượng cao chuyên biệt cho học sinh ôn thi đại học. Nơi các giáo viên có thể mở khóa học, đăng tải video bài giảng, tổ chức thi thử. Học sinh sẽ vào học và làm bài. Tuy nhiên, nỗi đau lớn nhất của các nền tảng hiện nay mà tôi muốn giải quyết triệt để là vấn đề thất thoát doanh thu do học sinh chia sẻ tài khoản cho nhau dùng chung, và việc bị đối thủ quay trộm, đánh cắp video bài giảng. Ngoài ra, việc thi thử phải đảm bảo tính nghiêm túc, chống gian lận.

**RE**: Tôi hiểu. Bảo vệ bản quyền và chống chia sẻ tài khoản là những yêu cầu kinh doanh sống còn. Trước khi đi sâu vào các giải pháp kỹ thuật cho vấn đề đó, chúng ta hãy xác định các tác nhân (Actors) sẽ tương tác với hệ thống. Theo anh, nền tảng sẽ có những nhóm người dùng nào?

**PO**: Đầu tiên chắc chắn là Học sinh (Student) và Giáo viên (Teacher). Tiếp theo là đội ngũ nhân viên của tôi (Staff) để hỗ trợ vận hành. Quản trị viên cấp cao (Admin) là tôi. Và cuối cùng là nhóm Khách vãng lai (Guest) - những người chưa đăng ký nhưng vào web để xem thử các bài giảng miễn phí (Free Trial) nhằm mục đích marketing.

**RE**: Rất rõ ràng. Vậy hệ thống sẽ có 5 vai trò (Roles): Admin, Teacher, Staff, Student và Guest. Về phía Staff, nhiệm vụ chính của họ là gì?

**PO**: Staff chủ yếu là để quản lý nội dung thảo luận, trả lời bình luận của học sinh trên diễn đàn (Forum) và kiểm duyệt khung chat (moderate chat) trong các buổi học trực tiếp.

**RE**: Ghi nhận. Tôi sẽ mô hình hóa các vai trò này vào hệ thống phân quyền (RBAC). Bây giờ, chúng ta hãy quay lại vấn đề nhức nhối nhất mà anh đã đề cập: Chia sẻ tài khoản. Anh mong muốn kiểm soát việc này ở mức độ nào?

**PO**: Cực kỳ khắt khe. Một tài khoản học sinh chỉ được phép đăng nhập tối đa trên 2 thiết bị. Ví dụ một điện thoại và một máy tính cá nhân.

**RE**: Việc quản lý theo "Thiết bị" thay vì chỉ "Phiên đăng nhập" (Session) sẽ cần chúng ta thu thập "Dấu vân tay thiết bị" (Device Fingerprint) và địa chỉ IP. Giả sử học sinh mua máy tính mới và đăng nhập, hệ thống nên xử lý thế nào? Cấm luôn hay cho phép họ gỡ thiết bị cũ?

**PO**: Nếu họ đăng nhập ở thiết bị thứ 3, hệ thống phải chặn lại. Họ bắt buộc phải vào quản lý thiết bị, đăng xuất thiết bị cũ. Trong một số trường hợp khả nghi, ví dụ đăng nhập từ 2 vị trí địa lý cách xa nhau trong thời gian ngắn, tôi muốn hệ thống yêu cầu xác thực OTP hoặc tự động vô hiệu hóa phiên đăng nhập đó.

**RE**: Tuyệt vời, đây là một luồng nghiệp vụ rất chặt chẽ. Tôi sẽ ghi chú lại yêu cầu về Device Fingerprint, Location tracking và cơ chế OTP Challenge. Hôm nay chúng ta tạm dừng ở đây, buổi sau chúng ta sẽ đi sâu vào luồng khóa học, bảo mật video và thi thử.

---

## Buổi 2: Phân tích Chi tiết và Làm rõ Nghiệp vụ (Elaboration & Analysis)

**RE**: Chào anh A. Nối tiếp buổi hôm trước, hôm nay chúng ta sẽ bàn về Bảo mật Video và Hệ thống Thi thử (Quiz Engine). Đối với video bài giảng, anh có đề cập đến việc chống quay trộm. Hiện tại không có công nghệ nào trên trình duyệt chống quay màn hình tuyệt đối 100%. Cách tốt nhất là "truy vết" (Watermarking). Anh nghĩ sao về việc chèn thông tin người học vào video?

**PO**: Đúng, tôi đã tham khảo và thấy cách đó rất hay. Tôi muốn một hình mờ (Watermark) động, liên tục hiển thị Họ Tên và Số điện thoại của học sinh đó trôi nổi trên màn hình video đang phát. Nếu nó bị quay lại và phát tán, tôi chỉ cần xem video là biết ngay tài khoản nào làm rò rỉ và khóa vĩnh viễn tài khoản đó.

**RE**: Yêu cầu này hoàn toàn khả thi. Về mặt kỹ thuật, chúng tôi sẽ thiết kế một hệ thống Video Access Token kết hợp với luồng streaming độc lập. Mỗi lần học sinh mở bài, một token dùng một lần sẽ được sinh ra cùng với chuỗi Watermark chứa thông tin của họ. Tiếp theo, về hệ thống Thi thử (Quiz), quy trình tạo và làm bài sẽ diễn ra như thế nào?

**PO**: Giáo viên sẽ tạo một Ngân hàng câu hỏi (Question Bank) theo từng môn học và chuyên đề. Các câu hỏi phải được phân loại rõ ràng theo 4 mức độ của Bộ Giáo dục: Nhận biết, Thông hiểu, Vận dụng, và Vận dụng cao. Mỗi câu hỏi sẽ có nội dung, các đáp án lựa chọn và lời giải chi tiết (explanation).

**RE**: Khi cấu hình đề thi (Quiz), đề sẽ là tĩnh (Static - giáo viên chọn sẵn từng câu) hay lấy ngẫu nhiên từ ngân hàng câu hỏi?

**PO**: Hỗ trợ cả hai, nhưng chủ yếu là lấy ngẫu nhiên. Khi học sinh làm bài, tôi cần hệ thống chống gian lận gắt gao.

**RE**: Anh có thể mô tả cụ thể các hành vi gian lận mà hệ thống cần ngăn chặn không?

**PO**: Học sinh hay mở tab khác để tra Google. Hệ thống phải nhận biết được việc học sinh chuyển tab. Nếu họ chuyển tab (Tab switch count) vượt quá 3 lần, hệ thống sẽ cảnh báo đỏ và lập tức tự động nộp bài (Auto-submit), không cho thi nữa. 

**RE**: Chúng tôi có thể sử dụng Page Visibility API trên trình duyệt để bắt sự kiện này. Còn vấn đề về đường truyền mạng thì sao? Rất nhiều hệ thống LMS gặp lỗi học sinh đang thi thì rớt mạng, lúc có mạng lại thì đồng hồ đếm ngược bị sai hoặc mất bài.

**PO**: Đó cũng là điều tôi lưu tâm. Đồng hồ đếm ngược (Timer) phải cực kỳ chuẩn xác và độc lập với trình duyệt. Dù họ có rớt mạng hay tắt máy tính bật lại, thời gian làm bài vẫn phải trôi đi theo máy chủ (Timer persists on network loss). Khi hết giờ trên máy chủ, trạng thái bài thi tự động chuyển sang đã nộp.

**RE**: Yêu cầu về độ tin cậy rất cao. Chúng tôi sẽ thiết kế bảng QuizAttempt ghi nhận chính xác `started_at` trên máy chủ và sử dụng webhook hoặc cronjob để force-submit. 

---

## Buổi 3: Hệ sinh thái và Tự động hóa Thanh toán (Negotiation & Elaboration)

**RE**: Hôm nay chúng ta sẽ chốt về mảng Giao dịch và các tính năng bổ trợ. Đối với việc thanh toán mua khóa học, quy trình hiện tại anh đang dự tính là gì?

**PO**: Tôi không muốn phải có người ngồi trực duyệt lệnh chuyển khoản thủ công nữa. Phải hoàn toàn tự động. Khi học sinh bấm mua khóa học (PaymentOrder), hệ thống sinh ra một mã QR Code (VietQR) có sẵn số tiền và mã giao dịch (transaction_code). Học sinh quét mã chuyển khoản. Khi tiền vào tài khoản ngân hàng của tôi, hệ thống phải tự động nhận biết, cập nhật trạng thái đơn hàng sang "Completed" và lập tức cấp quyền truy cập khóa học cho học sinh.

**RE**: Để làm được điều này, chúng ta sẽ cần tích hợp Webhook với một cổng thanh toán hoặc dịch vụ thứ ba kết nối với ngân hàng. Hệ thống của chúng ta sẽ mở một API Endpoint (WebhookLog) để lắng nghe biến động số dư. Chúng tôi sẽ thiết kế thêm `idempotency_key` để tránh việc xử lý trùng lặp nếu ngân hàng gọi webhook 2 lần cho cùng 1 giao dịch. 

**PO**: Rất chuyên nghiệp, chị cứ thiết kế sao cho an toàn nhất.

**RE**: Về tính năng Livestream và Diễn đàn (Forum) thì sao?

**PO**: Giáo viên thỉnh thoảng sẽ mở Livestream để chữa đề trực tiếp (LiveSession). Sẽ có một khung Chat bên cạnh. Tôi cần chức năng để Teacher hoặc Staff có thể chặn (Block) những học sinh bình luận khiếm nhã (`blocked_user_id`). Sau buổi live, video sẽ được lưu lại thành một bài học bình thường (VOD). Về Forum, nó sẽ gắn liền với từng bài học để học sinh tiện hỏi đáp, có thể đăng kèm hình ảnh minh họa bài toán.

**RE**: Mọi thứ đã rất rõ ràng. Tôi thấy chúng ta cũng cần một hệ thống Thông báo (Notification Outbox) gửi qua Email hoặc Zalo để báo cho học sinh khi mua khóa học thành công, hoặc khi có phản hồi mới trên diễn đàn.

**PO**: Đúng vậy, thông báo đa kênh sẽ giúp tăng trải nghiệm người dùng rất nhiều.

---

## Buổi 4: Đặc tả, Thống nhất và Xác nhận (Specification & Validation)

**RE**: Dựa trên 3 buổi thảo luận vừa qua, tôi đã tổng hợp lại Danh sách Yêu cầu (Product Backlog) và lập mô hình dữ liệu tĩnh (Class Diagram / ERD). Xin phép tóm tắt lại các Yêu cầu Nghiệp vụ Cốt lõi (Core Business Requirements) để anh xác nhận (Sign-off) trước khi đội phát triển tiến hành code:

1. **Quản lý Định danh & Phân quyền (RBAC)**: Hỗ trợ 5 Roles.
2. **Anti-Sharing Account**: Theo dõi IP, Location, Device Fingerprint. Giới hạn tối đa 2 thiết bị. Hỗ trợ OTP Challenge cho hành vi đáng ngờ.
3. **Bảo mật Video**: Sử dụng Video Access Token sinh ra Dynamic Watermark (Tên + SĐT) trôi nổi trên màn hình.
4. **Hệ thống Thi thử (Quiz Engine)**: 
    - Ngân hàng câu hỏi 4 mức độ: Nhận biết, Thông hiểu, Vận dụng, Vận dụng cao.
    - Chống gian lận: Theo dõi số lần chuyển tab, tự động nộp bài nếu >= 3 lần.
    - Xử lý mất kết nối: Server-side timer, tiếp tục tính giờ ngay cả khi mất mạng.
5. **Thanh toán Tự động (Automated Payment)**: Sinh mã VietQR, lắng nghe Bank Webhook, cấp quyền tự động (xử lý Idempotency).
6. **Livestream & Forum**: Live token, Khung chat có kiểm duyệt (Block user), Diễn đàn hỗ trợ tải ảnh.

**PO**: Bản tóm tắt này bao quát chính xác tất cả những "nỗi đau" và mong muốn của tôi về hệ thống. Các giải pháp kỹ thuật chị đưa ra như Watermark động, Page Visibility API hay Server-side timer rất đúng trọng tâm. 

**RE**: Cảm ơn anh. Tôi sẽ lưu tài liệu này cùng với các bản vẽ UML (Class Diagram, ERD) vào hồ sơ dự án để đội ngũ kỹ sư bắt đầu triển khai kiến trúc nền tảng.

**PO**: Tốt lắm, chúng ta tiến hành thôi! Mọi thứ đã được duyệt.
