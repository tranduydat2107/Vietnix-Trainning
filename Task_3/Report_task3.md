# Vietnix-trainning Report task 3  


##  1. LAMP stack:   
![LAMP stack](images/lamp_stack.jpeg)  
- LAMP là viết tắt của Linux, Apache, MySQL và PHP. Các thành phần này, được sắp xếp theo các lớp hỗ trợ lẫn nhau, tạo thành các stack phần mềm.  
- Linux: là lớp đầu tiên trong stack. Hệ điều hành này là cơ sở nền tảng cho các lớp phần mềm khác.  
- Apache đóng vai trò một HTTP server dùng để xử lý các yêu cầu gửi tới máy chủ.  
- Mysql là cơ sở dữ liệu để lưu trữ mọi thông tin trên website.  
- PHP sẽ đóng vai trò middleware để liên kêt với CSDL và xử lý các tác vụ logic.  

### 1.1 Cài apache web server trên centOS 7
![apache](images/apache.png)  

- Cài apache trên centOS 7: sudo yum install httpd  
- Sau khi cài apache sử dụng lệnh để start web server này: systemctl start httpd
- Sử dụng lệnh để kiểm tra trạng thái của service apache: systemctl status httpd  

![apache status](images/httpd-status.png)  

### 1.2 Cài mariaDB trên centOS 7

![maria data base](images/mariaDB.png)

- Cài mariaDB và các dependencies trên repo mặc định của centOS 7 bằng lệnh: sudo yum install mariadb-server  
- Sử dụng lệnh wget để fecth file setip trên trang chủ của mariaDB: wget https://downloads.mariadb.com/MariaDB/mariadb_repo_setup  
- Phân quyền thực thi cho file set up vừa tải về: chmod +x mariadb_repo_setup  
- Chạy file setup mariaDB: sudo ./mariadb_repo_setup  
- Sử dụng lệnh để start service mariaDB: systemctl start mariaDB  
- Sử dụng lệnh để kiếm tra trạng thái của mariaDB: systemctl status mariaDB  

![database status status](images/mariadb-status.png)   

### 1.3 Tạo user và database để chuẩn bị cho việc thực hiện cài đặt mã nguồn wordpress  

- Đăng nhập vào mariaDB sử dụng lệnh: mysql -u root -p sau đó nhập mật khẩu  

![sign in mariadb](images/sign-in-mariadb.png)   

- Tạo database cho wordpress sử dụng lệnh: CREATE DATABASE wordpress;  

![create database for wordpress](images/create-wordpressdb.png)  

- Tạo user để sử dụng database vừa tạo:  CREATE USER wordpressuser@local IDENTIFIED BY '123456';
  
![create user for wordpress](images/create-wordpress-user.png)  

- Để user vừa tạo có quyền ghi, đọc, tạo bảng,....  trên database wordpress thì ta tiến hành phân quyền sử dụng lệnh:  GRANT ALL PRIVILEGES ON wordpres.* TO wordpressuser@localhost IDENTIFIED BY '123456';  

![grant access for user wordpressuser](images/grant-user-privilege .png)  

### 1.4 Cài đặt mã nguồn wordpress  
- Vì wordpres được viết trên ngôn ngữ PHP nên trước khi tải code của wordpress ta phải tiến hành cài php trên máy centOS 7.  

![install PHP](images/install-php.png)  

- Sau khi cài đặt sử dụng lệnh: php -v để kiểm tra đã cài đặt thành công hay chưa.  
- Sau khi cài thành công PHP, tiếp tục tải code của wordpress sử dụng wget: wget http://wordpress.org/latest.tar.gz   

![wget wordpress code](images/wget-wordpress.png)  

- Giải nén file vừa tải về sử dụng: tar -xvzf latest.tar.gz  
 
![extract wordpress code](images/extract-wp.png)    

- Sau khi giải nén, tiến hành chuyển code wordpres vừa giải nén sang thư mục /var/www/html của apache sử dụng lệnh: sudo rsync -avP ~/wordpress/ /var/www/html/   

![transfer wordpress code](images/transfer-wp-to-apache.png)    

- Tạo thư mục để upload cho wordpress: mkdir /var/www/html/wp-content/uploads  
- Phân quyền sở hữu toàn bộ thư mục /var/www/html cho user apache bằng lệnh: sudo chown -R apache:apache /var/www/html/*  

![chown apache user](images/chown-wp-user.png)  

### 1.5 Cấu hình wordpress  

- Copy file cấu hình wordpress mẫu sang file cấu hình thật:  cp wp-config-sample.php wp-config.php  
- Sử dụng editor gedit để cấu hình database cho wordpress: sudo gedit wp-config.php  
- Số 1: tên database wordpress ta đã tạo ở bước 1.3  
- Số 2: tên user để thực hiện việc tương tác với database wordpress  
- Số 3: mật khẩu của usẻr wordpressuser.

![config database for wordpress](images/config-wp.png)  

- Restart lại apache sử dụng lệnh: systemctl restảt httpd  
- Sau khi restart ta mở browser và truy cập vào đường dẫn: http://localhost browser sẽ tự redirect chúng ta sang trang cài đặt của wordpress  
- Chọn ngôn ngữ sử dụng:  

![config language for wordpress](images/config-wp-2.png)  

- Nhập thông tin website, username và password admin để chỉnh sửa trang web sau này, sau đó chọn install wordpress:  

![config wordpress](images/config-wp-3.png)  

- Sau khi tạo xong username và password admin, ta sẽ đăng nhập vào web admin của wordpress bằng username và password vừa tạo  
- Giao diện của trang admin:  

![wordpress admin page](images/wp-admin.png)  

- Giao diện của trang chính:  

![wordpress main page](images/wp-main-page.png)  

- Để domain trên localhost được đẹp hơn ta cấu hình file /etc/hosts sử dụng gedit: sudo gedit /etc/hosts

![config domain for localhost](images/config-domain.png)  

- Sau khi cấu hình xong ta restart lại service httpd sử dụng lệnh: systemctl restart httpd  
- Sử dụng browser truy cập vào tên ta vừa sửa ở file /etc/hosts  

![chage domain](images/change-ip.png)  

