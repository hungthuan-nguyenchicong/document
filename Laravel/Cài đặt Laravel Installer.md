# cai dat
sudo apt install php8.3-zip unzip -y
# Cài đặt Laravel Installer
composer global require laravel/installer
# Cấu hình biến môi trường (PATH)
nano ~/.bashrc
# Dán dòng sau vào cuối file:
export PATH="$PATH:$HOME/.config/composer/vendor/bin"
# Áp dụng thay đổi ngay lập tức
source ~/.bashrc