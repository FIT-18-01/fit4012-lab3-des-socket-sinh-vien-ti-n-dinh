# Report 1 page - Lab 3

## Thông tin nhóm
- Thành viên 1: Đỗ Trung Kiên
- Thành viên 2: ********

## Mục tiêu
Xây dựng và kiểm thử một hệ thống Client-Server đơn giản giao tiếp qua TCP Socket. Dữ liệu truyền đi được bảo vệ bằng mã hoá DES-CBC (có sử dụng IV và Header). Mục tiêu cốt lõi là hiểu được luồng truyền nhận, cách xử lý padding PKCS#7, đồng thời nhận diện được điểm yếu chí mạng khi trao đổi khóa (Key) trên cùng một luồng mạng không an toàn.

## Phân công thực hiện
Đỗ Trung Kiên phụ trách code phía Sender (`sender.py`), xử lý hàm mã hóa, padding và thực hiện các ca kiểm thử giả mạo dữ liệu (tamper). ******** trách code phía Receiver (`receiver.py`), xử lý giải mã, unpad, nhận diện kích thước Header và làm các ca kiểm thử sai khóa (wrong key). Cả hai cùng viết báo cáo và hoàn thiện tài liệu CI.

## Cách làm
Sử dụng thư viện `socket` của Python để giao tiếp TCP. Quá trình mã hóa dùng thư viện `pycryptodome`. Gói tin (Packet) được thiết kế tuần tự: Key (8 byte) + IV (8 byte) + Chiều dài bản mã (4 byte unsigned int) + Ciphertext. Receiver sẽ đọc đúng 20 byte đầu để lấy cấu hình giải mã, sau đó đọc phần còn lại theo chiều dài đã cho.

## Kết quả
Hệ thống chạy ổn định, Sender và Receiver kết nối thành công qua localhost. Dữ liệu được mã hóa và giải mã chính xác. Pass 5/5 test case tự động (local integration, padding, contract, tamper, wrong key). Log chạy thật của từng thành viên đã được lưu đầy đủ vào thư mục `logs/`.

## Kết luận
Về mặt kỹ thuật, nhóm đã nắm vững cách xử lý byte array và socket trong Python. Về mặt bảo mật, lab này minh họa rõ ràng rằng thuật toán mã hóa (DES/AES) dù tốt đến đâu cũng vô dụng nếu quy trình quản lý và trao đổi Khóa (Key Management/Exchange) bị lỗi (gửi key kèm ciphertext).