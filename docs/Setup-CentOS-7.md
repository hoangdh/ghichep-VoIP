## Hướng dẫn cài đặt tổng đài FreePBX

### Menu

- 1. Chuẩn bị cài đặt
- 2. Các bước tiến hành
- 2.1 Thiết lập hệ thống
- 2.2 Cài đặt các gói hỗ trợ và FreePBX
- 3. Tham khảo

## 1. Chuẩn bị cài đặt

### Mô hình cài đặt

[!mohinh](/images/voip-mohinh.png)

### Bảng phân hoạch IP

[!IPPlanning](/images/IPPlanning.png)

Link docs: https://goo.gl/qApSCc

## 2. Các bước tiến hành

### 2.1 Thiết lập hệ thống

**Chú ý**: Các câu lệnh phải chạy với quyền cao nhất là `root`.

- **Bước 1:** Tắt SELinux

	Chỉnh sửa file cấu hình SELINUX tại: */etc/sysconfig/selinux*

	```
	sed -i 's/\(^SELINUX=\).*/\SELINUX=disabled/' /etc/sysconfig/selinux
	sed -i 's/\(^SELINUX=\).*/\SELINUX=disabled/' /etc/selinux/config
	```

	Tắt tức thời bằng lệnh:

	```
	setenforce 0
	```

	Kiểm tra lại bằng lệnh

	```
	SELinux status:                 enabled
	SELinuxfs mount:                /sys/fs/selinux
	SELinux root directory:         /etc/selinux
	Loaded policy name:             targeted
	Current mode:                   permissive
	Mode from config file:          disable
	Policy MLS status:              enabled
	Policy deny_unknown status:     allowed
	Max kernel policy version:      28
	```

	- **Bước 2:** Cập nhật hệ thống

	```
	yum -y update
	yum -y groupinstall core base "Development Tools"
	```

- **Bước 3:** Cài đặt một số gói đi kèm

	```
	yum -y install lynx mariadb-server mariadb php php-mysql php-mbstring tftp-server \
	httpd ncurses-devel sendmail sendmail-cf sox newt-devel libxml2-devel libtiff-devel \
	audiofile-devel gtk2-devel subversion kernel-devel git php-process crontabs cronie \
	cronie-anacron wget vim php-xml uuid-devel sqlite-devel net-tools gnutls-devel php-pear unixODBC mysql-connector-odbc
	```

- **Bước 4:** Cài đặt Lagacy Pear (PHP)

	```
	pear install Console_Getopt
	```
	
- **Bước 5:** Cấu hình firewalld cơ bản
	Nếu sử dụng firewalld, bạn phải mở port 80 để có thể truy cập vào trang quản trị FreePBX.
	
	```	
	firewall-cmd --zone=public --add-port=80/tcp --permanent
	firewall-cmd --reload
	```
	
- **Bước 6:** Khởi động MariaDB

	```		
	systemctl enable mariadb.service
	systemctl start mariadb
	```
	
	Cài đặt MariaDB cơ bản bằng lệnh:
	
	```		
	mysql_secure_installation
	```
	
	Vui lòng không đặt password 
	
- **Bước 7:** Khởi động HTTPD - APACHE

### 2.2 Cài đặt các gói hỗ trợ và FreePBX

- **Bước 1:** Cài đặt gói bộ trợ cho Google Voice (Tùy chọn)
- **Bước 2:** Thêm user cho Asterisk
- **Bước 3:** Cài đặt và cấu hình Asterisk
- **Bước 4:** Biên dịch và cấu hình pjproject
- **Bước 5:** Biên dịch và cấu hình jansson 
- **Bước 6:** Biên dịch và cấu hình Asterisk
- **Bước 7:** Cài đặt các file âm thanh của Asterisk
- **Bước 8:** Cài đặt và cấu hình FreePBX 

## 3. Tham khảo

- https://wiki.freepbx.org/display/FOP/Installing+FreePBX+13+on+CentOS+7