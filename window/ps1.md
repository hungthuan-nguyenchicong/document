# file.ps1
```bash
# Cách mở an toàn nhất: Chỉ định thư mục và bắt đầu shell mặc định
# Tham số "~" tương đương với việc bảo WSL: "Mở tại Home"
# $arguments = '-e bash -c "cd ~; ls; exec bash"'
# $arguments = @("-e", "bash", "-i", "-c", "cd ~; ls; exec bash")
# Kiểm tra nếu VS Code cần mở (bỏ dấu # ở dưới)
# Start-Process "code" -ArgumentList "."
#$commenabc = "cd ~; ls; cd git; ls; exec bash"
$pass = "mat_khau"
$sudo_init = "echo '$pass' | sudo -S -v;"
$cm1 = "cd ~;"
$sudo = "sudo ls;"
$cm2 = "cd git; sudo ls;"
$exec = "exec bash"
$comment = $sudo_init + $cm1 + $cm2 + $sudo + $exec
# Dùng nháy kép ở ngoài để $commenabc được thực thi
# Dùng nháy đơn ở trong để bao bọc chuỗi lệnh cho Linux
$arguments = "-e bash -i -c `"$comment`""
#$arguments = "-e bash -i -c `"$commenabc`""

# Chạy WSL thu nhỏ
Start-Process "wsl.exe" -ArgumentList $arguments -WindowStyle Minimized

# Write-Host "WSL đang mo tai thu muc Home va da duoc thu nho." -ForegroundColor Green
```