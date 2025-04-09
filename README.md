# Hướng dẫn sử dụng Raspberry Pi

## Hướng dẫn cấu hình RaspberryPi để làm Server, cài đặt MySQL

* Bước 1: Kiểm tra hệ điều hành và cập nhật repository
    * Cập nhật hệ thống: ```sudo apt update && sudo apt upgrade -y```
* Bước 2: Cài đặt MariaDB:
Trên Raspberry Pi OS, MariaDB thường được sử dụng thay vì MySQL. MariaDB tương thích hoàn toàn với MySQL và có thể thay thế trong hầu hết các trường hợp. Để cài đặt MariaDB ta cấu hình như sau: ```sudo apt install mariadb-server -y```
    * Sau khi cài đặt:
        * Khởi động MariaDB: ```sudo systemctl start mariadb```
        * Đảm bảo nó chạy tự động khi khởi động: ```sudo systemctl enable mariadb```
        * Chạy lệnh bảo mật cơ bản: ```sudo mysql_secure_installation```
            * Lưu ý:
                * Nhập mật khẩu root (nếu chưa có, để trống và nhấn Enter)
                * Làm theo hướng dẫn để thiết lập mật khẩu root và bảo mật cài đặt.
* Bước 3: Kiểm tra cài đặt:
    * Sau khi cài xong, kiểm tra xem MariaDB đã chạy chưa:```sudo systemctl status mariadb```
    * Đăng nhập vào MariaDB để xác nhận: ```sudo mysql -u root -p```
    * Nhập mật khẩu root bạn vừa thiết lập. Nếu vào được shell MySQL/MariaDB, bạn đã cài đặt thành công.
* Bước 4: Cấu hình truy cập từ xa (nếu cần)
Nếu bạn vẫn muốn cho phép truy cập từ xa (như yêu cầu ban đầu), làm theo các bước sau:
    * Chỉnh sửa file cấu hình MariaDB: ```sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf```
        * Tìm dòng: ```bind-address = 127.0.0.1```
        * Thay thành: ```bind-address = 0.0.0.0```
        * Lưu file (Ctrl+O, Enter, Ctrl+X).
    * Khởi động lại MariaDB: ```sudo systemctl restart mariadb```
    * Tạo người dùng từ xa: Đăng nhập vào MariaDB: ```sudo mysql -u root -p```
    * Thực hiện câu lệnh:
        ```
        CREATE USER 'remote_user'@'%' IDENTIFIED BY 'your_password';
        GRANT ALL PRIVILEGES ON *.* TO 'remote_user'@'%' WITH GRANT OPTION;
        FLUSH PRIVILEGES;
        EXIT;
        ```
    * Trong đó:
        * remote_user: Tên tài khoản tạo mới
        * your_password: Mật khẩu tạo mới
        * Câu lệnh này sẽ thực hiện tạo tài khoản (Dòng 1) và cấp toàn quyền ('*') cho tài khoản vừa tạo khi kết nối từ bất kỳ địa chỉ nào (%)
* Bước 5: Kiểm tra quyền tài khoản vừa tạo
    * ```SHOW GRANTS FOR 'remote_user'@'%';```
    * Nếu thành công, bạn sẽ thấy danh sách quyền bao gồm ALL PRIVILEGES.
* Bước 6: Thoát và kiểm tra:
    * ```EXIT;```
    * Thử kết nối từ xa với remote_user.

===================

* Ví dụ: Tạo tài khoản đăng nhập mới trên MariaDB là: User_01 và mật khẩu là user10101
* Đăng nhập vào MariaDB trên Raspberry Pi
![This is an alt text.](https://i.imgur.com/bPcHVKn.png "This is a sample image.")
* Tạo tài khoản đăng nhập và cấp quyền truy cập
![This is an alt text.](https://i.imgur.com/VzZkl1C.png "This is a sample image.")
* Kiểm tra tài khoản vừa tạo
![This is an alt text.](https://i.imgur.com/eUS7RaP.png "This is a sample image.")
* Sử dụng phần mềm MySQL Workbench để kiểm tra đăng nhập (Nếu cần) [Tải phần mềm ở đây](https://dev.mysql.com/get/Downloads/MySQLGUITools/mysql-workbench-community-8.0.41-winx64.msi).
* Đăng nhập vào MySQL Workbench
![This is an alt text.](https://i.imgur.com/sDnXmZw.png "This is a sample image.")
* Kết quả sau khi đăng nhập thành công vào MySQL Workbench

![This is an alt text.](https://i.imgur.com/0KbvOSx.png "This is a sample image.")
