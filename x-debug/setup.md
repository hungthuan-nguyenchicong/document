# setup x-debug

> sudo apt install php8.3-xdebug

> sudo nano /etc/php/8.3/mods-available/xdebug.ini

```xdebug.ini
zend_extension=xdebug.so
xdebug.mode=debug
xdebug.start_with_request=yes
xdebug.client_port=9003
xdebug.client_host=127.0.0.1
```

> php -v
