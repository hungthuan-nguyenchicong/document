> docker run --rm mysql:oracle mysqld --version

> docker rmi mysql:oracle

> image: mysql:8.4-oracle

> docker pull mysql:8.4-oracle

```yml
services:
  db-wp:
    image: mysql:8.4-oracle
    container_name: mysql-wp-test
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
    volumes:
      - db_storage:/var/lib/mysql

  wordpress:
    image: wordpress:fpm-alpine
    container_name: wp-app-test
    environment:
      WORDPRESS_DB_HOST: db-wp
      WORDPRESS_DB_USER: ${MYSQL_USER}
      WORDPRESS_DB_PASSWORD: ${MYSQL_PASSWORD}
      WORDPRESS_DB_NAME: ${MYSQL_DATABASE}
    volumes:
      - ./wp-data:/var/www/html
    depends_on:
      - db-wp

  nginx-wp:
    image: nginx:alpine
    container_name: nginx-wp-test
    ports:
      - "${WP_HOST_PORT}:80"
    volumes:
      - ./wp-data:/var/www/html
      - ./nginx-wp.conf:/etc/nginx/conf.d/default.conf:ro
    depends_on:
      - wordpress

volumes:
  db_storage:
```

# .env

# MySQL Config

MYSQL_ROOT_PASSWORD=super_secret_root_pass
MYSQL_USER=wp_admin
MYSQL_PASSWORD=wp_password
MYSQL_DATABASE=wordpress_db

# Port Config

WP_HOST_PORT=8081
MYSQL_HOST_PORT=3306

> docker compose config
