# file.bat
```bash
@echo off
:: Chạy PowerShell của Windows để mở WSL ở trạng thái thu nhỏ ngay từ đầu
::powershell -command "Start-Process wsl -ArgumentList '-e bash -c \"cd ~; exec bash\"' -WindowStyle Minimized"

::@echo off
:: File .bat này gọi lệnh PowerShell chạy ngầm
::powershell -WindowStyle Hidden -Command "$p='chua chay'; $c=\"echo '$p' | sudo -S -v; cd ~; ls; exec bash\"; Start-Process wsl -ArgumentList '-e', 'bash', '-i', '-c', $c -WindowStyle Minimized"

powershell -command "Start-Process wsl -ArgumentList '-e bash -i -c \"cd ~/git/learn-laravel; code learn-laravel.code-workspace ;composer run dev; exec bash\"' -WindowStyle Minimized"

::@echo off

```