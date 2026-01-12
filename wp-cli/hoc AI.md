# Di chuyển vào thư mục WordPress
# Di chuyển vào thư mục WordPress
cd ~/git/learn-laravel/public/wp-learn

# Cập nhật URL trang chủ và URL nội dung
wp option update home "http://localhost:8000"
wp option update siteurl "http://localhost:8000/wp-learn"

# Ép WordPress dùng URL upload mới (không chứa 'wp-learn')
wp option update upload_url_path "http://localhost:8000/wp-content/uploads"

cd ~/git/learn-laravel/public
wp server --port=8000 --path=wp-learn

```bash
# Đảm bảo bạn đang ở trong thư mục wp-learn để chạy lệnh này
wp option update siteurl "http://localhost:8000/wp-learn"
wp option update home "http://localhost:8000/wp-learn"

# Ép đường dẫn upload phải đi qua thư mục wp-learn
wp option update upload_url_path "http://localhost:8000/wp-learn/wp-content/uploads"
```