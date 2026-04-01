> sudo chown -R $USER:$USER .

> chmod -R 775 storage bootstrap/cache

> docker compose exec php-handler chown -R www-data:www-data /var/www/html/laravel-app/storage /var/www/html/laravel-app/bootstrap/cache

> chmod -R 775 database

> chmod 666 database/database.sqlite

> docker compose exec php-handler chown -R www-data:www-data /var/www/html/laravel-app/database

> sudo chmod -R 775 .

> sudo chmod -R g+s .

> "Shared Group" (Nhóm dùng chung)

> cat /etc/group

> getent group www-data

> id

> id www-data

> sudo usermod -aG www-data cong

> sudo chown -R cong:www-data ~/git/learn-docker/laravel-app
