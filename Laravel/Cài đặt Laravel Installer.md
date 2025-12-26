# cai dat
sudo apt install php8.3-zip unzip -y

sudo apt install php-common php-bcmath php-curl php-mbstring php-xml php-zip php-sqlite3 -y

sudo apt install php-sqlite3 -y
# Cài đặt Laravel Installer
composer create-project laravel/laravel .
# Cấu hình biến môi trường (PATH)
nano ~/.bashrc
# Dán dòng sau vào cuối file:
export PATH="$PATH:$HOME/.config/composer/vendor/bin"
# Áp dụng thay đổi ngay lập tức
source ~/.bashrc