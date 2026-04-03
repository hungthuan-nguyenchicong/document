1. Truy vấn kiểm tra (Read-only)

```sql
-- Kiểm tra bộ 3 size cơ bản (Thumbnail, Medium, Large)
SELECT * FROM wp_options
WHERE option_name IN ('thumbnail_size_w', 'thumbnail_size_h', 'thumbnail_crop', 'medium_size_w', 'medium_size_h', 'large_size_w', 'large_size_h');

-- Kiểm tra "ông thần ẩn mình" Medium Large (768px)
SELECT * FROM wp_options WHERE option_name = 'medium_large_size_w';

-- Kiểm tra tất cả các option có chứa chữ "size" cho chắc chắn
SELECT option_id, option_name, option_value FROM wp_options WHERE option_name LIKE '%size_%';
```

2. Truy vấn cập nhật (Update)

```sql
-- Thiết lập Thumbnail 400x400 và bật chế độ Crop (1)
UPDATE wp_options SET option_value = '400' WHERE option_name IN ('thumbnail_size_w', 'thumbnail_size_h');
UPDATE wp_options SET option_value = '1' WHERE option_name = 'thumbnail_crop';

-- Vô hiệu hóa Medium và Large (Set về 0)
UPDATE wp_options SET option_value = '0' WHERE option_name IN ('medium_size_w', 'medium_size_h', 'large_size_w', 'large_size_h');

-- Vô hiệu hóa Medium Large (Kẻ chuyên sinh file 768px)
UPDATE wp_options SET option_value = '0' WHERE option_name = 'medium_large_size_w';
```

4. Lệnh Terminal Docker hỗ trợ

```bash
# Vào MySQL console nhanh (Thay user/pass/db tương ứng)
docker exec -it mysql-shared mysql -uwp_admin -pwp_password wordpress_db

# Ép WordPress cắt lại toàn bộ ảnh cũ theo cấu hình mới (Cần WP-CLI)
docker exec -it wp-app-test wp media regenerate --yes --allow-root
```
