# Các Lệnh Composer Cơ BảnLệnhMô tả
dụcomposer require Thêm một thư viện (package) mới vào dự án của bạn và tự động cài đặt nó. Lệnh này sẽ cập nhật cả file composer.json và composer.lock. 

-> composer require monolog/monolog

composer install Đọc file composer.lock và cài đặt tất cả các thư viện cần thiết. Bạn dùng lệnh này khi bắt đầu một dự án mới (ví dụ: clone từ Git) để đảm bảo mọi người dùng cùng một phiên bản thư viện. 

-> composer install

composer update Đọc file composer.json, kiểm tra phiên bản mới nhất của các thư viện (theo ràng buộc bạn định nghĩa) và cập nhật chúng. Sau đó, nó sẽ ghi phiên bản mới vào composer.lock.
-> composer update

composer dump-autoload Tái tạo lại file autoloader. Bạn thường dùng lệnh này khi bạn thêm/thay đổi class mà không cần cài đặt package mới (ví dụ: sau khi chỉnh sửa mục autoload trong composer.json).

-> composer dump-autoload  

composer self-update Cập nhật chính Composer lên phiên bản mới nhất.

-> composer self-update

# Hướng Dẫn Sử Dụng Autoloader (PS-4)
## Khởi tạo file composer.json
composer init
# Chạy lệnh để tạo Autoloader
composer dump-autoload

# Đường dẫn Ánh Xạ Bắt đầu từ đâu?

Đường dẫn ánh xạ (ví dụ: admin/src/ hay public/src/) luôn bắt đầu từ thư mục GỐC của dự án (nơi chứa file composer.json).

```bash
"autoload": {
        "psr-4": {
            // Ánh xạ 1: Mã nguồn Admin
            "App\\Admin\\": "admin/src/", 
            
            // Ánh xạ 2: Mã nguồn Public (Frontend)
            "App\\Public\\": "public/src/"
        }
    },
```

Tên namespace phải khớp với ánh xạ: App\Admin\

```bash
namespace App\Admin; 

class UserController {
    public function __construct() {
        echo "Admin User Controller loaded.\n";
    }
}
```

## Lưu ý quan trọng
Sau khi thêm hoặc thay đổi bất kỳ mục nào trong khối autoload của composer.json, bạn phải chạy lệnh sau để Composer tái tạo lại file vendor/autoload.php:

```bash
composer dump-autoload
```

## Tệp index.php (sử dụng Autoloader):
```bash
<?php
require 'vendor/autoload.php'; // Bắt buộc phải có

// Sử dụng class từ phân vùng Admin
$admin_user = new App\Admin\UserController();

// Sử dụng class từ phân vùng Public
$public_home = new App\Public\HomeController();
```

## // bootstrap.php (Nằm ở thư mục gốc)
```bash
// bootstrap.php (Nằm ở thư mục gốc)
<?php
// 1. Kích hoạt Autoloader
require __DIR__ . '/vendor/autoload.php'; 

// 2. Tải các cấu hình chung, hằng số chung, v.v.
// ...

return [
    // Trả về đối tượng Application hoặc Container (nếu dùng framework)
];
```

## Sau đó, trong mỗi điểm vào, bạn gọi file này:
```bash
// http/public/index.php
require __DIR__ . '/../../bootstrap.php';
// ... code xử lý Web ...
```

```bash
// api/index.php
require __DIR__ . '/../bootstrap.php';
// ... code xử lý API ...
```

# kiểm tra đã cài đặt chưa
```bash
composer global show laravel/installer
```