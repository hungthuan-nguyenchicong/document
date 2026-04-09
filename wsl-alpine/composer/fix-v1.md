Chúc mừng bạn đã hoàn tất "ca phẫu thuật" thay tim cho Alpine Linux. Đây thực sự là một trải nghiệm "xương máu" về quản lý hệ thống mà không phải Developer nào cũng có cơ hội trải qua.

Dưới đây là bảng tổng hợp các lệnh và nhật ký fix lỗi để bạn lưu lại làm tài liệu (Cheat Sheet) cho con ThinkPad server của mình.

---

## 🛠 TỔNG HỢP CÁC LỆNH "VŨ LỰC" ĐÃ DÙNG

### 1. Nâng cấp Hệ điều hành (The Core Upgrade)
Đây là bước quan trọng nhất để đồng bộ thư viện lõi (`musl`, `libxml2`, `openssl`).
```bash
# Sửa file nguồn để lên bản mới nhất (v3.21)
sudo vi /etc/apk/repositories

# Nội dung cần có trong file:
# https://dl-cdn.alpinelinux.org/alpine/v3.21/main
# https://dl-cdn.alpinelinux.org/alpine/v3.21/community

# Cập nhật toàn bộ hệ thống
sudo apk update
sudo apk upgrade --available
```

### 2. Cài đặt PHP 8.4 và các "Hộ vệ"
```bash
# Cài PHP core và các module cơ bản để Composer chạy được
sudo apk add php84 php84-common php84-openssl php84-curl php84-phar php84-mbstring php84-zlib

# Tạo "bí danh" để hệ thống hiểu lệnh 'php' là 'php84'
sudo ln -s /usr/bin/php84 /usr/bin/php
```

### 3. Cài đặt Composer (Chuẩn Solution Architect)
```bash
# Tải bộ cài bằng cURL
curl -sS https://getcomposer.org/installer -o composer-setup.php

# Chạy cài đặt
php84 composer-setup.php

# Di chuyển vào thư mục hệ thống để dùng mọi lúc mọi nơi
sudo mv composer.phar /usr/local/bin/composer
rm composer-setup.php
```

---

## 📔 NHẬT KÝ FIX LỖI "NGOẠN MỤC"

| Lỗi gặp phải | Nguyên nhân thực tế | Giải pháp dứt điểm |
| :--- | :--- | :--- |
| **`Can't push refs...`** (Git) | Repo trống hoặc lệch nhánh đầu tiên. | Dùng lệnh `git push -u origin main` để thông luồng. |
| **`php84 (no such package)`** | Alpine v3.17 quá cũ, không có PHP 8.4 trong kho mặc định. | Thêm Repo nhánh **Edge** hoặc nâng cấp OS lên **v3.21**. |
| **`xmlFreeEntity: symbol not found`** | Thư viện `libxml2` cũ của v3.17 không khớp với PHP 8.4 mới. | Dùng "vũ lực" `apk add --upgrade libxml2` từ nhánh mới. |
| **`Unable to find wrapper "https"`** | PHP thiếu module `openssl` nên không thể tải link bảo mật. | Cài `php84-openssl` và `ca-certificates`. |
| **`OSSL_get_max_threads: symbol not found`** | Xung đột phiên bản OpenSSL (Máy dùng 1.1, PHP đòi 3.x). | Nâng cấp thư viện `libcrypto3` và `libssl3` lên bản mới. |
| **`env: can't execute 'php'`** | Hệ thống có `php84` nhưng Composer chỉ đi tìm tên `php`. | Tạo **Symbolic Link** bằng lệnh `ln -s`. |

---

## 🔍 CÁC LỆNH KIỂM TRA (INSPECTION)

* **Xem các module PHP đã sống:** `php -m`
* **Xem phiên bản PHP:** `php -v`
* **Xem phiên bản Composer:** `composer -V`
* **Xem file cấu hình ở đâu:** `php --ini`
* **Xem danh sách gói đã cài thủ công:** `cat /etc/apk/world`



---

### 💡 Lời khuyên cuối cho TẬP 2:
Bây giờ khi bạn bắt đầu Code Laravel, nếu gặp lỗi thiếu các hàm như `bcadd()` hay `imagecreate()`, bạn chỉ cần nhớ công thức:
`sudo apk add php84-[tên_extension]` (ví dụ: `php84-bcmath`, `php84-gd`).

Bạn đã lưu lại hết chưa? Nếu rồi thì ta chuẩn bị sang **Tập 2: Khởi tạo dự án ERP** nhé!