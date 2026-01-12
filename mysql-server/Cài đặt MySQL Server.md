# Cài đặt MySQL Server
> sudo apt update

> sudo apt install mysql-server

> sudo mysql_secure_installation

> sudo systemctl status mysql

# Cách kết nối và sử dụng cơ bản
> sudo mysql -u root -p

# Một số lệnh SQL cơ bản để chuẩn bị cho WordPress:

```sql
-- Tạo database mới
CREATE DATABASE wp_database;

-- Tạo user mới và đặt mật khẩu
CREATE USER 'wp_user'@'localhost' IDENTIFIED BY 'mat_khau_manh';

-- Cấp quyền cho user quản lý database
GRANT ALL PRIVILEGES ON wp_database.* TO 'wp_user'@'localhost';

-- Lưu thay đổi
FLUSH PRIVILEGES;

-- Thoát
EXIT;
```

## php-mysql
>sudo apt install php8.3-mysql