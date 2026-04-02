# 1. Đảm bảo bạn đang ở đúng thư mục dự án

cd ~/git/learn-docker

# 2. Cấp quyền sở hữu cho chính bạn (user 1000) trên máy host

# Vì bạn dùng Bind Mount, quyền ở máy host sẽ phản chiếu vào container

sudo chown -R $USER:$USER laravel-app/storage laravel-app/bootstrap/cache shared-uploads

# 3. Cấp quyền ghi cho Group (775) để PHP-FPM có thể làm việc

chmod -R 775 laravel-app/storage laravel-app/bootstrap/cache shared-uploads

# Đứng từ thư mục learn-docker (nơi có file docker-compose.yml)

docker compose exec laravel-php php artisan config:clear

docker compose exec laravel-php php artisan cache:clear

docker compose exec laravel-php php artisan optimize:clear

docker compose exec --user root laravel-php docker-php-ext-install pdo_mysql

docker compose restart laravel-php

docker exec -it mysql-shared mysql -uwp_admin -pwp_password wordpress_db

docker compose down -v

# user: "1000:1000"

> cấp quyền user: "1000:1000"
