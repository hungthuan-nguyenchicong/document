# Tạo Khóa SSH Mới
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
# Khởi động SSH Agent
```bash
eval "$(ssh-agent -s)"
```
## Thêm Khóa
```bash
ssh-add ~/.ssh/id_ed25519
```
# Đăng ký Khóa Công khai lên Dịch vụ Git (Ví dụ: GitHub)
## Sao chép Khóa Công khai
```bash
cat ~/.ssh/id_ed25519.pub
```
# Dán Khóa lên GitHub
Đăng nhập vào GitHub.

Truy cập Settings (Cài đặt) (thường là biểu tượng ảnh đại diện của bạn ở góc trên bên phải).

Chọn SSH and GPG keys (Khóa SSH và GPG) ở menu bên trái.

Nhấn New SSH key (Khóa SSH mới)

## kiem tra
```bash
ssh -T git@github.com
```
