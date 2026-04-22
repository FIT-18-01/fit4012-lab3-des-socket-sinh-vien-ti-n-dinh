# Peer Review Response

## Thông tin nhóm
- Thành viên 1: Đỗ Trung Kiên
- Thành viên 2: Nguyễn Việt Cường

## Thành viên 1 góp ý cho thành viên 2
Đỗ Trung Kiên (TV1): Code phần `receiver.py` của Kiên chạy ổn, đọc đúng số byte của Header. Tuy nhiên, ở hàm `decrypt_des_cbc`, nếu bản tin bị lỗi dọc đường dẫn đến padding sai, chương trình sẽ văng lỗi `ValueError` và sập luôn receiver. Cần bắt try-catch ở đoạn này để server không bị chết.

## Thành viên 2 góp ý cho thành viên 1
Nguyễn Việt Cường (TV2): Phần `sender.py` của Kiên làm tốt, đóng gói packet chuẩn. Nhưng ở hàm `get_message()`, nếu người dùng không nhập gì mà nhấn Enter luôn, hệ thống vẫn tạo ra một ciphertext rỗng (chỉ có byte padding). Nên thêm logic bắt buộc người dùng nhập ít nhất 1 ký tự.

## Nhóm đã sửa gì sau góp ý
1. Cập nhật `receiver.py`: Thêm block `try...except ValueError` quanh hàm giải mã, in ra thông báo "Dữ liệu bị toàn vẹn hoặc sai khóa" thay vì crash chương trình.
2. Cập nhật `sender.py`: Thêm vòng lặp `while not plain:` để yêu cầu nhập lại nếu chuỗi rỗng.
3. Chạy lại toàn bộ bộ test CI bằng lệnh `pytest -q` để đảm bảo code sửa không làm hỏng logic cũ.