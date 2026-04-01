# Tạo thư mục nếu chưa có

mkdir -p wp-data

# Cấp quyền cho user của bạn (ID 1000) sở hữu thư mục này

sudo chown -R 1000:1000 wp-data

# Cấp quyền ghi đầy đủ

chmod -R 755 wp-data
