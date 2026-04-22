# Threat Model - Lab 3

## Thông tin nhóm
- Thành viên 1: Đỗ Trung Kiên
- Thành viên 2: Nguyễn Việt Cường

## Assets
- Dữ liệu gốc truyền tải (Plaintext).
- Thông tin khóa giải mã (DES Key) và Vector khởi tạo (IV).
- Tính khả dụng của tiến trình Receiver.

## Attacker model
Kẻ tấn công (Attacker) có khả năng can thiệp vào mạng lưới nội bộ hoặc chặn bắt đường truyền internet (Mô hình Man-in-the-Middle). Attacker có thể đọc, sửa đổi, hoặc chặn toàn bộ các gói tin TCP đi qua.

## Threats
1. **Lộ lọt dữ liệu (Information Disclosure):** Kẻ tấn công dùng công cụ như Wireshark bắt gói tin, đọc 8 byte đầu tiên lấy được DES Key, 8 byte tiếp theo lấy IV, từ đó dễ dàng giải mã toàn bộ Ciphertext.
2. **Giả mạo dữ liệu (Tampering):** Kẻ tấn công lật (flip) các bit trong phần Ciphertext. Khi Receiver nhận được, hàm giải mã vẫn chạy (hoặc báo lỗi padding) nhưng thông tin gốc đã bị phá hoại hoặc thay đổi ý nghĩa.
3. **Tấn công phát lại (Replay Attack):** Kẻ tấn công bắt một gói tin hợp lệ và gửi lại cho Receiver hàng ngàn lần để làm rối loạn logic hệ thống.

## Mitigations
1. **Trao đổi khóa an toàn:** Tuyệt đối không gửi Key trên cùng một luồng mạng dạng plaintext. Khóa phải được trao đổi bằng giao thức an toàn như Diffie-Hellman hoặc dùng thuật toán mã hóa bất đối xứng (RSA) để mã hóa khóa trước khi gửi.
2. **Xác thực và toàn vẹn (Integrity & Authentication):** Sử dụng Message Authentication Code (ví dụ: HMAC) đính kèm vào cuối gói tin để Receiver xác minh xem dữ liệu có bị chỉnh sửa trên đường truyền hay không.
3. **Chống Replay:** Bổ sung Timestamp hoặc Sequence Number vào bản rõ trước khi mã hóa để từ chối các gói tin cũ.

## Residual risks
Kể cả khi mã hóa chuẩn xác và có HMAC, hệ thống vẫn có thể bị tấn công Từ chối dịch vụ (DoS/DDoS) ở tầng TCP nếu kẻ tấn công liên tục mở các kết nối rác làm cạn kiệt tài nguyên của Receiver.