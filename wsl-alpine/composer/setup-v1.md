sudo apk add composer

sudo apk del composer

apk info

# Thêm repository edge vào lệnh cài đặt
sudo apk add php84 --repository=https://dl-cdn.alpinelinux.org/alpine/edge/community

apk search php

sudo vi /etc/apk/repositories

https://dl-cdn.alpinelinux.org/alpine/edge/main

https://dl-cdn.alpinelinux.org/alpine/edge/community

sudo apk add libxml2 --repository=https://dl-cdn.alpinelinux.org/alpine/edge/main

sudo apk add --upgrade libxml2 --repository=https://dl-cdn.alpinelinux.org/alpine/edge/main

sudo apk add php84-openssl ca-certificates --repository=https://dl-cdn.alpinelinux.org/alpine/edge/community

sudo apk add --upgrade libcrypto3 libssl3 --repository=https://dl-cdn.alpinelinux.org/alpine/edge/main

php84 -m | grep openssl

sudo apk update && sudo apk upgrade --available

sudo apk add curl php84-openssl php84-phar php84-mbstring php84-zlib php84-curl

sudo ln -s /usr/bin/php84 /usr/bin/php

# 1. Tải về thư mục hiện tại
php84 -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

# 2. Chạy trình cài đặt
php84 composer-setup.php

# 3. Xóa file cài đặt cho sạch máy
php84 -r "unlink('composer-setup.php');"

# 4. Đưa nó vào hệ thống để gõ lệnh 'composer' ở bất cứ đâu
sudo mv composer.phar /usr/local/bin/composer

php -m