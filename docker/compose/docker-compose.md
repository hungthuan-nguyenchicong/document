```yml
services:
  web-test:
    image: nginx:alpine
    container_name: nginx-compose-test
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html:ro
    restart: always
```

```yml
services:
  # Container xử lý PHP
  php-handler:
    image: php:8.3-fpm-alpine
    container_name: php-fpm-container
    volumes:
      - ./html:/var/www/html

  # Container Web Server
  web-test:
    image: nginx:alpine
    container_name: nginx-container
    ports:
      - "8080:80"
    volumes:
      - ./html:/var/www/html
      - ./default.conf:/etc/nginx/conf.d/default.conf:ro
    depends_on:
      - php-handler
```

## test

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
