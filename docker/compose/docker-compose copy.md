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
