# cai dat
alpine.exe config --default-user root
su -
# 1. Cài đặt sudo (bắt buộc phải có cái này mới sướng được)
apk update && apk add sudo

# 2. Tạo thư mục cấu hình nếu chưa có
mkdir -p /etc/sudoers.d

# 3. Cấp quyền "miễn mật khẩu" vĩnh viễn cho user cong
echo 'cong ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/cong

# 4. Phân quyền chuẩn cho file (không có bước này sudo sẽ lỗi)
chmod 0440 /etc/sudoers.d/cong

apk add docker docker-cli-compose

dockerd

# Chạy daemon và đẩy nó vào chạy ngầm
dockerd > /dev/null 2>&1 &

docker ps

apk add openrc

rc-update add docker default

vi /etc/profile
# Kích hoạt OpenRC và các dịch vụ đi kèm (như docker)
openrc default > /dev/null 2>&1

vi /etc/profile

# Sửa trong /etc/profile
if ! pgrep -x "dockerd" > /dev/null; then
    sudo dockerd > /dev/null 2>&1 &
    # Đợi 1s để socket kịp tạo ra
    sleep 1
    # Đảm bảo file socket luôn thuộc nhóm docker để bạn dùng được
    sudo chgrp docker /var/run/docker.sock > /dev/null 2>&1
fi

# Thêm user cong vào nhóm docker
addgroup cong docker

# Sửa quyền file socket để nhóm docker có thể dùng được
chown root:docker /var/run/docker.sock
