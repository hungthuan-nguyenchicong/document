server {
listen 80;
index index.php index.html;
root /var/www/html/laravel-app/public; # Mặc định vào Laravel

    # Cấu hình cho WordPress (nằm ở /blog)
    location /blog {
        alias /var/www/html/blog;
        try_files $uri $uri/ /blog/index.php?$args;

        location ~ \.php$ {
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
            fastcgi_pass cms-wp:9000; # Đẩy sang container WordPress
            fastcgi_index index.php;
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $request_filename;
        }
    }

    # Cấu hình mặc định cho Laravel
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass php-handler:9000; # Đẩy sang container Laravel
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

}

## learn

```yml
services:
  # 1. Database dùng chung cho cả Laravel và WordPress
  db-server:
    image: mariadb:10.11
    container_name: mysql-db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root_password
      MYSQL_DATABASE: laravel_db # DB cho Laravel
    volumes:
      - db_data:/var/lib/mysql

  # 2. Container xử lý PHP cho Laravel
  php-handler:
    image: php:8.3-fpm-alpine
    container_name: php-fpm-container
    user: "1000:1000" # Dùng ID của bạn để tránh lỗi permission
    volumes:
      - .:/var/www/html
    depends_on:
      - db-server

  # 3. Container WordPress (Đã có sẵn PHP-FPM bên trong)
  cms-wp:
    image: wordpress:fpm-alpine
    container_name: wordpress-container
    user: "1000:1000"
    environment:
      WORDPRESS_DB_HOST: db-server
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: root_password
      WORDPRESS_DB_NAME: wordpress_db # DB riêng cho WP
    volumes:
      - ./wordpress-app:/var/www/html # Mount vào thư mục riêng
    depends_on:
      - db-server

  # 4. Container Nginx (Điều phối cả 2)
  web-test:
    image: nginx:alpine
    container_name: nginx-container
    ports:
      - "8080:80"
    volumes:
      - .:/var/www/html
      - ./wordpress-app:/var/www/html/blog # Mount code WP vào path /blog để Nginx thấy
      - ./default.conf:/etc/nginx/conf.d/default.conf:ro
    depends_on:
      - php-handler
      - cms-wp

volumes:
  db_data:
```
