# ví dụ
Tuyệt vời\! Tôi sẽ hướng dẫn bạn cách tạo một cấu trúc dự án cơ bản sử dụng `composer.json` và tệp `bootstrap.php` để thiết lập môi trường cho nhiều điểm vào.

## 📁 Cấu trúc thư mục cơ bản

Đây là cấu trúc thư mục mà chúng ta sẽ hướng tới:

```
/my-project
├── app/
│   └── src/           <- Nơi chứa mã nguồn chính (Namespace: App\)
│       └── Greeter.php
├── config/            <- Chứa các tệp cấu hình
├── public/            <- Điểm vào Web (http/s)
│   └── index.php
├── cli/               <- Điểm vào CLI (dòng lệnh)
│   └── process_queue.php
├── vendor/            <- Thư viện Composer
├── bootstrap.php      <- Tệp khởi tạo chung
└── composer.json      <- Cấu hình dự án
```

## 1\. Thiết lập `composer.json`

Chúng ta định nghĩa namespace `App\` cho thư mục `app/src/` và thêm một thư viện cơ bản (ví dụ: `monolog/monolog` để ghi log).

```json
{
    "name": "cong/basic-project",
    "description": "Basic PHP project structure with bootstrap.",
    "type": "project",
    "autoload": {
        "psr-4": {
            "App\\": "app/src/"
        }
    },
    "require": {
        "php": ">=8.0",
        "monolog/monolog": "^3.0"  
    },
    "license": "MIT"
}
```

Sau khi tạo file này, bạn chạy lệnh sau trong thư mục gốc của dự án:

```bash
composer install
```

Lệnh này sẽ tạo ra thư mục `vendor/` và file `vendor/autoload.php`.

## 2\. Tạo Tệp `bootstrap.php` (Bộ khởi tạo)

Tệp này là nơi bạn thiết lập những thứ cần thiết cho mọi hoạt động của ứng dụng (web hay CLI).

**Tệp: `bootstrap.php`**

```php
<?php
/**
 * Tệp Khởi tạo Ứng dụng (Bootstrap)
 * Mục đích: Thiết lập môi trường, tải Autoloader và cấu hình cơ bản.
 */

// 1. Tải Composer Autoloader
// Đường dẫn: /vendor/autoload.php
require __DIR__ . '/vendor/autoload.php';

// 2. Định nghĩa các hằng số hữu ích
define('ROOT_PATH', __DIR__);
define('CONFIG_PATH', ROOT_PATH . '/config');
define('APP_PATH', ROOT_PATH . '/app');

// 3. Thiết lập các cấu hình cơ bản (Ví dụ: Thiết lập Timezone)
date_default_timezone_set('Asia/Ho_Chi_Minh');

// 4. Thiết lập hệ thống ghi Log (Ví dụ với Monolog)
use Monolog\Logger;
use Monolog\Handler\StreamHandler;

$log = new Logger('AppLog');
$log->pushHandler(new StreamHandler(ROOT_PATH . '/app.log', Logger::DEBUG));

// Trả về một đối tượng quan trọng (ví dụ: Logger) hoặc Container
return [
    'logger' => $log
];
```

## 3\. Tạo Mã nguồn (Class)

**Tệp: `app/src/Greeter.php`**

```php
<?php

namespace App;

class Greeter {
    public function sayHello(string $name): string {
        return "Xin chào, {$name} từ class đã được Autoload!";
    }
}
```

## 4\. Các Điểm vào (Entry Points)

### A. Điểm vào Web (HTTP)

**Tệp: `public/index.php`**

```php
<?php

// 1. Yêu cầu tệp khởi tạo
$app_config = require __DIR__ . '/../bootstrap.php';

// 2. Sử dụng các dịch vụ đã thiết lập trong bootstrap
$logger = $app_config['logger'];
$logger->info('Web request started.');

// 3. Sử dụng Class đã được Autoload
use App\Greeter;

$greeter = new Greeter();
$message = $greeter->sayHello("Thế giới Web");

header('Content-Type: text/plain');
echo $message;
```

*Lưu ý:* Đường dẫn `../bootstrap.php` là tương đối từ `public/` lên thư mục gốc.

### B. Điểm vào CLI (Dòng lệnh)

**Tệp: `cli/process_queue.php`**

```php
<?php
// Tệp này sẽ chạy qua dòng lệnh: php cli/process_queue.php

// 1. Yêu cầu tệp khởi tạo
$app_config = require __DIR__ . '/../bootstrap.php';

// 2. Sử dụng các dịch vụ đã thiết lập trong bootstrap
$logger = $app_config['logger'];
$logger->info('CLI worker started.');

// 3. Sử dụng Class đã được Autoload
use App\Greeter;

$greeter = new Greeter();
$message = $greeter->sayHello("Hệ thống CLI");

echo "--- CLI Output ---\n";
echo $message . "\n";
$logger->info('CLI worker finished successfully.');
```

*Lưu ý:* Đường dẫn `../bootstrap.php` là tương đối từ `cli/` lên thư mục gốc.

**Kết luận:** Với `bootstrap.php` này, bạn đảm bảo rằng dù chạy bằng web server hay dòng lệnh CLI, mọi class và dependency đều được thiết lập chính xác chỉ với một dòng `require` duy nhất\!

Bạn có muốn tôi hướng dẫn bạn cách sử dụng mục `files` trong `composer.json` để tự động tải các file chứa hàm (function) toàn cục không?