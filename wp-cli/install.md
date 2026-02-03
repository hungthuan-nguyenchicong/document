# wp core download --path=wpclidemo.dev

> wp core download --path=wordpress-app

> cd wordpress-app

> wp config create --dbhost=127.0.0.1 --dbname=wp_database --dbuser=wp_user --prompt=dbpass

> wp db create

> wp core install --url=127.0.0.1:8080 --title="WP-CLI" --admin_user=wp_user --admin_password=wpcli --admin_email=info@wp-cli.org

> wp option update upload_path uploads

> wp option update upload_url_path /uploads

> wp server

> wp user list

> wp user update 1 --user_login=new_admin_name

> wp user update wp_user --user_pass=newpassword123

> wp user update wp_user --user_email=newemail@example.com
